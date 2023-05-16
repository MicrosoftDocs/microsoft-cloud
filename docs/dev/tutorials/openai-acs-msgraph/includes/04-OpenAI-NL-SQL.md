<!-- markdownlint-disable MD041 -->

Now that you have your Azure OpenAI Service resource setup, let's learn about how it can be used for natural language to SQL scenarios. By harnessing the capabilities of natural language processing (NLP) technology, you can enable users without SQL expertise to query databases using everyday language, such as plain English. For example, they may enter:

```
Get the the total revenue for all companies in London.
```    

Azure OpenAI will convert this query to SQL and you can then use the SQL to return results from the database. As a result, non-technical users including business analysts, marketers, and executives can more easily retrieve valuable information from databases without grappling with intricate SQL syntax or relying on constrained datagrids and filters. This streamlined approach can boost productivity by eliminating the need for users to seek assistance from technical experts. Finally, by capitalizing on advanced language models like GPT (Generative Pre-training Transformer) provided by Azure OpenAI, you can design applications that are not only intuitive, but also adaptable to a diverse user base. 

In this exercise, you will:

- Use GPT prompts to convert natural language to SQL.
- Experiment with different GPT prompts.
- Use the generated SQL to query the PostgreSQL database started earlier.
- Return query results.

Let's start by experimenting with different GPT prompts that can be used to convert natural language to SQL.

### Using the Natural Language to SQL Feature

1. In the [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph?tutorial-step=2#start-app-services) you started the database, APIs, and application. If you didn't complete those steps, follow the instructions at the end of the exercise before continuing.

1. Go back to the browser (*http://localhost:4200*) and locate the **Custom Query** section of the page below the datagrid. Notice that a sample query value is already included: *Get the total revenue for all orders. Group by company and include the city.*

    :::image type="content" source="../media/openai-custom-query.png" alt-text="Natural language to SQL query.":::

1. Select the **Run Query** button. This will pass the user's natural language query to Azure OpenAI which will convert it to SQL. The SQL query will then be used to query the database and return any potential results.

1. View the terminal window running the API server in Visual Studio Code and notice it displays the SQL query that is returned from Azure OpenAI.

    :::image type="content" source="../media/nl-sql-terminal-output.png" alt-text="Natural language to SQL query output to the terminal window.":::

### Exploring the Natural Language to SQL Code

[!INCLUDE [Note-Open-Files-VS-Code](./tip-open-files-vs-code.md)]

> [!NOTE]
> The goal of this exercise is to show what's possible with natural language to SQL functionality and how to get started using it. It's important to discuss if this type of AI is appropriate for your organization before proceeding with any implementation. It's also **imperative to plan for proper prompt rules and database security measures** to prevent unauthorized access and protect sensitive data. This is not a comprehensive tutorial on how to implement natural language to SQL functionality, but rather a starting point to help you understand how it works and how to get started.

1. Now that you've seen the natural language to SQL feature in action, let's examine how it is implemented.

1. Open the *server/apiRoutes.ts* file and locate the `generatesql` route. This API route is called by the client-side application running in the browser and used to generate SQL from a natural language query. Once the SQL query is retrieved, it's used to query the database and return results. 

1. Notice the following functionality in the `generatesql` route:

    - It retrieves the user query value from `req.body.query` and assigns it to a variable named `userQuery`. This value will be used in the GPT prompt.
    - It calls a `getSQL()` function to convert natural language to SQL.
    - It passes the generated SQL to a function named `queryDb` that executes the SQL query and returns results from the database.

    ```typescript
    router.post('/generatesql', async (req, res) => {
        const userQuery = req.body.query;

        if (!userQuery) {
            return res.status(400).json({ error: 'Missing parameter "query".' });
        }

        try {
            // Call Azure OpenAI to convert the user query into a SQL query
            const sqlCommandObject = await getSQL(userQuery);

            let result: any[] = [];
            // Execute the SQL query
            if (sqlCommandObject && !sqlCommandObject.error) {
                result = await queryDb(sqlCommandObject) as any[];
            }
            else {
                result = [ { query_error : sqlCommandObject.error } ];
            }
            res.json(result);
        } catch (e) {
            console.error(e);
            res.status(500).json({ error: 'Error generating or running SQL query.' });
        }
    });
    ```

1. Open the *server/openAI.ts* file in your editor and locate the `getSQL()` function. This function is called by the `generatesql` route and is used to convert natural language to SQL.

    ```typescript
    async function getSQL(userPrompt: string): Promise<QueryData> {
        // Get the high-level database schema summary to be used in the prompt.
        // The db.schema file could be generated by a background process or the 
        // schema could be dynamically retrieved.
        const dbSchema = await fs.promises.readFile('db.schema', 'utf8');

        const systemPrompt = `
        Assistant is a natural language to SQL bot that returns a JSON object with the SQL query and 
        the parameter values in it. The SQL will query a PostgreSQL database.

        PostgreSQL tables, with their properties:

        ${dbSchema}

        Rules:
        - Convert any strings to a PostgreSQL parameterized query value to avoid SQL injection attacks.
        - Return a JSON object with the SQL query and the parameter values in it. 
        
            Example JSON object to return: { "sql": "", "paramValues": [] }
        `;

        let queryData: QueryData = { sql: '', paramValues: [], error: '' };
        try {
            const results = await callOpenAI(systemPrompt, userPrompt);
            console.log('sqlQuery', results);
            if (results && results.startsWith('{') && results.endsWith('}')) {
                queryData = JSON.parse(results);
                if (isProhibitedQuery(queryData.sql)) {
                    queryData.sql = '';
                    queryData.error = 'Prohibited query.';
                }
            }
            else {
                queryData.error = results;
            }
        }
        catch (e) {
            console.log(e);
        }

        return queryData;
    }
    ```

    - You'll notice that a `userPrompt` paramter is passed into the function. The `userPrompt` value is the natural language query entered by the user in the browser. 
    - The `getSQL()` function also includes a `systemPrompt` value that defines the type of AI assistant to be used and rules that should be followed. The `systemPrompt` value is used to help Azure OpenAI understand the database structure and how to generate SQL queries.
    - A function named `callOpenAI()` is called and the `systemPrompt` and `userPrompt` values are passed to it.
    - The results are checked to ensure no prohibited values are included in the generated SQL query. If prohibited values are found, the SQL query is set to an empty string.

1. Let's walk through the system prompt in more detail:

    ```typescript
    const systemPrompt = `
    Assistant is a natural language to SQL bot that returns a JSON object with the SQL query and 
    the parameter values in it. The SQL will query a PostgreSQL database.

    PostgreSQL tables, with their columns:

    ${dbSchema}

    Rules:
    - Convert any strings to a PostgreSQL parameterized query value to avoid SQL injection attacks.
    - Return a JSON object with the SQL query and the parameter values in it. 
    
        Example JSON object to return: { "sql": "", "paramValues": [] }
    `;
    ```

    - The type of AI assistant to be used is defined. In this case a "natural language to SQL bot".
    - Table names and columns in the database are defined. The high-level schema included in the prompt can be found in the *server/db.schema* file and looks like the following.

    ```
    - customers (id, company, city, email)
    - orders (id, customer_id, date, total)
    - order_items (id, order_id, product_id, quantity, price)
    - reviews (id, customer_id, review, date, comment)
    ```

    > [!TIP]
    > In a real-world scenario, the schema could be updated as the database schema changes and only include tables and fields that users are allowed to query using natural language processing.

    - A rule is defined to convert any string values to a parameterized query value to avoid SQL injection attacks.
    - An example is given for the type of JSON object to return.

1. The `getSQL()` function sends the system and user prompts to a function named `callOpenAI()` which is also located in the *server/openAI.ts* file. The `callOpenAI()` function determines if the Azure OpenAI service or OpenAI service should be called by checking environment variables. If a key, endpoint, and model are available in the environment variables then Azure OpenAI is called, otherwise OpenAI is called.

    ```typescript
    function callOpenAI(systemPrompt: string, userPrompt: string, temperature = 0) {
        const isAzureOpenAI = OPENAI_API_KEY && OPENAI_ENDPOINT && OPENAI_MODEL;
        if (isAzureOpenAI) {
            return getAzureOpenAICompletion(systemPrompt, userPrompt, temperature);
        }
        else {
            return getOpenAICompletion(systemPrompt, userPrompt, temperature);
        }
    }
    ```

    > [!NOTE]
    > Although we'll focus on Azure OpenAI throughout this tutorial, if you only supply an `OPENAI_API_KEY` value in the *.env* file, the application will use OpenAI instead. If you choose to use OpenAI instead of Azure OpenAI you may see different results. This is because Azure OpenAI uses the same models as OpenAI, but has additional security and responsible AI features built-in.

1. Locate the `getAzureOpenAICompletion()` function.

    ```typescript
    async function getAzureOpenAICompletion(systemPrompt: string, userPrompt: string, temperature = 0): Promise<string> {

        if (!OPENAI_API_KEY || !OPENAI_ENDPOINT || !OPENAI_MODEL) {
            throw new Error('Missing Azure OpenAI API key, endpoint, or model in environment variables.');
        }

        // While it's possible to use the OpenAI SDK (as of today) with a little work, we'll use the REST API directly for Azure OpenAI calls.
        // https://learn.microsoft.com/en-us/azure/cognitive-services/openai/reference
        const url = `${OPENAI_ENDPOINT}/openai/deployments/${OPENAI_MODEL}/chat/completions?api-version=${OPENAI_API_VERSION}`;
        const data = {
            max_tokens: 1024,
            temperature,
            messages: [
                { role: 'system', content: systemPrompt },
                { role: 'user', content: userPrompt }
            ]
        };

        try {
            const response = await fetch(url, {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                    'api-key': `${OPENAI_API_KEY}`,
                },
                body: JSON.stringify(data),
            });

            const completion = await response.json() as AzureOpenAIResponse;
            let content = (completion.choices[0]?.message?.content?.trim() ?? '') as string;
            console.log('Azure OpenAI Output: \n', content);
            if (content && content.includes('{') && content.includes('}')) {
                content = extractJson(content);
            }
            return content;
        }
        catch (e) {
            console.error('Error getting data:', e);
            throw e;
        }
    }
    ```

    This function does the following:

    - Accepts `systemPrompt`, `userPrompt`, and `temperature` parameters. 
        - `systemPrompt`: Lets the Azure OpenAI model know what role it should play and what rules to follow. 
        - `userPrompt`: User information entered into the application such as natural language or rules that will be used by the model to generate the output.
        - `temperature`: Determines how creative the model should be when generating a response. A higher value means the model will take more risks.
    - Ensures that a valid Azure OpenAI API key, endpoint, ,and model are available.
    - Creates a `url` value that will be used to call Azure OpenAI's REST API and embeds the endpoint, model, and API version values from the environment variables into the URL.
    - Creates a data object that includes `max_token`, `temperature`, and `messages` to send to Azure OpenAI.
        - `max_tokens`: The maximum number of tokens to generate in the completion. The token count of your prompt plus max_tokens can't exceed the model's context length. Older [models](/azure/cognitive-services/openai/concepts/models#gpt-3-models-1) have a context length of 2,048 tokens while newer ones support 4,096, 8,192, or even 32,768 tokens depending on the model being used.
        - `temperature`: What sampling temperature to use, between 0 and 2. A higher values means the model will take more risks. Try 0.9 for more creative applications, and 0 for ones with a well-defined answer.
        - `messages`: Represents the messages to generate chat completions for, in the chat format. In this example two messages are passed in: one for the system and one for the user. The system message defines the overall behavior and rules that will be used, while the user message defines the prompt text provided by the user.
    - Uses `fetch()` to send the `url` and `data` values to Azure OpenAI and processes the response by retrieving the `completion.choices[0].message.content` value. If the response contains the expected results, the code extracts the JSON object from the response and returns it.
    
        > [!NOTE]
        > You can learn more about these parameters and others in the [Azure OpenAI reference documentation](/azure/cognitive-services/openai/reference#chat-completions).

1. Comment out the following lines in the `getSQL()` function:

    ```typescript
    // if (isProhibitedQuery(queryData.sql)) { 
    //     queryData.sql = '';
    // }
    ```

1. Save *openAI.ts*. The API server will automatically rebuild the TypeScript code and restart the server.

1. Go back to the browser and enter *Select all table names from the database* into the **Custom Query** input. Select **Run Query**. Are table names displayed?

1. Go back to the `getSQL()` function in *server/openAI.ts* and add the following rule into the `Rules:` section of the system prompt and then save the file.

    ```
    - Do not allow the SELECT query to return table names, function names, or procedure names.
    ```

1. Go back to the browser and perform the following tasks:

    - Enter *Select all table names from the database* into the **Custom Query** input. Select **Run Query**. Are table names displayed? Notice the output to the screen.
    - Enter *Select all function names from the database.* into the **Custom Query** input and select **Run Query** again. Are function names displayed? Notice the output to the screen.

    The generated SQL query is attempting to return table and function names which is not allowed based on the rule you added to the system prompt.

    > [!NOTE]
    > If you're using OpenAI rather than Azure OpenAI you may see that the model returns a SQL query that includes table names, function names, or procedure names. This can happen even if the rules in the prompt say not to allow those values. This is a good example of why you need to plan your prompt text and rules carefully, but also plan to add a post-processing step into your code to handle cases where you receive unexpected results.

1. QUESTION: You added a rule into the GPT prompt that said to not allow table, function, or procedure names and Azure OpenAI respected it. What if some of the rules you add aren't respected? Why would that happen?

    ANSWER: AI may generate unexpected results even if you have specific rules in place. This is why you need to plan your prompt text and rules carefully, but also plan to add a post-processing step into your code to handle cases where you receive unexpected results.

1. Go back to *server/openAI.ts* and locate the `isProhibitedQuery()` function. This is an example of post-processing code that can be run after Azure OpenAI returns results. Notice that it sets the `sql` property to an empty string if prohibited keywords are returned in the generated SQL query. This ensures that if unexpected results are returned from Azure OpenAI, the SQL query will not be run against the database.

    ```typescript
    function isProhibitedQuery(query: string): boolean {
        const prohibitedKeywords = [
            'insert', 'update', 'delete', 'drop', 'truncate', 'alter', 'create', 'replace',
            'information_schema', 'pg_catalog', 'pg_tables', 'pg_namespace', 'pg_class',
            'table_schema', 'table_name', 'column_name', 'column_default', 'is_nullable',
            'data_type', 'udt_name', 'character_maximum_length', 'numeric_precision',
            'numeric_scale', 'datetime_precision', 'interval_type', 'collation_name',
            'grant', 'revoke', 'rollback', 'commit', 'savepoint', 'vacuum', 'analyze'
        ];
        const queryLower = query.toLowerCase();
        return prohibitedKeywords.some(keyword => queryLower.includes(keyword));
    }
    ```

    > [!NOTE]
    > It's important to note that this is only demo code. There may be other prohibited keywords required to cover your specific use cases if you choose to convert natural language to SQL. This is a feature that you must plan for and use with care to ensure that only valid SQL queries are returned and run against the database. In addition to prohibited keywords, you will also need to factor in security as well.

1. Go back to *server/openAI.ts* and uncomment the following code in the `getSQL()` function. Save the file.

    ```typescript
    if (isProhibitedQuery(queryData.sql)) { 
        queryData.sql = '';
    }
    ```

1. Remove the rule you added earlier into the `getSql()` from the system prompt and save the file.

1. Go back to the browser, enter *Select all table names from the database* into the **Custom Query** input again and select the **Run Query** button. Do any table results display? Even without the rule in place, the `isProhibitedQuery()` post-processing code prohibits that type of query from being run against the database.

1. A few final points to consider before moving on to the next exercise:

    - It's important to discuss if natural language to SQL is the right solution for your use case and if it will provide value to your customers.
    - While natural language to SQL can be very powerful, careful planning must go into it to ensure prompts have required rules and that post-processing functionality is included.
    - Security should be a primary concern and built into the planning, development, and deployment process.
    - There are no guarantees that the SQL returned from a natural language query will be efficient. In some cases, additional calls to Azure OpenAI may be required if post-processing rules detect issues with SQL queries.
    - With Azure OpenAI, customers get the security capabilities of Microsoft Azure while running the same models as OpenAI. Azure OpenAI offers private networking, regional availability, and responsible AI content filtering. Learn more about [Data, privacy, and security for Azure OpenAI Service](/legal/cognitive-services/openai/data-privacy).