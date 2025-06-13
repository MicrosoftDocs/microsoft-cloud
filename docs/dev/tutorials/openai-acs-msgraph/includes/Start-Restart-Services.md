### Start/Restart the Application and API Servers

Perform **one** of the following steps based on the exercises you completed up to this point:

- If you started the database, API server, and web server in an earlier exercise, you need to stop the API server and web server and restart them to pick up the *.env* file changes. You can leave the database running. 

    Locate the terminal windows running the API server and web server and press <kbd>CTRL + C</kbd> to stop them. Start them again by typing `npm start` in each terminal window and pressing <kbd>Enter</kbd>. Continue to the next exercise.

- If you haven't started the database, API server, and web server yet, complete the following steps:

    [!INCLUDE [Start-Services](./start-services.md)]