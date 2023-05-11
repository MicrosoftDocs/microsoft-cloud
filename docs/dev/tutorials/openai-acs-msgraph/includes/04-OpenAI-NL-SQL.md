<!-- markdownlint-disable MD041 -->

Before getting started, it's important to understand the benefits of incorporating natural language to SQL functionality into applications. By harnessing the capabilities of natural language processing (NLP) technology, you can enable users without SQL expertise to query databases using everyday language, such as plain English. For example, they may enter:

```
Get the the total revenue for all companies in London.
```    

Azure OpenAI Service will convert this query to SQL and you can then use the SQL to return results from the database. As a result, non-technical stakeholders including business analysts, marketers, and executives can more effortlessly retrieve valuable information from databases without grappling with intricate SQL syntax or relying on constrained datagrids and filters. This streamlined approach can boost productivity by eliminating the need for users to seek assistance from technical experts. Finally, by capitalizing on advanced language models like GPT, you can design applications that are not only intuitive but also adaptable to a diverse user base. 

In this exercise, you will:

- Use GPT prompts to convert natural language to SQL.
- Experiment with different GPT prompts.
- Use the generated SQL to query the PostgreSQL database started earlier.
- Return query results.

Let's get started by experimenting with different GPT prompts that can be used to convert natural language to SQL.

### Using the Natural Language to SQL Feature

1. In the [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=2) you started the database, APIs, and application. If you didn't complete those steps, follow the instructions at the end of the exercise before continuing.

1. Go back to the browser (*http://localhost:4200*) and locate the **Custom Query** section of the page below the datagrid.

    :::image type="content" source="../media/openai-custom-query.png" alt-text="Natural language to SQL query.":::

1. Notice that a sample query value is already included: "Get the total revenue for all orders. Group by company and include the city." 

1. Select the **Run Query** button. This will pass the user's natural language query to Azure OpenAI which will convert it to SQL. The SQL query will then be used to query the database and return any potential results.

1. View the terminal window running the APIs and notice it displays the SQL query that is returned from Azure OpenAI.

    :::image type="content" source="../media/nl-sql-terminal-output.png" alt-text="Natural language to SQL query output to the terminal window.":::

### Exploring the Natural Language to SQL Code

[!INCLUDE [Note-Open-Files-VS-Code](./tip-open-files-vs-code.md)]

1. Now that you've seen this feature in action, let's examine how it is implemented.

1. Open the *server/apiRoutes.ts* file and locate the `generatesql` route. This API route is called by the client-side application running in the browser and used to generate SQL from a natural language query. Once the SQL query is retrieved, it's used to query the database and return results. 

1. Notice the following functionality in the function:

    - It accepts a user query parameter which is used in the GPT prompt.
    - It calls a `getSQL()` function to convert natural language to SQL.
    - It passes the generated SQL to a function named `queryDb` that executes the SQL query and returns results from the database.

    ```typescript
    router.post('/generatesql', async (req, res) => {
        const userQuery = req.body.query;

        if (!userQuery) {
            return res.status(400).json({ error: 'Missing parameter "query".' });
        }

        try {
            // Call OpenAI to convert the user query into a SQL query
            const sqlCommandObject = await getSQL(userQuery);

            let result = [];
            // Execute the SQL query
            if (sqlCommandObject) {
                result = await queryDb(JSON.parse(sqlCommandObject));
            }
            res.json(result);
        } catch (e) {
            console.error(e);
            res.status(500).json({ error: 'Error generating or running SQL query.' });
        }
    });
    ```

1. Open the *server/openAI.ts* file in your editor and locate the `getSQL()` function. 

    ```typescript
    async function getSQL(userPrompt: string) : Promise<QueryData> {
        // Get the database schema summary to be used in the prompt
        // This may be created and updated by a process that reads the database schema to keep it in sync
        const dbSchema = await fs.promises.readFile('dbSchema.txt', 'utf8');

        const prompt = `
        PostgreSQL tables, with their properties:

        ${dbSchema}

        User prompt: ${userPrompt}

        Rules:
        - Convert any strings to a PostgreSQL parameterized query value to avoid SQL injection attacks.
        
        Return a JSON object with the SQL query and the parameter values in it. 
        
        Example: { "sql": "", "paramValues": [] }
        `;

        let queryData: QueryData = { sql: '', paramValues: [] };
        try {
            const results = await callOpenAI(prompt);
            if (results) {
                queryData = JSON.parse(results);
                if (isProhibitedQuery(queryData.sql)) { 
                    queryData.sql = '';
                }
            }
        }
        catch (e) {
            console.log(e);
        }

        return queryData;
    }
    ```

1. Let's walk through the key parts of the prompt used in the `getSQL()` function:

    - Table names and columns in the database are defined first. The high-level schema included in the prompt can be found in the *server/db.schema* file. Additional information such as column types and other properties can be added to this file to improve the accuracy of the generated SQL if desired. 

    ```
    - customers (id, company, city, email)
    - orders (id, customer_id, date, total)
    - order_items (id, order_id, product_id, quantity, price)
    - reviews (id, customer_id, review, date, comment)
    ```

    > [!NOTE]
    > In a real-world scenario, the schema would be updated as the database schema changes and only include tables that users are allowed to query using natural language processing. Additionally, it is imperative to plan for proper database security measures to prevent unauthorized access and protect sensitive data.

    - The natural language query created by the end user is embedded into the prompt.
    - A rule is defined to convert any string values to a parameterized query value to avoid SQL injection attacks.
    - An example is given for the type of JSON object to return.

    ```typescript
    const prompt = `
    PostgreSQL tables, with their properties:

    ${dbSchema}

    User prompt: ${userPrompt}

    Rules:
    - Convert any strings to a PostgreSQL parameterized query value to avoid SQL injection attacks.
    
    Return a JSON object with the SQL query and the parameter values in it. 
    
    Example: { "sql": "", "paramValues": [] }
    `;
    ```

1. The `getSQL()` function sends the prompt to a function named `callOpenAI()` which is also located in the *server/openAI.ts* file. The `callOpenAI()` function determines if the Azure OpenAI service or OpenAI service should be called by checking environment variables. If a key, endpoint, and model are available in the environment variables, Azure OpenAI is called.

    ```typescript
    function callOpenAI(prompt: string, temperature = 0) {
        const isAzureOpenAI = OPENAI_API_KEY && OPENAI_ENDPOINT && OPENAI_MODEL;
        if (isAzureOpenAI) {
            return getAzureOpenAICompletion(prompt, temperature);
        }
        else {
            return getOpenAICompletion(prompt, temperature);
        }
    }
    ```

    > [!NOTE]
    > Although we'll focus on Azure OpenAI here, if you only supply an `OPENAI_API_KEY` value in the *.env* file, the application will use OpenAI instead.

1. Locate the `getOpenAICompletion()` function and note that it does the following:

    - Ensures that a valid Azure OpenAI API key, endpoint, ,and model are available.
    - Creates a `url` value that will be used to call Azure OpenAI's REST API and embeds the endpoint, model, and API version values from the environment variables into the URL.
    - Creates a data object that includes `max_token`, `temperature`, and `messages` to send to Azure OpenAI.
        - `max_tokens`: The maximum number of tokens to generate in the completion. The token count of your prompt plus max_tokens can't exceed the model's context length. Older [models](https://learn.microsoft.com/azure/cognitive-services/openai/concepts/models#gpt-3-models-1) have a context length of 2,048 tokens while newer ones support 4,096, 8,192, or even 32,768 tokens depending on the model being used.
        - `temperature`: What sampling temperature to use, between 0 and 2. A higher values means the model will take more risks. Try 0.9 for more creative applications, and 0 for ones with a well-defined answer.
        - `messages`: Represents the messages to generate chat completions for, in the chat format. In this example `messages` is assigned a `role` of `user` and `content` is assigned to the `prompt` parameter value passed into the function. In addition to the `user` role, the roles of `system` and `assistant` are also available to use.
    
        > [!NOTE]
        > You can learn more about these parameters and others in the [reference documentation](/azure/cognitive-services/openai/reference#chat-completions).

    ```typescript
    async function getAzureOpenAICompletion(prompt: string, temperature = 0) : Promise<string> {

        if (!OPENAI_API_KEY || !OPENAI_ENDPOINT || !OPENAI_MODEL) {
            throw new Error('Missing Azure OpenAI API key, endpoint, or model in environment variables.');
        }

        // While it's possible to use the OpenAI SDK (as of today) with a little work, we'll use the REST API directly for Azure OpenAI calls.
        // https://learn.microsoft.com/en-us/azure/cognitive-services/openai/reference
        const url = `${OPENAI_ENDPOINT}/openai/deployments/${OPENAI_MODEL}/chat/completions?api-version=${OPENAI_API_VERSION}`;
        const data = {
            max_tokens: 1024,
            temperature,
            messages: [{ role: 'user', content: prompt }]
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
            let content = '';
            if (completion.choices.length) {
                content = completion.choices[0].message?.content.trim();
                console.log('Output:', content);
            }
            return content;
        }
        catch (e) {
            console.error('Error getting data:', e);
            throw e;
        }
    }
    ```

1. Go back to the `getSQL()` function and add the following rule into the `Rules:` section of the prompt:

    ```
    - Do not allow the SELECT query to return table names, function names, or procedure names.
    ```

1. Comment out the following lines in the `getSQL()` function:

    ```typescript
    // if (isProhibitedQuery(queryData.sql)) { 
    //     queryData.sql = '';
    // }
    ```

1. The API server will automatically rebuild the TypeScript code and restart the server after you save the *openAI.ts* file.

1. Go back to the browser, and enter *Select all table names* into the **Custom Query** input. Select **Run Query**. Are table names displayed? 

1. Enter *Select all function names.* into the **Custom Query** input and select **Run Query** again. Are function names displayed?

1. QUESTION: You added a rule into the GPT prompt that said to not allow table, function, or procedure names, so why did the Azure OpenAI model still return a SQL query to retrieve those values?

    ANSWER: AI may generate unexpected results even if you have specific rules in place. This is a good example of why you need to plan your prompt text carefully, but also plan to add a post-processing step into your code to handle cases where you receive unexpected results.

1. Go back to *server/openAI.ts* and locate the `isProhibitedQuery()` function. This is an example of post-processing code that can be run after Azure OpenAI returns results. Notice that it sets the `sql` property to an empty string if prohibited keywords are returned in the generated SQL query. 

    > [!NOTE]
    > It's important to note that this is only demo code. There may be other prohibited keywords required to cover your specific use cases if you choose to convert natural language to SQL. This is a feature that you must plan for and use with care to ensure that only valid SQL queries are returned and run against the database. In addition to prohibited keywords, you will also need to factor in security as well.

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

1. Go back to *server/openAI.ts* and uncomment the following code in the `getSQL()` function:

    ```typescript
    if (isProhibitedQuery(queryData.sql)) { 
        queryData.sql = '';
    }
    ```

1. Go back to the browser, enter *Select all table names* into the **Custom Query** input again and select the **Run Query** button. Do any table results display? The `isProhibitedQuery()` post-processing code prohibits that type of query from being run against the database.

1. A few final points to consider before moving on to the next exercise:

    - While natural language to SQL can be very powerful, careful planning must go into it to ensure prompts have required rules and that post-processing functionality is included.
    - Security should be a primary concern and built into the planning, development, and deployment process.
    - There are no guarantees that the SQL returned from a natural language query will be efficient. In some cases, additional calls to Azure OpenAI may be required if post-processing rules detect issues with SQL queries.
    - With Azure OpenAI, customers get the security capabilities of Microsoft Azure while running the same models as OpenAI. Azure OpenAI offers private networking, regional availability, and responsible AI content filtering.
    - Learn more about [Data, privacy, and security for Azure OpenAI Service](https://learn.microsoft.com/legal/cognitive-services/openai/data-privacy?context=%2Fazure%2Fcognitive-services%2Fopenai%2Fcontext%2Fcontext).