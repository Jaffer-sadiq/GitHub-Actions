# Getting Started with GitHub

### What is GitHub?

**GitHub** is a cloud-based platform that uses Git, a distributed version control system, at its core. The GitHub platform simplifies the process of collaborating on projects and provides a website, command-line tools, and overall flow that allows developers and users to work together.

As we learned earlier GitHub provides an AI powered developer platform to build, scale, and deliver secure software. Let’s break down each one of the core pillars of the GitHub Enterprise platform, AI, Collaboration, Productivity, Security and Scale.

### Benefits of Using GitHub

1. **Collaboration:** GitHub makes it easy for multiple developers to work together on a project, regardless of their physical location. With features like branches, pull requests, and code reviews, teams can work in parallel without interfering with each other's work.

2. **Version Control:** Every change made to a project is tracked and recorded, making it easy to revert to previous versions if needed. This history of changes is valuable for understanding how a project has evolved.

3. **Community and Open Source:** GitHub hosts millions of open-source projects. Developers can contribute to these projects, learn from each other, and showcase their work to the global developer community.

4. **Integration:** GitHub integrates with numerous tools and services, from continuous integration and deployment (CI/CD) to project management and code review tools, streamlining the development workflow.

## Key Features of GitHub

1. **What is a repository?**
    
    - A repository contains all of your project's files and each file's revision history. It is one of the essential parts that helps you collaborate with people. You can use repositories to manage your work, track changes, store revision history and work with others. Before we dive too deep, let’s first start with how to create a repository.

2. **What are branches**
   
    - In the last section, we created a new file in the last section, along the way to you also created a new branch in your repositories.
    - Branches are an essential part to the GitHub experience because they're where we can make changes without affecting the entire project we're working on.
    - Your branch is a safe place to experiment with new features or fixes. If you make a mistake, you can revert your changes or push more changes to fix the mistake. Your changes won't update on the default branch until you merge your branch.

3. **What are commits**

   - A **commit** is a change to one or more files on a branch. Every time a commit is created, it's assigned a unique ID and tracked, along with the time and contributor. Commits provide a clear audit trail for anyone reviewing the history of a file or linked item, such as an issue or pull request.
    
   ![](../media/2-commits.png)
   
     - Within a git repository, a file can exist in several valid states as it goes through the version control process:
     - The primary states for a file in a Git repository are:
     - **Untracked**: An initial state of a file when it isn't yet part of the Git repository. Git is unaware of its existence.
     - **Tracked**: A tracked file is one that Git is actively monitoring. It can be in one of the following substates:
     - **Unmodified**: The file is tracked, but it hasn't been modified since the last commit.
     - **Modified**: The file has been changed since the last commit, but these changes aren't yet staged for the next commit.
     - **Staged**: The file has been modified, and the changes have been added to the staging area (also known as the index). These changes are ready to be committed.
     - **Committed**: The file is in the repository's database. It represents the latest committed version of the file.

These states and substates are important to collaborating with your team to know where each and every commit is in the process of your project.

4. **What are pull requests?**

   - A pull request is the mechanism used to signal that the commits from one branch are ready to be merged into another branch.The team member submitting the pull request requests one or more reviewers to verify the code and approve the merge. These reviewers have the opportunity to comment on changes, add their own, or use the pull request itself for further discussion. Once the changes have been approved (if approval is required), the pull request's source branch (the compare branch) is merged into the base branch.

     ![](../media/2-pull-request.png)

     #### The GitHub flow
   
     ![](../media/2-branching.png)

     - The GitHub flow can be defined as a lightweight workflow that allows for safe experimentation. You can test new ideas and collaboration with your team by using branching, pull requests, and merging.Now that we know the basics of GitHub we can walk through the GitHub flow and its components.

     i. The first step of the GitHub flow is creating a branch so that the changes, features, and fixes you create don't affect the main branch. <br/>
     ii. The second step is to make your changes. We recommend deploying changes to your feature branch before merging into the main branch. Doing so ensures the changes are valid in a production environment. <br/>
     iii. The third step is to create a pull request to ask collaborators for feedback. Pull request review is so valuable that some repositories require an approving review before pull requests can be merged. <br/>
     iv. Next is the fourth step of reviewing and implementing your feedback from your collaborators. <br/>
     v. The fifth step, once you’re feeling great about your changes now it's time to get your pull request approved and merge it into the main branch. <br/>
     vi. The sixth and final step is to delete your branch. Deleting your branch signals your work on the branch is completed and prevents you or others from accidentally using old branches. <br/>

And that’s it, you’ve been through a GitHub flow cycle!

5. **Forking:** Forking is copying a repository from one user’s account to another. It allows you to freely experiment with changes without affecting the original project.

6. **GitHub Issues:** Issues are a way to track enhancements, tasks, and bugs for your projects. They help you manage and prioritize work, discuss ideas, and keep track of tasks.

### How does GitHub Actions automate development tasks?

#### GitHub decreases time from idea to deployment

GitHub is designed to help teams of developers and DevOps engineers build and deploy applications quickly. There are many features in GitHub that enable this, but they generally fall into one of two categories:

- **Communication:** Consider all of the ways that GitHub makes it easy for a team of developers to communicate about the software development project: code reviews in pull requests, GitHub issues, project boards, wikis, notifications, and so on.
  
- **Automation:** GitHub Actions lets your team automate workflows at every step in the software-development process, from integration to delivery to deployment. It even lets you automate adding labels to pull requests and checking for stale issues and pull requests.

#### Use workflow automation to decrease development time

We'll focus on automation in this module, so let's take a moment to understand how teams can use automation to reduce the amount of time it takes to complete a typical development and deployment workflow.

Consider all of the tasks that must happen after the code is written, but before you can reliably use the code for its intended purpose. Depending on your organization's goals, you'll likely need to perform one or more of the following tasks:

- Ensure the code passes all unit tests
- Perform code quality and compliance checks to make sure the source code meets the organization's standards
- Check the code and its dependencies for known security issues
- Build the code integrating new source from (potentially) multiple contributors
- Ensure the software passes integration tests
- Version the new build
- Deliver the new binaries to the appropriate filesystem location
- Deploy the new binaries to one or more servers
- If any of these tasks don't pass, report the issue to the proper individual or team for resolution

### What is GitHub Actions?

GitHub Actions are packaged scripts designed to automate tasks within a software development workflow on GitHub. These actions can be configured to trigger complex workflows that meet your organization's specific needs. They can be set off whenever developers check new source code into a particular branch, at scheduled intervals, or manually. This results in a reliable and sustainable automated workflow, significantly reducing development time.

These scripts follow a YAML data format. Each repository features an Actions tab, providing a straightforward way to set up your first script. If you find a workflow that seems like a good starting point, simply click the Configure button to add the script and start editing the YAML source.

[Introduction to GitHub](https://learn.microsoft.com/en-us/training/modules/introduction-to-github/)