### Activity 4: Configure Environment Variables

Setting up local debugging

When you deploy this solution it automatically injects endpoints and configuration values into the .env file stored in the .azure directory in a folder with the name of your resource group.
* 
 * Navigate to .azure\[your-resource-group-name]\.env
* 
 * Open the .env file using any text editor.
* 
 * Navigate to csharp\src\MultiAgentCopilot.sln and open the solution.
* 
 * Within your IDE, navigate to ChatAPI project and open appsettings.json
* 
 * Update "CosmosDBSettings:CosmosUri": "https://[accountname].documents.azure.com:443/" with the AZURECOSMOSDBENDPOINT value from the .env file.
* 
 * Update "SemanticKernelServiceSettings:AzureOpenAISettings:Endpoint": "https://[accountname].openai.azure.com/" with the AZUREOPENAIENDPOINT value from the .env file.
* 
 * Update "ApplicationInsights:ConnectionString": "[connectionstring]" with the APPINSIGHTSCONNECTION_STRING value from the .env file.

