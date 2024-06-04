# Lab 2: Action Integration

### Task 1: Using GitHub Actions with container registries

1. Navigate to `potal.azure.com`, in the search bar search for **Container registries** **(1)** and select **Container registries** **(2)**.

   ![](../media/search-continer.png)

1. In the Container registries tab click on **+ Create** button.

   ![](../media/create-continer.png)

1. On the **basics** tab, add the following details:

    - **Subscription**: Select the default subscription
    - **Resource Group**: Select github-action-
    - **Registry Name**: Add gacontainer
    - **Location**: Select the default **location**
    - **Pricing Plan**: Choose **Standard** **(4)**

 Click on **Review + Create** **(5)**

1. Click on **Create**.

1. Once the deployment is complete, click on **Go to Resource**.

1. Navigate to **Access Keys** **(1)** on the left pane under the **Settings** tab and copy the **Registry Name** **(2)** and **Login server** **(3)** into a notepad.

1. Navigate to the **Code** **(1)**, click on **Add File** **(2)** and click on **+ Create new file** **(3)**.
    
    ![](../media/ex2-task2-step18.png)
    
2. Provider file name as **Dockerfile** **(1)**, in the editor **copy and paste** **(2)** the below script, and click in **commit changes** **(3)**.

    ```
    # Use an official Nginx runtime as a parent image
    FROM nginx:latest

    # Copy the HTML file to the Nginx container
    COPY index.html /usr/share/nginx/html

    # Make port 80 available to the outside of the Docker container
    EXPOSE 80

    # Start Nginx when the container launches
    CMD ["nginx", "-g", "daemon off;"]
    ```

    ![](../media/ex2-task2-step18a.png)

3. In the **Commit changes** pop-up, click on **Commit changes** button.

    ![](../media/docker-commit.png)

4. Now let's create a workflow to publish into Docker Hub using GitHub action. Navigate to the **Code** **(1)**, click on **Add File** **(2)** and click on **+ Create new file** **(3)**.
    
    ![](../media/ex2-task2-step18.png)

5. Provider file name as **index.html** **(1)**, in the editor **copy and paste** **(2)** the below script, and click in **commit changes** **(3)**.

    ```
    <!DOCTYPE html>
    <html>
    <head>
        <title>GitHub Actions Workshop</title>
        <style>
            body {
                font-family: Arial, sans-serif;
                margin: 20px;
            }
            h1 {
                color: #333;
            }
            h2 {
                color: #666;
                margin-top: 30px;
            }
            p {
                margin-bottom: 20px;
            }
            ol {
                margin-left: 20px;
            }
        </style>
    </head>
    <body>
        <h1>GitHub Actions Workshop</h1>
        <p>Welcome to the GitHub Actions Workshop! This workshop is designed to help you understand and implement GitHub Actions effectively in your projects. Whether you're new to GitHub Actions or looking to enhance your existing knowledge, this workshop covers a range of topics to get you started and take your workflows to the next level.</p>

        <h2>Table of Contents</h2>
        <ol>
            <li><a href="#workflow-setup">Workflow Setup</a></li>
            <li><a href="#action-integration">Action Integration</a></li>
            <li><a href="#best-practices-and-security">Best Practices and Security</a></li>
            <li><a href="#advanced-usage-and-integration">Advanced Usage and Integration</a></li>
        </ol>

        <h2 id="workflow-setup">Workflow Setup</h2>
        <p>In this module 1, you'll learn the fundamentals of GitHub Actions and workflow files. We'll cover how to trigger workflows with events like pushes and pull requests, define jobs and steps within workflows, and set up a basic CI workflow for testing code on every push.</p>

        <h2 id="action-integration">Action Integration</h2>
        <p>In Module 2 focuses on incorporating pre-built actions from the GitHub Marketplace and creating custom actions for reusable tasks. You'll learn how to leverage third-party actions to streamline your workflows, with a practical example of deploying a Docker container to a cloud platform.</p>

        <h2 id="best-practices-and-security">Best Practices and Security</h2>
        <p>In Module 3, we'll discuss guidelines for writing efficient and maintainable workflows, as well as securing sensitive data like API keys and credentials. You'll explore examples of optimizing workflow performance by caching dependencies to ensure smooth operations.</p>

        <h2 id="advanced-usage-and-integration">Advanced Usage and Integration</h2>
        <p>Module 4 dives into advanced features of GitHub Actions, including matrix builds and parallelism. You'll also learn how to integrate with GitHub features such as pull requests and issue tracking, with a hands-on example of setting up a matrix build to test across different operating systems and versions.</p>
    </body>
    </html>
    ```

    ![](../media/newupdate.png)

6. In the **Commit changes** pop-up, click on **Commit changes** button.

    ![](../media/index-commit.png)

7. Navigate to the **Code** **(1)** and click on **.github/workflows** **(2)** folder.

    ![](../media/editfolder.png)

8. Navigate to the **Code** **(1)** and click on **.github/workflows** **(2)** folder.

    ![](../media/editfolder.png)

9. In the **.github/workflows** folder, click on **Add files** **(1)**, and click on **+ Create new file** **(2)**.

    ![](../media/4th-oidc.png)


10. In the editor update the code with the below-provided code, replace **{Login_server}** from line 30 and 40 with **Azure Container registry Login server**, and replace **{Registry name}** from line number **Azure Container registry Registry name**.

    ```
    name: Build and Push Docker Image to ACR
    
    on:
      push:
        branches:
          - main
    
    jobs:
      build-and-push:
        runs-on: ubuntu-latest
    
        steps:
        # Checkout the repository
        - name: Checkout repository
          uses: actions/checkout@v2
    
        # Log in to Azure
        - name: Log in to Azure CLI
          uses: azure/login@v1
          with:
            creds: ${{ secrets.AZURE_CREDENTIALS }}
    
        # List files in the current directory
        - name: List files
          run: ls -la
    
        # Build Docker image
        - name: Build Docker image
          run: |
            docker buildx build . -t {Login_server}/my-app:latest
    
        # Log in to Azure Container Registry
        - name: Log in to Azure Container Registry
          run: |
            az acr login --name {Registry name}
    
        # Push Docker image to Azure Container Registry
        - name: Push Docker image
          run: |
            docker push {Login_server}/my-app:latest
    ```

    ![](../media/ex2-task2-step17.png)

11. Provider file name as **docker.yml** **(1)**, in the editor **copy and paste** **(2)** the below script, and click in **commit changes** **(3)**.

12. In the pop up windows of **Commit Changes** click on the **Commit changes**.

    ![](../media/commit-changes.png)

13. Click on **Action** **(1)**, verify the workflow has been executed successfully once the workflow is succedded select the newly created workflow **updated cl.yml** **(2)**.

    ![](../media/ex1-task4-step6.png)

14. Navigate back to the Docker Hub and click on **Repositories** and cross verify the Repositorie has been created **successfully**.

    ![](../media/ex2-task2-step25.png)

15. 
15. Click on **Next** button for next Lab.
