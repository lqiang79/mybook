# Continuous Integration with Atlassian Products(JIRA, Bitbuckt, Bamboo and Artifactory)

In the architecture we have the following components, here are their roll by the CI plan.

1. **Bitbuckt** is the code repository
1. **Artifactory** is the dependency repositories for 3th. party code and also the artifact repositories for own projects
1. **JIRA** is the issure and task management tools
1. **Bamboo** is build server for build and testing 
1. **SonarQube**(optional) is the code quality gate


## Workflow
Here is the general workflow of our continuous integration process:

1. User start a JIRA ticket/task to build a new functionality into the project. He create a new *feature branch* in the Bitbuckt based on this JIRA task.
1. After the implementation and local testing the developer commits his code changes into the Bitbucket. This commit will triggt the build and test jobs in the Bamboo **feature plan**. Here is only the build and testing. No project package will be produced.
1. A positive feedback from the **feature plan** makes sure, the code has filled the basic code quality rules. It can be merged into the develop branch. The developer send a *pull request* to code reviewer to prepare merging into the master/develop branch.
1. After code review, the code will be merged into the develop branch. Now the **develop plan** will be trigged, the merged code will be rebuild and tested. If everything is gone without any errors, the project package(s) will be produces and deploy into the *Artifactory* server. At the same time, the product will be deploy into the integration server and restarted. It happens in *Bamboo* **deploy plan**.
1. A manuel action in Bamboo can *deploy* the save product into the QA enviroment.
1. To keep the safety in the production enviroment, only a special user can run the deploy job to production and promot the product into the correct stage in *Artifactory*.