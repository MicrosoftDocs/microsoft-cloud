---
title: AI: Natural Language to SQL
description: Learn how to use Azure OpenAI to convert natural language queries into SQL, enabling non-technical users to retrieve database information while implementing proper security controls.
author: DanWahlin
ms.author: dwahlin
ms.date: 06/12/2025
ms.topic: tutorial
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - azure
  - github
ms.custom:
  - fcp
  - team=cloud_advocates

#customer intent: As a developer, I want to integrate Azure OpenAI, Azure Communication Services, and Microsoft Graph/Microsoft Graph Toolkit into a Line of Business application.

---

<!-- markdownlint-disable MD041 -->

# AI: Natural Language to SQL

The quote "Just because you can doesn't mean you should" is a useful guide when thinking about AI capabilities. For example, Azure OpenAI's natural language to SQL feature allows users to make database queries in plain English, which can be a powerful tool to enhance their productivity. However, *powerful* doesn't always mean *appropriate* or *safe*. This exercise will demonstrate how to use this AI feature while also discussing important considerations to keep in mind before deciding to implement it. 

Here's an example of a natural language query that can be used to retrieve data from a database:

```
Get the the total revenue for all companies in London.
```    

With the proper prompts, Azure OpenAI will convert this query to SQL that can be used to return results from the database. As a result, non-technical users including business analysts, marketers, and executives can more easily retrieve valuable information from databases without grappling with intricate SQL syntax or relying on constrained datagrids and filters. This streamlined approach can boost productivity by eliminating the need for users to seek assistance from technical experts. 

This exercise provides a starting point that will help you understand how natural language to SQL works, introduce you to some important considerations, get you thinking about pros and cons, and show you the code to get started.

In this exercise, you will:

- Use GPT prompts to convert natural language to SQL.
- Experiment with different GPT prompts.
- Use the generated SQL to query the PostgreSQL database started earlier.
- Return query results from PostgreSQL and display them in the browser.

Let's start by experimenting with different GPT prompts that can be used to convert natural language to SQL.

### Using the Natural Language to SQL Feature

1. In the [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/02-openai-create-resource.md#start-app-services) you started the database, APIs, and application. You also updated the `.env` file. If you didn't complete those steps, follow the instructions at the end of the exercise before continuing.

1. Go back to the browser (*http://localhost:4200*) and locate the **Custom Query** section of the page below the datagrid. Notice that a sample query value is already included: *Get the total revenue for all orders. Group by company and include the city.*

    :::image type="content" source="./media/openai-custom-query.png" alt-text="Natural language to SQL query.":::

1. Select the **Run Query** button. This will pass the user's natural language query to Azure OpenAI which will convert it to SQL. The SQL query will then be used to query the database and return any potential results.

1. Run the following **Custom Query**:

    ```
    Get the total revenue for Adventure Works Cycles. Include the contact information as well.
    ```

1. View the terminal window running the API server in Visual Studio Code and notice it displays the SQL query returned from Azure OpenAI. The JSON data is used by the server-side APIs to query the PostgreSQL database. Any string values included in the query are added as parameter values to prevent SQL injection attacks:

    ```json
    { 
        "sql": "SELECT c.company, c.city, c.email, SUM(o.total) AS revenue FROM customers c INNER JOIN orders o ON c.id = o.customer_id WHERE c.company = $1 GROUP BY c.company, c.city, c.email", 
        "paramValues": ["Adventure Works Cycles"] 
    }
    ```

1. Go back to the browser and select **Reset Data** to view all of the customers again in the datagrid.

### Exploring the Natural Language to SQL Code

[!INCLUDE [Note-Open-Files-VS-Code](./includes/tip-open-files-vs-code.md)]

> [!NOTE]
> The goal of this exercise is to show what's possible with natural language to SQL functionality and demonstrate how to get started using it. As mentioned earlier, it's important to discuss if this type of AI is appropriate for your organization before proceeding with any implementation. It's also **imperative to plan for proper prompt rules and database security measures** to prevent unauthorized access and protect sensitive data.

1. Now that you've seen the natural language to SQL feature in action, let's examine how it is implemented.

1. Open the *server/apiRoutes.ts* file and locate the `generateSql` route. This API route is called by the client-side application running in the browser and used to generate SQL from a natural language query. Once the SQL query is retrieved, it's used to query the database and return results. 

    ```typescript
    router.post('/generateSql', async (req, res) => {
        const userPrompt = req.body.prompt;

        if (!userPrompt) {
            return res.status(400).json({ error: 'Missing parameter "prompt".' });
        }

        try {
            // Call Azure OpenAI to convert the user prompt into a SQL query
            const sqlCommandObject = await getSQLFromNLP(userPrompt);

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

    Notice the following functionality in the `generateSql` route:

    - It retrieves the user query value from `req.body.prompt` and assigns it to a variable named `userPrompt`. This value will be used in the GPT prompt.
    - It calls a `getSQLFromNLP()` function to convert natural language to SQL.
    - It passes the generated SQL to a function named `queryDb` that executes the SQL query and returns results from the database.

1. Open the *server/openAI.ts* file in your editor and locate the `getSQLFromNLP()` function. This function is called by the `generatesql` route and is used to convert natural language to SQL.

    ```typescript
    async function getSQLFromNLP(userPrompt: string): Promise<QueryData> {
        // Get the high-level database schema summary to be used in the prompt.
        // The db.schema file could be generated by a background process or the 
        // schema could be dynamically retrieved.
        const dbSchema = await fs.promises.readFile('db.schema', 'utf8');

        const systemPrompt = `
        Assistant is a natural language to SQL bot that returns a JSON object with the SQL query and 
        the parameter values in it. The SQL will query a PostgreSQL database.
        
        PostgreSQL tables with their columns:    
    
        ${dbSchema}
    
        Rules:
        - Convert any strings to a PostgreSQL parameterized query value to avoid SQL injection attacks.
        - Return a JSON object with the following structure: { "sql": "", "paramValues": [] }

        Examples:

        User: "Display all company reviews. Group by company."      
        Assistant: { "sql": "SELECT * FROM reviews", "paramValues": [] }

        User: "Display all reviews for companies located in cities that start with 'L'."
        Assistant: { "sql": "SELECT r.* FROM reviews r INNER JOIN customers c ON r.customer_id = c.id WHERE c.city LIKE 'L%'", "paramValues": [] }

        User: "Display revenue for companies located in London. Include the company name and city."
        Assistant: { 
            "sql": "SELECT c.company, c.city, SUM(o.total) AS revenue FROM customers c INNER JOIN orders o ON c.id = o.customer_id WHERE c.city = $1 GROUP BY c.company, c.city", 
            "paramValues": ["London"] 
        }

        User: "Get the total revenue for Adventure Works Cycles. Include the contact information as well."
        Assistant: { 
            "sql": "SELECT c.company, c.city, c.email, SUM(o.total) AS revenue FROM customers c INNER JOIN orders o ON c.id = o.customer_id WHERE c.company = $1 GROUP BY c.company, c.city, c.email", 
            "paramValues": ["Adventure Works Cycles"] 
        }
        `;

        let queryData: QueryData = { sql: '', paramValues: [], error: '' };
        let results = '';

        try {
            results = await callOpenAI(systemPrompt, userPrompt);
            if (results) {
                console.log('results', results);
                const parsedResults = JSON.parse(results);
                queryData = { ...queryData, ...parsedResults };
                if (isProhibitedQuery(queryData.sql)) {
                    queryData.sql = '';
                    queryData.error = 'Prohibited query.';
                }
            }
        } catch (error) {
            console.log(error);
            if (isProhibitedQuery(results)) {
                queryData.sql = '';
                queryData.error = 'Prohibited query.';
            } else {
                queryData.error = results;
            }
        }

        return queryData;
    }
    ```

    - A `userPrompt` parameter is passed into the function. The `userPrompt` value is the natural language query entered by the user in the browser. 
    - A `systemPrompt` defines the type of AI assistant to be used and rules that should be followed. This helps Azure OpenAI understand the database structure, what rules to apply, and how to return the generated SQL query and parameters.
    - A function named `callOpenAI()` is called and the `systemPrompt` and `userPrompt` values are passed to it.
    - The results are checked to ensure no prohibited values are included in the generated SQL query. If prohibited values are found, the SQL query is set to an empty string.

1. Let's walk through the system prompt in more detail:

    ```typescript
    const systemPrompt = `
      Assistant is a natural language to SQL bot that returns a JSON object with the SQL query and 
      the parameter values in it. The SQL will query a PostgreSQL database.
      
      PostgreSQL tables with their columns:    
  
      ${dbSchema}
  
      Rules:
      - Convert any strings to a PostgreSQL parameterized query value to avoid SQL injection attacks.
      - Return a JSON object with the following structure: { "sql": "", "paramValues": [] }

      Examples:

      User: "Display all company reviews. Group by company."      
      Assistant: { "sql": "SELECT * FROM reviews", "paramValues": [] }

      User: "Display all reviews for companies located in cities that start with 'L'."
      Assistant: { "sql": "SELECT r.* FROM reviews r INNER JOIN customers c ON r.customer_id = c.id WHERE c.city LIKE 'L%'", "paramValues": [] }

      User: "Display revenue for companies located in London. Include the company name and city."
      Assistant: { 
        "sql": "SELECT c.company, c.city, SUM(o.total) AS revenue FROM customers c INNER JOIN orders o ON c.id = o.customer_id WHERE c.city = $1 GROUP BY c.company, c.city", 
        "paramValues": ["London"] 
      }

      User: "Get the total revenue for Adventure Works Cycles. Include the contact information as well."
      Assistant: { 
        "sql": "SELECT c.company, c.city, c.email, SUM(o.total) AS revenue FROM customers c INNER JOIN orders o ON c.id = o.customer_id WHERE c.company = $1 GROUP BY c.company, c.city, c.email", 
        "paramValues": ["Adventure Works Cycles"] 
      }
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
        > You may consider creating read-only views that only contain the data users are allowed to query using natural language to SQL.

    - A rule is defined to convert any string values to a parameterized query value to avoid SQL injection attacks.
    - A rule is defined to always return a JSON object with the SQL query and the parameter values in it.
    - Example user prompts and the expected SQL query and parameter values are provided. This is referred to as ["few-shot" learning](/azure/ai-services/openai/concepts/prompt-engineering?WT.mc_id=m365-94501-dwahlin#examples). Although LLMs are trained on large amounts of data, they can be adapted to new tasks with only a few examples. An alternative approach is "zero-shot" learning where no example is provided and the model is expected to generate the correct SQL query and parameter values.

1. The `getSQLFromNLP()` function sends the system and user prompts to a function named `callOpenAI()` which is also located in the *server/openAI.ts* file. The `callOpenAI()` function determines if the Azure OpenAI service or OpenAI service should be called by checking environment variables. If a key, endpoint, and model are available in the environment variables then Azure OpenAI is called, otherwise OpenAI is called.

    ```typescript
    function callOpenAI(systemPrompt: string, userPrompt: string, temperature = 0, useBYOD = false) {
        const isAzureOpenAI = OPENAI_API_KEY && OPENAI_ENDPOINT && OPENAI_MODEL;

        if (isAzureOpenAI) {
            if (useBYOD) {
                return getAzureOpenAIBYODCompletion(systemPrompt, userPrompt, temperature);
            }
            return getAzureOpenAICompletion(systemPrompt, userPrompt, temperature);
        }

        return getOpenAICompletion(systemPrompt, userPrompt, temperature);
    }
    ```

    > [!NOTE]
    > Although we'll focus on Azure OpenAI throughout this tutorial, if you only supply an `OPENAI_API_KEY` value in the *.env* file, the application will use OpenAI instead. If you choose to use OpenAI instead of Azure OpenAI you may see different results in some cases.

1. Locate the `getAzureOpenAICompletion()` function.

    ```typescript
    async function getAzureOpenAICompletion(systemPrompt: string, userPrompt: string, temperature: number): Promise<string> {
        const completion = await createAzureOpenAICompletion(systemPrompt, userPrompt, temperature);
        let content = completion.choices[0]?.message?.content?.trim() ?? '';
        console.log('Azure OpenAI Output: \n', content);
        if (content && content.includes('{') && content.includes('}')) {
            content = extractJson(content);
        }
        return content;
    }
    ```

    This function does the following:

    - **Parameters:**
        - `systemPrompt`, `userPrompt`, and `temperature` are the main parameters.
            - `systemPrompt`: Informs the Azure OpenAI model of its role and the rules to follow.
            - `userPrompt`: Contains the user-provided information such as natural language input or rules for generating the output.
            - `temperature`: Dictates the creativity level of the model's response. A higher value results in more creative outputs, while lower values (e.g., 0) produce more deterministic answers.

    - **Completion Generation:**
        - The function calls `createAzureOpenAICompletion()` with `systemPrompt`, `userPrompt`, and `temperature` to generate a completion.
        - It extracts the content from the first choice in the completion, trimming any extra whitespace.
        - If the content contains JSON-like structures (indicated by the presence of `{` and `}`), it extracts the JSON content.

    - **Logging and Return Value:**
        - The function logs the Azure OpenAI output to the console.
        - It returns the processed content as a string.

1. Locate the `createAzureOpenAICompletion()` function. 

    ```typescript
    async function createAzureOpenAICompletion(systemPrompt: string, userPrompt: string, temperature: number, dataSources?: any[]): Promise<any> {
        const baseEnvVars = ['OPENAI_API_KEY', 'OPENAI_ENDPOINT', 'OPENAI_MODEL'];
        const byodEnvVars = ['AZURE_AI_SEARCH_ENDPOINT', 'AZURE_AI_SEARCH_KEY', 'AZURE_AI_SEARCH_INDEX'];
        const requiredEnvVars = dataSources ? [...baseEnvVars, ...byodEnvVars] : baseEnvVars;
        checkRequiredEnvVars(requiredEnvVars);

        const config = { 
            apiKey: OPENAI_API_KEY,
            endpoint: OPENAI_ENDPOINT,
            apiVersion: OPENAI_API_VERSION,
            deployment: OPENAI_MODEL
        };
        const aoai = new AzureOpenAI(config);
        const completion = await aoai.chat.completions.create({
            model: OPENAI_MODEL, // gpt-4o, gpt-3.5-turbo, etc. Pulled from .env file
            max_tokens: 1024,
            temperature,
            response_format: {
                type: "json_object",
            },
            messages: [
                { role: 'system', content: systemPrompt },
                { role: 'user', content: userPrompt }
            ],
            // @ts-expect-error data_sources is a custom property used with the "Azure Add Your Data" feature
            data_sources: dataSources
        });
        return completion;
    }

    function checkRequiredEnvVars(requiredEnvVars: string[]) {
        for (const envVar of requiredEnvVars) {
            if (!process.env[envVar]) {
                throw new Error(`Missing ${envVar} in environment variables.`);
            }
        }
    }
    ```

    This function does the following:

    - **Parameters:**
        - `systemPrompt`, `userPrompt`, and `temperature` are the main parameters discussed earlier.
        - An optional `dataSources` parameter supports the "Azure Bring Your Own Data" feature, which will be covered later in this tutorial.

    - **Environment Variables Check:**
        - The function verifies the presence of essential environment variables, throwing an error if any are missing.

    - **Configuration Object:**
        - A `config` object is created using values from the `.env` file (`OPENAI_API_KEY`, `OPENAI_ENDPOINT`, `OPENAI_API_VERSION`, `OPENAI_MODEL`). These values are used to construct the URL for calling Azure OpenAI.

    - **AzureOpenAI Instance:**
        - An instance of `AzureOpenAI` is created using the `config` object. The `AzureOpenAI` symbol is part of the `openai` package, which should be imported at the top of your file.

    - **Generating a Completion:**
        - The `chat.completions.create()` function is called with the following properties:
            - `model`: Specifies the GPT model (e.g., gpt-4o, gpt-3.5-turbo) as defined in your `.env` file.
            - `max_tokens`: Defines the maximum number of tokens for the completion.
            - `temperature`: Sets the sampling temperature. Higher values (e.g., 0.9) yield more creative responses, while lower values (e.g., 0) produce more deterministic answers.
            - `response_format`: Defines the response format. Here, it's set to return a JSON object. More details on JSON mode can be found in the [Azure OpenAI reference documentation](/azure/ai-services/openai/how-to/json-mode?WT.mc_id=m365-94501-dwahlin).
            - `messages`: Contains the messages for generating chat completions. This example includes two messages: one from the system (defining behavior and rules) and one from the user (containing the prompt text).

    - **Return Value:**
        - The function returns the completion object generated by Azure OpenAI.

1. Comment out the following lines in the `getSQLFromNLP()` function:

    ```typescript
    // if (isProhibitedQuery(queryData.sql)) { 
    //     queryData.sql = '';
    // }
    ```

1. Save *openAI.ts*. The API server will automatically rebuild the TypeScript code and restart the server.

1. Go back to the browser and enter *Select all table names from the database* into the **Custom Query** input. Select **Run Query**. Are table names displayed?

1. Go back to the `getSQLFromNLP()` function in *server/openAI.ts* and add the following rule into the `Rules:` section of the system prompt and then save the file.

    ```
    - Do not allow the SELECT query to return table names, function names, or procedure names.
    ```

1. Go back to the browser and perform the following tasks:

    - Enter *Select all table names from the database* into the **Custom Query** input. Select **Run Query**. Are table names displayed?
    - Enter *Select all function names from the database.* into the **Custom Query** input and select **Run Query** again. Are function names displayed?

1. QUESTION: Will a model always follow the rules you define in the prompt?

    ANSWER: No! It's important to note that OpenAI models can return unexpected results on occasion that may not match the rules you've defined. It's important to plan for that in your code.

1. Go back to *server/openAI.ts* and locate the `isProhibitedQuery()` function. This is an example of post-processing code that can be run after Azure OpenAI returns results. Notice that it sets the `sql` property to an empty string if prohibited keywords are returned in the generated SQL query. This ensures that if unexpected results are returned from Azure OpenAI, the SQL query will not be run against the database.

    ```typescript
    function isProhibitedQuery(query: string): boolean {
        if (!query) return false;

        const prohibitedKeywords = [
            'insert', 'update', 'delete', 'drop', 'truncate', 'alter', 'create', 'replace',
            'information_schema', 'pg_catalog', 'pg_tables', 'pg_proc', 'pg_namespace', 'pg_class',
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

1. Go back to *server/openAI.ts* and uncomment the following code in the `getSQLFromNLP()` function. Save the file.

    ```typescript
    if (isProhibitedQuery(queryData.sql)) { 
        queryData.sql = '';
    }
    ```

1. Remove the following rule from `systemPrompt` and save the file.

    ```
    - Do not allow the SELECT query to return table names, function names, or procedure names.
    ```

1. Go back to the browser, enter *Select all table names from the database* into the **Custom Query** input again and select the **Run Query** button. 

1. Do any table results display? Even without the rule in place, the `isProhibitedQuery` post-processing code prohibits that type of query from being run against the database.

1. As discussed earlier, integrating natural language to SQL in line of business applications can be quite beneficial to users, but it does come with its own set of considerations.

    **Advantages:**

    - User-friendliness: This feature can make database interaction more accessible to users without technical expertise, reducing the need for SQL knowledge and potentially speeding up operations.

    - Increased productivity: Business analysts, marketers, executives, and other non-technical users can retrieve valuable information from databases without having to rely on technical experts, thereby increasing efficiency.

    - Broad application: By using advanced language models, applications can be designed to cater to a wide range of users and use-cases.

    **Considerations:**

    - Security: One of the biggest concerns is security. If users can interact with databases using natural language, there needs to be robust security measures in place to prevent unauthorized access or malicious queries. You may consider implementing a read-only mode to prevent users from modifying data.

    - Data Privacy: Certain data might be sensitive and should not be easily accessible, so you'll need to ensure proper safeguards and user permissions are in place.

    - Accuracy: While natural language processing has improved significantly, it's not perfect. Misinterpretation of user queries could lead to inaccurate results or unexpected behavior. You'll need to plan for how unexpected results will be handled.

    - Efficiency: There are no guarantees that the SQL returned from a natural language query will be efficient. In some cases, additional calls to Azure OpenAI may be required if post-processing rules detect issues with SQL queries.

    - Training and User Adaptation: Users need to be trained to formulate their queries correctly. While it's easier than learning SQL, there can still be a learning curve involved.

1. A few final points to consider before moving on to the next exercise:

    - Remember that, "Just because you can doesn't mean you should" applies here. Use extreme caution and careful planning before integrating natural language to SQL into an application. It's important to understand the potential risks and to plan for them.
    - Before using this type of technology, discuss potential scenarios with your team, database administrators, security team, stakeholders, and any other relevant parties to ensure that it's appropriate for your organization. It's important to discuss if natural language to SQL meets security, privacy, and any other requirements your organization may have in place.
    - Security should be a primary concern and built into the planning, development, and deployment process.
    - While natural language to SQL can be very powerful, careful planning must go into it to ensure prompts have required rules and that post-processing functionality is included. Plan for additional time to implement and test this type of functionality and to account for scenarios where unexpected results are returned.
    - With Azure OpenAI, customers get the security capabilities of Microsoft Azure while running the same models as OpenAI. Azure OpenAI offers private networking, regional availability, and responsible AI content filtering. Learn more about [Data, privacy, and security for Azure OpenAI Service](/legal/cognitive-services/openai/data-privacy).

1. You've now seen how to use Azure OpenAI to convert natural language to SQL and learned about the pros and cons of implementing this type of functionality. In the next exercise, you'll learn how email and SMS messages can be generated using Azure OpenAI.

## Next Step

> [!div class="nextstepaction"]
> [AI: Generating Completions](04-OpenAI-Completions.md)