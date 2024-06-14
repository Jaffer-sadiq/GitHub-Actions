# Lab 3: Advanced Workflows

- **Reusing workflows**: Rather than copying and pasting from one workflow to another, you can make workflows reusable. You and anyone with access to the reusable workflow can then call the reusable workflow from another workflow.Reusing workflows avoids duplication. This makes workflows easier to maintain and allows you to create new workflows more quickly by building on the work of others, just as you do with actions. Workflow reuse also promotes best practice by helping you to use workflows that are well designed, have already been tested, and have been proven to be effective. Your organization can build up a library of reusable workflows that can be centrally maintained, for more infomation please go throught the given link [Reusing workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows).


## Task 1: Using Reusable Workflow

1. From the GitHub, navigate to `.github/workflows` and Click on **ellipse button (2)** select **+ create new file (3)**.

   ![](../media/env23.png)

1. Copy the code given below, paste it in the window, and name the file as **reusable-print-message.yml**

   ```
   name: Reusable Print Message

   on:
     workflow_call:
       inputs:
         message:
           required: true
           type: string

   jobs:
     print-message:
       runs-on: ubuntu-latest

       steps:
         - name: Print message
           run: echo "Hi, this message is from primary workflow" 
   ```

   ![](../media/env24.png)

1. Click on **Commit changes (1)** button and click on **Commit changes (2)** in the pop window.

   ![](../media/env25.png)

1. Again, navigate to `.github/workflows`, Click on **Add file (2)** dropdown, and select **+ create new file (3)**.

   ![](../media/env26.png)

1. Copy the code given below, paste it in the window, and name the file as **caller-workflows.yml**

   ```
   name: Caller Workflow

   on:
     push:
       branches: [ "main" ]
     workflow_dispatch:

   jobs:
     call-reusable-workflow:
       uses: ./.github/workflows/reusable-print-message.yml
       with:
         message: "Hello from the caller workflow!"
    ```

   ![](../media/env28.png)   

1. Click on **Commit changes (1)** button and click on **Commit changes (2)** in the pop window.

   ![](../media/env27.png)

1. Now, navigate to **Actions (1)** tab and select **Create caller-workflow.yml (2)**. 

   ![](../media/env29.png)

1. Select the **print-message (1)** job from the side-blade and **expand (2)** the same job in the output window. You'll see that the message from reusable-print-message.yml is fetched by the caller workflow. Hence, this is how the concept of reusable wokflows in GitHub action works.

   ![](../media/env30.png)

### Task 2: Explanation and usage of Matrix builds

Matrix builds and parallelism are advanced features in GitHub Actions that allow you to run multiple jobs concurrently.

Matrix builds let you test your code across multiple environments by creating a job matrix. This is a set of keys and values that create a combination of conditions and run a job for each one.

Parallelism allows you to run jobs or steps concurrently, reducing the total execution time.

1. Naviagte to the [sample-node-project](https://github.com/acemilyalcin/sample-node-project) repo and click on **Fork** **(2)**.


2. Navigate to the **Action** **(1)** directory in your repository, in `Get started with GitHub Actions` click on set up a workflow yourself (2).

    ![](../media/newaction.png)

3. Navigate to the **Code** **(1)** and click on **.github/workflows** **(2)** folder.

    ![](../media/optimize1.png)

4. Provider file name as **nodejs_ci.yml** **(1)**, in the editor **copy and paste** **(2)** the below script, and click in **commit changes** **(3)**.

    ![](../media/optimize2.png)

    ```
    name: Node.js CI
    
    env:
      OUTPUT_PATH: ${{ github.workspace }}
    
    on:
      push:
        branches:
          - master
          - dev
    
    jobs:
      build:
        runs-on: ubuntu-latest
    
        strategy:
          matrix:
            node-version: [18.x]
    
        steps:
          - uses: actions/checkout@v3
    
          - name: Cache Node.js dependencies
            uses: actions/cache@v2
            with:
              path: ~/.npm
              key: ${{ runner.os }}-node-${{ matrix.node-version }}-${{ hashFiles('${{ env.OUTPUT_PATH }}/package-lock.json') }}
              restore-keys: |
                ${{ runner.os }}-node-${{ matrix.node-version }}-
    
          - name: Use Node.js ${{ matrix.node-version }}
            uses: actions/setup-node@v3
            with:
              node-version: ${{ matrix.node-version }}
    ```

    ![](../media/new-workflow.png)

5. In the pop up windows of **Commit Changes** click on the **Commit changes**.

    ![](../media/newcommit.png)

6. Click on **Action** **(1)**, verify the workflow has been executed successfully once the workflow is succedded select the newly created workflow **Create nodejs_ci.yml** **(2)**.

    ![](../media/optimize4.png)

    > Feel free to go through the workflow

### Task 3: Matrix builds for testing across multiple environments.

A matrix build is a CI/CD pipeline strategy that allows you to run tests across a variety of environments simultaneously. Each environment can vary by operating system, programming language version, dependency versions, and other factors. The matrix build configuration defines combinations of these variables, creating a grid (or matrix) of different test scenarios.

- **Comprehensive Testing**: Ensures that your software works under different configurations, reducing the risk of environment-specific bugs.
- **Parallel Execution**: Tests can run in parallel, speeding up the testing process and providing faster feedback.
- **Consistency**: Helps maintain consistent behavior across different environments, which is crucial for cross-platform applications.

1. Navigate back to the `github-action` repo, from the GitHub repository.

2. Navigate to the **Code** **(1)**, Click on **Add File** **(2)** and click on **+ Create new file** **(3)**.

3. Provider file name as **requirements.txt** (1), in the editor **copy and paste** **(2)** the below script, and click in **commit changes** **(3)**.

   ```
   pytest
   ```
   
4. In the pop up windows of Commit Changes click on the **Commit changes** button.

5. Click on **Add File** **(2)** and click on **+ Create new file** **(3)**.   

6. Provider file name as with folder creation **tests/test_sample.py** **(1)**, in the editor **copy and paste** **(2)** the below script, and click in **commit changes** **(3)**.

    ```
    def test_sample():
        print("Running test_sample")
        assert 1 + 1 == 2
        print("Completed test_sample successfully")
    ```

7. In the pop up windows of Commit Changes click on the **Commit changes** button.

8. Navigate to the **Code** **(1)** and click on **.github/workflows** **(2)** folder.

   ![](../media/4th-oidc-click.png)

9. In the **.github/workflows** folder, click on **Add files** **(1)**, and click on **+ Create new file** **(2)**.

   ![](../media/4th-oidc.png)   

10. Provider file name as **matrix.yml** **(1)**, in the editor **copy and paste** **(2)** the below script, and click in **commit changes** **(3)**.

    ```
    name: CI
    
    on:
      push:
        branches:
          - main
      pull_request:
        branches:
          - main
    
    jobs:
      build:
        runs-on: ubuntu-latest
        
        strategy:
          matrix:
            python-version: [3.12]
            os: [ubuntu-latest, windows-latest, macos-latest]
    
        environment: action-environment
    
        steps:
          - name: Checkout repository
            uses: actions/checkout@v2
    
          - name: Set up Python ${{ matrix.python-version }}
            uses: actions/setup-python@v2
            with:
              python-version: ${{ matrix.python-version }}
    
          - name: Install dependencies
            run: |
              python -m pip install --upgrade pip
              pip install -r requirements.txt
    
          - name: Run tests
            run: |
              pytest
    ```

    > **Note**: This CI configuration uses GitHub Actions to run tests on multiple OS (Ubuntu, Windows, macOS) with Python 3.12. It triggers on pushes and pull requests to the main branch, checks out the code, sets up Python, installs dependencies, and runs tests with pytest, ensuring cross-platform compatibility.

12. In the pop up windows of Commit Changes click on the **Commit changes** button.

13. Click on **Action** **(1)**, verifiy the **Create matrix.yml** workflow has been executed successfully.

14. Click on **Create matrix.yml** action, This configuration allows you to ensure your project is tested on multiple operating systems using Python 3.12, ensuring broader compatibility and catching environment-specific issues early.

    ![](../media/matrix-output.png)

### Task 3: Using artifacts and dependencies in workflows

Optimizing workflow performance by caching dependencies can significantly improve the execution time of your workflows. By caching dependencies, you can avoid unnecessary downloads and installations, resulting in faster and more efficient workflows.

- **Identify Dependencies**: Determine which dependencies in your project take a long time to install.
- **Add Cache Step**: In your GitHub Actions workflow file, add a step that uses the `actions/cache@v3` action.
- **Configure Cache Key**: Set the `key` to an expression that uniquely identifies each set of dependencies. This typically includes the package manager's lock file.
- **Specify Path**: Set the `path` to the directory where dependencies are installed.
- **Restore Cache**: If a cache hit occurs, the action restores the cached dependencies.

1. Navigate to the **Code** **(1)** and click on **.github/workflows** **(2)** folder.

    ![](../media/optimize1.png)

2. In the **.github/workflows** folder, select **nodejs_ci.yml** **(1)** and click on **edit** **(2)**.

    ![](../media/optimize2.png)

3. Replace the following code with the below code.

    ```
    name: Node.js CI
    
    env:
      OUTPUT_PATH: ${{ github.workspace }}
    
    on:
      push:
        branches:
          - master
          - dev
    
    jobs:
      build:
        runs-on: ubuntu-latest
    
        steps:
          - uses: actions/checkout@v3
    
          - name: Cache Node.js dependencies
            uses: actions/cache@v2
            with:
              path: ~/.npm
              key: ${{ runner.os }}-node-${{ hashFiles('${{ env.OUTPUT_PATH }}/package-lock.json') }}
              restore-keys: |
                ${{ runner.os }}-node-
    
          - name: Use Node.js
            uses: actions/setup-node@v3
            with:
              node-version: 18.x
    ```
    
    ![](../media/optimize3.png)

4. In the pop up windows of **Commit Changes** click on the **Commit changes**.

    ![](../media/newcommit.png)

5. Click on **Action** **(1)**, verify the workflow has been executed successfully once the workflow is succedded select the newly created workflow **Create nodejs_ci.yml** **(2)**.

    ![](../media/optimize4.png)

    > Feel free to go through the workflow

### Task 4: Code scanning and vulnerability detection

1. Naviage back to `github-action` repo.

1. Click on **Security** **(1)**, and click on **Enable vulnerability reporting** **(2)** next to the Private vulnerability reporting

    ![](../media/security.png)

1. Once naviagted to **Code security and analysis**, click on **Enable** button for Private vulnerability reporting, Dependabot alerts, Dependabot security updates, and Dependabot on Actions runners.

    ![](../media/enable.png)

1. Click on **Set up** **(1)** button to enable CodeQL analysis and select the **Advanced** **(2)** option for creating a CodeQL Analysis YAML file.

   ![](../media/2dgn169.png)      

1. Update the workflow name to **codeql-analysis.yml** ***(1)*** and review the yaml file. Select **Commit changes** ***(2)***, then select **Commit directly to the main branch** ***(3)*** and click on **Commit new file** ***(4)***.
  
   ![](../media/ex5-task1-step3a.png)

   ![](../media/ex5-task1-step3b.png) 
  
1. Navigate to **Actions** ***(1)*** tab, You can review the **workflow** ***(2)*** run.
    
   ![](../media/ex5-codeql-actions.png) 
  
1. Navigate to **Security** ***(1)*** tab and click on **View alerts** ***(2)***.
   
   ![](../media/ex5-codescanning-viewalerts.png)
  
1. You will be navigated to **Code scanning** section. You'll be able to visualize that the **No code scanning alerts here!**.
   
1. Go to **Settings** -> **Code security and analysis** -> scroll down to **Push protection** and click **Enable**.

   ![Picture1](../media/code_security_1.png)

   ![Picture1](../media/push_1.png)

1. After this is enabled, we are going to go back through the process of creating a secret and pasting it into the codebase. Once again, go to your profile and then **Settings** -> **Developer settings** -> **Personal access tokens** -> **Tokens (classic)**. Click on **Generate new token** at the top and select **Generate new token (classic)**.

1. Once again, go to your profile, which is at the top of your right hand, and then select **Settings**.

   ![Picture1](../media/profilesetting.png)

1. Go to **Developer settings** -> **Personal access tokens (1)** -> **Tokens (classic) (2)**, and then click on **Generate new token (3)** at the top. Now select **Generate new token (classic) (4)**.

    ![Picture1](../media/generate_new.png)

1. From here, give your secret a name, **secret2**, set the **Expiration** to **_"Custom..."_** and select the next calendar day. By default, no permissions are granted, so it is safe to scroll to the bottom and click on **Generate token**.

   ![](../media/PATtoken.png)

1. Once you've generated the token, click on the **"Copy"** icon to the right of the secret value.

   ![](../media/token.png)

1. Navigate to **Code** from the top navigation pane.

### Summary

In this, you learn how to securely store sensitive data such as API keys and passwords within a GitHub repository using secrets, ensuring that this data remains protected and separate from the codebase. The code details the process for creating and managing these secrets and how to reference them in workflows to maintain security. Matrix builds enhance workflows by allowing the same workflow to run across multiple configurations, such as different language versions or operating systems. The code demonstrates defining a configuration matrix to run jobs in parallel, which boosts testing efficiency and ensures compatibility across various environments. This comprehensive approach to handling secrets, matrix builds, and artifacts ensures robust security, efficient testing, and effective data management in GitHub workflows.
