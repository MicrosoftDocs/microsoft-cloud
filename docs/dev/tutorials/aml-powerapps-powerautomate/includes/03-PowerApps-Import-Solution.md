<!-- markdownlint-disable MD041 -->

In this exercise, you will clone a GitHub repository that has a Solution package and import it into Power Apps to enable you to integrate your deployed machine learning model with Power Automate and use it in Power Apps.

A Solution is a package that helps us transport apps and components from one environment to another. It can contain one or more apps as well as other components such as tables, flows, and more.

In this exercise, you will:

- Clone a GitHub repository.
- Import a Solution into Power Apps.

Let's get started by cloning a GitHub repository!

### Clone the Solution

1. Run the following command to clone the [Solution's GitHub Repository](https://github.com/microsoft/AzureML-PowerAppSolution) to your machine.

    > [!TIP]
    > If you hover over the code you can select the "copy" icon to copy the code to your clipboard.

    ```console
    git clone https://github.com/microsoft/AzureML-PowerAppSolution
    ```

2. Notice the *Solution* folder as it contains the packaged solution that you will import into Power Apps.

    :::image type="content" source="../media/github-repo-locally.png" alt-text="GitHub Cloned Repository Locally":::

### Import the Solution Into Power Apps

1. Visit [Power Apps Studio](https://make.powerapps.com/) in your browser and sign in.

    :::image type="content" source="../media/powerapps-studio.png" alt-text="Power Apps Studio":::

    > [!NOTE]
    > You need to create a Power Platform environment before being able to move forward by following the guide included in the Prerequisites.

2. Select **More** from the left side window. Select **Solutions**.

    :::image type="content" source="../media/powerapps-select-solutions.png" alt-text="Selecting Solutions in Power Apps Studio":::

3. Select **Import solution** to load the *.zip* file that you cloned.

    :::image type="content" source="../media/powerapps-solutions.png" alt-text="Solutions tab in Power Apps Studio":::

4. Perform the following tasks:
    - Select **Browse**
    - Select the *.zip* file that you cloned.
    - Select **Open** followed by **Next**.
    - After it finishes processing, select **Import**.

    :::image type="content" source="../media/powerapps-import-solution.png" alt-text="Importing a Solution in Power Apps Studio":::

5. You can now find a solution named **AML PowerApps HelpMeWrite Sample** in the solutions table ready for you to use.

    :::image type="content" source="../media/powerapps-solution-imported.png" alt-text="Solution After Import in Power Apps Studio":::
