# Build CI and CD Pipeline using Azure DevOps - Step by Step

**Azure DevOps** supports a collaborative culture and set of processes that bring together developers, project managers, and contributors to develop software. It allows organizations to create and improve products at a faster pace than they can with traditional software development approaches.

Continuous integration/continuous delivery (CI/CD) pipelines will improve software’s frequent delivery by adapting Azure DevOps services. A CI/CD pipeline will perform automated monitoring and controlling over the application’s delivery to improve its integration and testing phases. 

Continuous Integration(CI) is an automated process that allows any code changes in code to an application to be regularly developed, tested, and merged to a shared repository. Continuous Delivery(CD) represents the deployment of artifacts into the production environment. This will automatically test the bug and upload it to a repository.


## Build steps

- Step 1
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

- Step 3
```
dotnet publish HelloWorldApp.csproj --configuration Realease --output 
```
<img src="/pictures/dotnet_restore_authentication.png" title="dotnet restore authentication"  width="800">





IvEGX0j4xw8