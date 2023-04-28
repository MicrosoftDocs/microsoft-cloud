1. In the following steps, you create three terminal windows in Visual Studio Code. 

    :::image type="content" source="../media/vs-code-terminals.png" alt-text="Three terminal windows in Visual Studio Code":::

1. Right-click on the *.env* file in the Visual Studio Code file list and select **Open in Integrated Terminal**. Ensure that your terminal is at the root of the project before continuing.

1. Run `docker-compose up` in the window and press <kbd>Enter</kbd> to start the Postgresql server.

1. Press the **+** icon in the Visual Studio Code **Terminal toolbar** to create a second terminal window. 

    :::image type="content" source="../media/vs-code-terminal-plus-icon.png" alt-text="Visual Studio Code + icon in the terminal toolbar.":::

1. `cd` into the *server/typescript* folder and run the following commands to install the dependencies and start the API server.

    - `npm install`
    - `npm start`

1. Press the **+** icon again in the Visual Studio Code **Terminal toolbar** to create a third terminal window. `cd` into the *client* folder and run the following commands to install the dependencies and start the client application.

    - `npm install`
    - `npm start` 

1. A browser launches and you are taken to *http://localhost:4200*. 

    :::image type="content" source="../media/openai-enabled-screenshot.png" alt-text="Application screenshot with Azure OpenAI enabled":::