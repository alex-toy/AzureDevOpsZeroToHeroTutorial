# Build CI and CD Pipeline using Azure DevOps - Step by Step

**Azure DevOps** supports a collaborative culture and set of processes that bring together developers, project managers, and contributors to develop software. It allows organizations to create and improve products at a faster pace than they can with traditional software development approaches.

Continuous integration/continuous delivery (CI/CD) pipelines will improve software’s frequent delivery by adapting Azure DevOps services. A CI/CD pipeline will perform automated monitoring and controlling over the application’s delivery to improve its integration and testing phases. 

Continuous Integration(CI) is an automated process that allows any code changes in code to an application to be regularly developed, tested, and merged to a shared repository. Continuous Delivery(CD) represents the deployment of artifacts into the production environment. This will automatically test the bug and upload it to a repository.

<img src="/pictures/all_steps.png" title="all steps"  width="900">

**Basic Rule** : never do anything on **Azure devops**, unless you know how to do it manually!!


## Manual Build steps

- Step 1 : bring in the necessary packages
```
dotnet restore HelloWorldApp.csproj --interactive
```
<img src="/pictures/dotnet_restore_authentication.png" title="dotnet restore authentication"  width="900">

- Step 2 : will produce binaries on the local machine that can be used for testing.
```
dotnet build HelloWorldApp.csproj --configuration Realease --interactive
```
<img src="/pictures/dotnet_build.png" title="dotnet build"  width="900">

The project can now be launched locally :
```
dotnet C:\path\to\HelloWorldApp\HelloWorldApp\bin\Realease\net6.0\HelloWorldApp.dll
```
<img src="/pictures/dotnet_local.png" title="dotnet launched locally"  width="900">
<img src="/pictures/dotnet_local2.png" title="dotnet launched locally"  width="900">

- Step 3 : publish
```
dotnet publish HelloWorldApp.csproj --configuration Realease --output C:\source\out
```
<img src="/pictures/dotnet_publish.png" title="dotnet publish"  width="900">

Now the files necessary for deployment can be found in the source folder. You can now go into it and execute the .dll file :
```
dotnet C:\source\out\HelloWorldApp.dll
```
<img src="/pictures/dotnet_execute.png" title="dotnet execute dll"  width="900">

and the app is again listening on port 5000.


## Build CI pipeline on Azure devops

- Step 1 : add Azure Devops as repo
```
git remote add originazure https://alexviseo@dev.azure.com/alexviseo/HelloWorldApp/_git/HelloWorldApp
git push -u originazure --all
```
<img src="/pictures/add_repo.png" title="add azure devops as origin for repos"  width="900">

- Step 2 : create a classic pipeline 
<img src="/pictures/classic_pipeline.png" title="classic pipeline"  width="900">
<img src="/pictures/empty_job.png" title="empty job"  width="900">
<img src="/pictures/dotnet_core.png" title="choose dotnet core"  width="900">
<img src="/pictures/tasks.png" title="add tasks"  width="900">

- Step 3 : add configuration and variable

    - add parameters : --configuration $(BuildConfiguration)
    <img src="/pictures/configuration.png" title="configuration"  width="900">

    - add a variable BuildConfiguration = Release
    <img src="/pictures/release.png" title="release"  width="900">

    - add parameters (built-in variable, you don't have to declare it) : --output $(Build.ArtifactStagingDirectory)
    <img src="/pictures/output.png" title="output"  width="900">

- Step 4 : add a **publish Artifact** task

<img src="/pictures/publish_artifact.png" title="publish artifact"  width="900">

- Step 5 : save and queue

<img src="/pictures/save_and_queue.png" title="save and queue"  width="900">
<img src="/pictures/save_and_queue2.png" title="save and queue"  width="900">

In case you have an issue on step dotnet restore, you may need to add a task for installing .NET 6 at the top of the pipeline :

<img src="/pictures/use_dotnet.png" title="use dotnet"  width="900">
<img src="/pictures/use_dotnet2.png" title="use dotnet"  width="900">

You have now access to the artifact produced by the pipeline :

<img src="/pictures/artifact.png" title="artifact"  width="400">
<img src="/pictures/artifact1.png" title="artifact"  width="900">

The content of that zip file is exactly the same the one in the /out folder.


## Build CD pipeline on Azure devops

- Step 1 : go into the **Azure Portal** and create **Web App** for **dev**, **uat** and **prod** :

<img src="/pictures/web_app.png" title="web app"  width="900">

For the **prod** web app, add a deployement slot **staging** :

<img src="/pictures/staging.png" title="deployment slot staging"  width="900">

On the configuration of slot **staging**, set Swap enabled :

<img src="/pictures/swap_enabled.png" title="swap enabled"  width="900">

- Step 2 : go into the **Project Settings** of **Azure Devops** and create a **Service Connection**. Choose **Resource manager** :

<img src="/pictures/service_connection.png" title="service connection"  width="900">

- Step 3 : Create a **release Pipeline**. Choose an empty job.

<img src="/pictures/release_pipeline.png" title="release pipeline"  width="900">

Add artifact :
<img src="/pictures/add_artifact.png" title="add artifact"  width="900">

Add **App Service Deploy** :
<img src="/pictures/app_service_deploy.png" title="app service deploy"  width="900">

Problem : Missing rights to create the connection !!!!

Clone stage **dev** and create stage **uat** as well as **prod** :
<img src="/pictures/stages.png" title="stage uat and prod"  width="900">

In prod, choose **Deploy to Slot or App Service Environment** and add staging :
<img src="/pictures/deploy_slot.png" title="deploy slot"  width="900">

Add a **swap slot** task to prod found under **App Service Manage** :
<img src="/pictures/swap_slot.png" title="swap slot"  width="900">

Add approval conditions :
<img src="/pictures/approval.png" title="approval"  width="900">