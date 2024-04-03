# Lab 2: Action Integration

### Task 1: Incorporating pre-built actions from the GitHub Marketplace

GitHub Marketplace is a central location for you to find actions created by the GitHub community. These actions can be incorporated into your workflows to automate common tasks without having to write the code yourself.

Here's how you can incorporate a pre-built action from the GitHub Marketplace:

1. In the Egde browser add a new tab and navigate `https://login.docker.com/u/login` page if you already having an Docker hub account **enter Username or email address** **(1)** and click on **Continue** **(2)**.

    ![](../media/ex2-task2-step3.png)

2. If you don't have account in the Docker Hub click on `sign up`.

    ![](../media/ex2-task2-step3a.png)

3. In the sign up page of Docker Hub enter you **personal email id** **(1)**, proivde a unique **Username** **(2)**, provide a **Password** **(3)**, click on check box for **Send me occasional product updates and announcements** **(4)**, and click on **Sign Up** **(5)**. Perform Step 3

    ![](../media/ex2-task2-step5.png)

4. Enter the **Password** **(1)** and click on **Continue** **(2)**.

    ![](../media/ex2-task2-step6.png)

5. Login to the Personal Email and **Verify Email Address** send by docker.

6. In the Docker Hub tab click on **Profile** **(1)** and click on **My Account** **(2)**.

   ![](../media/ex2-task2-step8.png)

7. In the Docker Hub my account tab click on **Security** **(1)** and click on **New Access Token** **(2)**.

   ![](../media/ex2-task2-step9.png)

8. In **New Access Token** pop-up, enter the Access Token Description as **docker-demo** **(1)**, and click on **Generate** **(2)**.

    ![](../media/ex2-task2-step10.png)

9. In **Copy Access Token** pop-up, click on **Copy and Close** button and paste the coped access token in a notepad for future.

    ![](../media/ex2-task2-step11.png)

10. **Find an Action**: Browse the [GitHub](https://github.com/marketplace?type=actions) Marketplace to find an action that suits your needs. You can search by keywords, categories, or use filters to narrow down the results.

11. In the **GitHub Marketplace**, in search bar search for **Build and push Docker images** **(1)** hit enter, select **Build and push Docker images** **(2)** and feel free to go throught the content.

    ![](../media/ex2-task2-step16.png)

    ![](../media/ex2-task2-step16a.png)

12. Now lets create a workflow to publish into Docker Hub using GitHub action. Navigate to the **Code** **(1)**, click on **Add File** **(2)** and click on **+ Create new file** **(3)**.
    
    ![](../media/ex2-task2-step18.png)

13. Provider file name as **docker** **(1)**, in the editor **copy and paste** **(2)** the below script, and click in **commit changes** **(3)**.

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

14. Now lets create a workflow to publish into Docker Hub using GitHub action. Navigate to the **Code** **(1)**, click on **Add File** **(2)** and click on **+ Create new file** **(3)**.
    
    ![](../media/ex2-task2-step18.png)

15. Provider file name as **index.html** **(1)**, in the editor **copy and paste** **(2)** the below script, and click in **commit changes** **(3)**.

    ```
    <!DOCTYPE html>
    <html>
    <head>
        <title>My Sample Page</title>
    </head>
    <body>
        <h1>Welcome to My Sample Page!</h1>
        <p>This is a simple HTML file served from a Docker container.</p>
    </body>
    </html>
    ```

    ![](../media/ex2-task2-step20.png)

16. Navigate to the **Code** **(1)** and click on **.github/workflows** **(2)** folder.

    ![](../media/editfolder.png)

17. In the **.github/workflows** folder, select **cl.yml** **(1)** and click on **edit** **(2)**.

    ![](../media/editfolder1.png)

18. In the editor update the code with the below provided code and click on and click on **commit changes** **(1)**, replace **{DOCKERHUB_USERNAME}** **(2)** with you docker username in line number 17 and 29, and replace **{DOCKERHUB_TOKEN}** **(3)** with Docker PAT line number 18.

    ```
    name: ci

    on:
      workflow_dispatch:

    jobs:
    build:
        runs-on: ubuntu-latest
        steps:
        -
            name: Checkout
            uses: actions/checkout@v4
        -
            name: Login to Docker Hub
            uses: docker/login-action@v3
            with:
            username: {DOCKERHUB_USERNAME}
            password: {DOCKERHUB_TOKEN}
        -
            name: Set up Docker Buildx
            uses: docker/setup-buildx-action@v3
        -
            name: Build and push
            uses: docker/build-push-action@v5
            with:
            context: .
            file: ./docker
            push: true
            tags: {DOCKERHUB_USERNAME}/clockbox:latest
    ```

    ![](../media/ex2-task2-step17.png)

19. In the pop up windows of **Commit Changes** click on the **Commit changes**.

    ![](../media/commit-changes.png)

20. Click on **Action** **(1)**, verifiy the workflow has been executed successfully once the workflow is succedded select the newly created workflow **updated cl.yml** **(2)**.

    ![](../media/ex1-task4-step6.png)

21. Naviagte back to the Docker Hub and click on **Repositories** and crossverify the Repositorie has been created **successfully**.

    ![](../media/ex2-task2-step25.png)

### Task 2: Using a third-party action to deploy a Docker container to a Azure platform.

1. 