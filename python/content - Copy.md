

## Activity 1 Configure Workshop Environment

Complete the following tasks in order to prepare your environment for this workshop.

Note: These pre-requisites are a hard requirement for successful completion of this workshop. If you do not have all three you will not be able to successfully complete this workshop.

Prerequisites

• Laptop or workstation with administrator rights (Alternatively you can run this workshop virtually in GitHub Codespaces:https://github.com/features/codespaces)
• Azure subscription with owner rights
• Subscription access to Azure OpenAI service. Start here to Request Access to Azure OpenAI Service:https://aka.ms/oaiapply. If you have access, see below for ensuring enough quota to deploy.

   Checking Azure OpenAI quota limits

  For this sample to deploy successfully, there needs to be enough Azure OpenAI quota for the models used by this sample within your subscription. This sample deploys a new Azure OpenAI account with two models, gpt-4o with 30K tokens per minute and text-3-embedding-small with 5k tokens per minute. For more information on how to check your model quota and change it, see Manage Azure OpenAI Service Quota:https://learn.microsoft.com/azure/ai-services/openai/how-to/quota

   Azure Subscription Permission Requirements

  This solution deploys a user-assigned managed identity:https://learn.microsoft.com/entra/identity/managed-identities-azure-resources/overview and defines then applies Azure Cosmos DB and Azure OpenAI RBAC permissions to this as well as your own Service Principal Id. You will need the following Azure RBAC roles assigned to your identity in your Azure subscription or Subscription Owner:https://learn.microsoft.com/azure/role-based-access-control/built-in-roles/privilegedowner access which will give you both of the following.

  • Manged Identity Contributor:https://learn.microsoft.com/azure/role-based-access-control/built-in-roles/identitymanaged-identity-contributor
  • Cosmos DB Operator:https://learn.microsoft.com/azure/role-based-access-control/built-in-roles/databasescosmos-db-operator
  • Cognitive Services OpenAI User:https://learn.microsoft.com/azure/role-based-access-control/built-in-roles/ai-machine-learningcognitive-services-openai-user


Get Started

You can choose from the following options to get started with the workshop.

GitHub Codespaces

You can run this sample app and workshop virtually by using GitHub Codespaces. The button will open a web-based VS Code instance in your browser:

 Open the template (this may take several minutes):

   Open in GitHub Codespaces（https://github.com/codespaces/badge.svg）:https://codespaces.new/AzureCosmosDB/banking-multi-agent-workshop?devcontainerpath=.devcontainer%2Fcsharp%2Fdevcontainer.json

2. Move on to the Deployment:Module-00.mddeployment section.

Local Environment using VS Code Dev Containers

 Install Docker Desktop:https://docs.docker.com/desktop/, and VS Code:https://code.visualstudio.com/Download along with the Dev Containers extension:https://code.visualstudio.com/docs/devcontainers/tutorialinstall-the-extension extension.

2. Clone the repository:

   bash:
++++git clone https://github.com/AzureCosmosDB/banking-multi-agent-workshop/++++

3. Open the repository in VS Code and select Reopen in Container when prompted. When asked to Select a devcontainer.json file, select the C Development Container.

4. Wait for the container to build and start. This is a one time operation and may take a few minutes.

5. Move on to the Deployment:Module-00.mddeployment section.

Local Environment without VS Code Dev Containers

 To run the workshop locally on your machine, install the following:

*  Docker Desktop:https://docs.docker.com/desktop/
*  Git:https://git-scm.com/downloads
*  Azure Developer CLI (azd):https://aka.ms/install-azd
*  .NET 8:https://dotnet.microsoft.com/downloads/
*  Visual Studio:https://visualstudio.microsoft.com/downloads/ or VS Code:https://code.visualstudio.com/Download with C Dev Kit:https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit
*  To build and run the frontend component, install Node.js:https://nodejs.org/en/download/ and Angular CLI:https://angular.dev/installationinstall-angular-cli

2. Clone the repository and navigate to the folder:

 bash:
 ++++git clone https://github.com/AzureCosmosDB/banking-multi-agent-workshop/++++ 
 ++++cd banking-multi-agent-workshop++++ 

3. Move on to the Deployment:Module-00.mddeployment section.

