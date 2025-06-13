1. In the following steps you'll create three terminal windows in Visual Studio Code. 

    :::image type="content" source="../media/vs-code-terminals.png" alt-text="Three terminal windows in Visual Studio Code":::

1. Right-click on the *.env* file in the Visual Studio Code file list and select **Open in Integrated Terminal**. Ensure that your terminal is at the root of the project - *openai-acs-msgraph* - before continuing.

1. Choose from **one** of the following options to start the PostgreSQL database:

    - If you have [Docker Desktop](https://www.docker.com/get-started/) installed and running, run `docker-compose up` in the terminal window and press <kbd>Enter</kbd>.
    - If you have Podman with [podman-compose](https://podman-desktop.io/docs/compose/podman-compose) installed and running, run `podman-compose up` in the terminal window and press <kbd>Enter</kbd>.
    - To run the PostgreSQL container directly using either Docker Desktop, Podman, nerdctl, or another container runtime you have installed, run the following command in the terminal window:

       - Mac, Linux, or Windows Subsystem for Linux (WSL): 

           ```
           [docker | podman | nerdctl] run --name postgresDb -e POSTGRES_USER=web -e POSTGRES_PASSWORD=web-password -e POSTGRES_DB=CustomersDB -v $(pwd)/data:/var/lib/postgresql/data -p 5432:5432 postgres
           ```

       - Windows with PowerShell: 

           ```
           [docker | podman] run --name postgresDb -e POSTGRES_USER=web -e POSTGRES_PASSWORD=web-password -e POSTGRES_DB=CustomersDB -v ${PWD}/data:/var/lib/postgresql/data -p 5432:5432 postgres
           ```

1. Once the database container starts, press the **+** icon in the Visual Studio Code **Terminal toolbar** to create a second terminal window. 

    :::image type="content" source="../media/vs-code-terminal-plus-icon.png" alt-text="Visual Studio Code + icon in the terminal toolbar.":::

1. `cd` into the *server/typescript* folder and run the following commands to install the dependencies and start the API server.

    - `npm install`
    - `npm start`

1. Press the **+** icon again in the Visual Studio Code **Terminal toolbar** to create a third terminal window. 

1. `cd` into the *client* folder and run the following commands to install the dependencies and start the web server.

    - `npm install`
    - `npm start` 

1. A browser will launch and you'll be taken to *http://localhost:4200*. 

    :::image type="content" source="../media/openai-enabled-screenshot.png" alt-text="Application screenshot with Azure OpenAI enabled":::