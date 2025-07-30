**@lab.Title**

## Module 00 - Prerequisites - Deployment and Setup

Introduction

In this Module, you'll deploy the Azure Services needed to run this workshop and get your local environment configured and ready. You will also learn about the structure of this workshop and get an overview of Multi-Agent Systems.

Learning Objectives and Activities

• Begin the deployment of the Azure Services
• Learn the structure and get an overview of this workshop
• Explore the core principals of multi-agent systems
• Complete the configuration of your local environment
• Compile and run the starter solution locally

#### Module Exercises

 * Activity 1: Configure Workshop Environment:activity-1-configure-workshop-environment
 * Activity 2: Deploy Azure Services:activity-2-deploy-azure-services
 * Activity 3: Workshop Structure and Overview Session:activity-3-workshop-structure-and-overview-session
 * Activity 4: Configure Environment Variables:activity-4-configure-environment-variables
 * Activity 5: Compile and Run:activity-5-compile-and-run


===

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
git clone https://github.com/AzureCosmosDB/banking-multi-agent-workshop/

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

===

### Activity 2: Deploy Azure Services

Deployment

 From the terminal, switch to the start branch:

   bash:
++++git checkout start++++

 Navigate to the correct folder:

   bash:
++++cd csharp/infra++++

 Log in to Azure using AZD.

   bash:
++++azd auth login++++

 Provision the Azure services and deploy the application.

   bash:
++++azd up++++

This step will take approximately 10-15 minutes.

[!IMPORTANT] 
> [!help] If you encounter any errors during the deployment, rerun azd up to continue the deployment from where it left off. This will not create duplicate resources, and tends to resolve most issues.


===

Activity 3: Workshop Structure and Overview Session

While the Azure Services are deploying we will have a presentation to cover on the structure for this workshop for today as well as provide an introduction and overview of multi-agent sytems.

===

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

===

### Activity 5: Compile and Run

Running the ChatAPI and Frontend App

 Start the ChatAPI

If running on Codespaces

 Navigate to src\ChatAPI.
2. Run the following command to trust the development certificate:

   sh:
++++dotnet dev-certs https --trust++++

3. Start the application:

   sh:
++++dotnet run++++

4. In the Ports tab, right-click and select the Port Visibility option to set port 63280 as Public.
5. Copy the URL for 63280 port.

If running locally on Visual Studio or VS Code

 Navigate to src\ChatAPI.
2. Press F5 or select Run to start the application.
3. Copy the URL from the browser window that opens.

2. Run the Frontend App

 Open a new terminal. Navigate to the frontend folder.
 Copy and run the following:

   sh:
++++npm i++++
  ++++ng serve --host 0.0.0.0++++

If running locally

 Open your browser and navigate to <http://localhost:4200/>.

If running on Codespaces

 From the PORTS tab, search for the port with the label Frontend app. Hover over the address and choose Open in Browser (second icon) to access the frontend application.

3. Start a Conversation

 Open the frontend app.
 Start a new conversation.
 Send the message:

   text:
Hello, how are you?

 You should see something like the output below.

   Test output（./media/module-00/test-output.png）

4. Stop the Application

• In the frontend terminal, press Ctrl + C to stop the application.
• In your IDE press Shift + F5 or stop the debugger.

Deployment Validation

Use the steps below to validate that the solution was deployed successfully.

• [ ] All Azure resources are deployed successfully
• [ ] You can compile the solution in CodeSpaces or locally
• [ ] You can start the project and it runs without errors
• [ ] You are able to launch the Chat Frontend app , create a new chat session, and get a reply when you send a message.

Common Issues and Troubleshooting

 Errors during azd deployment:
   • Service principal "not found" error.
   • Rerun azd up
 Azure OpenAI deployment issues:
   • Ensure your subscription has access to Azure OpenAI
   • Check regional availability
 Frontend issues:
   • If frontend doesn't fully start, navigate to /frontend/src/environments/environment.ts and update apiUrl: 'https://localhost:63279/'
   • Frontend will restart
 Connecting to backend running CodeSpaces

   • If you cannot get the front end to connect to the backend service when running in Codespaces try the following

     • Navigate to the /src/ChatAPI folder in the Terminal
     • Run the following command to trust the development certificate:

       sh:
++++dotnet dev-certs https --trust++++

     • Then start the application:

       sh:
++++dotnet run++++

   • Copy the URL from the Ports tab and use this for the environments.ts file

Success Criteria

To complete this Module successfully, you should be able to:

• Verify that all services have been deployed successfully.
• Have an open IDE or CodeSpaces session with the source code and environment variables loaded.
• Be able to compile and run the application with no warnings or errors.

Next Steps

===

#### Proceed to Creating Your First Agent:./Module-0md

Resources

• azd Command Reference:https://learn.microsoft.com/azure/developer/azure-developer-cli/reference

• Semantic Kernel Agent Framework:https://learn.microsoft.com/semantic-kernel/frameworks/agent

• LangGraph:https://langchain-ai.github.io/langgraph/concepts/

• Azure OpenAI Service documentation:https://learn.microsoft.com/azure/cognitive-services/openai/

• Azure Cosmos DB Vector Database:https://learn.microsoft.com/azure/cosmos-db/vector-database

===

### Module 01 - Creating Your First Agent


#### Introduction

In this Module, you'll implement your first agent as part of a multi-agent banking system implemented using either Semantic Kernel Agent Framework. You will get an introduction to Semantic Kernel and their plug-in integration with OpenAI for generating completions.

#### Learning Objectives and Activities

• Learn the basics for Semantic Kernel Agent Framework
• Learn how to integrate agent frameworks to Azure OpenAI
• Build a simple chat agent

Module Exercises

 Activity 1: Session on Single-agent architecture:activity-1-session-on-single-agent-architecture
 Activity 2: Session on Semantic Kernel Agent Framework and LangGraph:activity-2-session-on-semantic-kernel-agent-framework-and-langgraph
 Activity 3: Instantiate Agent Framework and Connect to Azure OpenAI:activity-3-instantiate-semantic-kernel-agent-framework-and-connect-to-azure-openai
 Activity 4: Create a Simple Agent:activity-4-create-a-simple-agent
 Activity 5: Test your Work:activity-5-test-your-work


Activity 1: Session on Single-agent architecture

In this session you will get an overview of Semantic Kernel Agents and LangGraph and learn the basics for how to build a chat app that interacts with a user and generations completions using an LLM powered by Azure OpenAI.

Activity 2: Session on Semantic Kernel Agent Framework and LangGraph

In this session ou will get a deeper introduction into the Semantic Kernel Agent Framework and LangGraph with details on how to implement plug-in or tool integration with Azure Open AI.

===

Activity 3: Instantiate Semantic Kernel Agent Framework and Connect to Azure OpenAI

In this hands-on exercise, you will learn how to initialize an agent framework and integrate it with a Large Language Model(LLM).

The Semantic Kernel Agent Framework is a platform within Microsoft's Semantic Kernel ecosystem designed to facilitate the creation and orchestration of AI agents. It enables developers to incorporate agentic patterns into applications, leveraging the core features of the Semantic Kernel framework. Agents built using this framework can collaborate, manage multiple concurrent conversations, and integrate human input, making it suitable for complex, multi-agent workflows.

There are a few key components and concepts necessary to build multi-agent apps using this framework.

• AgentChat - In the Semantic Kernel framework, AgentChat is an abstract class designed to facilitate interactions among multiple agents, even if they are of different types. This design allows, for example, a ChatCompletionAgent and an OpenAIAssistantAgent which are concrete implementations to collaborate within the same conversation. The AgentChat class defines entry points for initiating collaboration between agents, supporting scenarios where multiple responses or a single agent response are required. As an abstract class, AgentChat can be subclassed to support custom scenarios.

• Functions - Functions in Semantic Kernel are discrete units of work that can be executed within the AI application. They come in two primary forms:
  • Native - These are standard methods written in a programming language like C or Python. They perform specific tasks, such as accessing a database, calling an external API, or processing data. To define a native function, you create a method in your codebase and annotate it with attributes that describe its purpose and parameters. This metadata allows the Semantic Kernel to understand and utilize the function appropriately.
  • Semantic - These functions are defined using natural language prompts and are designed to interact with LLMs. They guide the model to generate responses or perform tasks based on the provided prompt. Semantic functions are typically stored as text files, (for example, Prompty) containing the prompt and are loaded into the application at runtime. They can include variables and expressions to make the prompts dynamic and context-aware.

• Plugins - A plugin in Semantic Kernel is a collection of related functions-both native and semantic-that are grouped to provide a cohesive set of capabilities. Plugins serve as modular components that can be easily integrated into AI applications to extend their functionality.​ A plugin is typically implemented as a class containing multiple functions. Each function within the plugin is annotated with descriptive metadata, enabling the Semantic Kernel to understand its purpose and how to invoke it. Plugins encapsulate functionality that can be reused across different parts of an application or even across multiple applications. They promote modularity and maintainability by organizing related functions into a single, coherent unit. To use a plugin, it is imported into the Semantic Kernel, which then makes its functions available for invocation. This integration allows AI applications to dynamically utilize the plugin's capabilities as needed.

• Connectors - ​In the Semantic Kernel framework, Connectors serve as essential bridges that facilitate seamless integration between the kernel and various external services and AI models. These connectors enable developers to incorporate diverse AI functionalities-such as text generation, chat completion, embeddings, Vector Search, and more-into their applications without delving into the complexities of each AI provider's API. There are two types of connectors we will use in this workshop, AI Connectors and Vector Store Connectors.
  • AI Connectors - AI Connectors provide a uniform interface to interact with multiple AI services, such as Chat Completion and Embedding Generation, allowing developers to switch between different AI providers effortlessly. This flexibility is particularly beneficial for experimenting with various models to determine the best fit for specific use cases.
  • Vector Store Connectors - Beyond direct AI service integrations, Semantic Kernel provides connectors for various vector databases, including Azure Cosmos DB, facilitating tasks like semantic search and retrieval-augmented generation (RAG).

Let's dive into the starter solution for our workshop and get started completing the implementation for our multi-agent application.

The following steps are completed in your IDE.

Project Structure

This solution is initially organized with the following three projects.

• BankingServices this project provides a data service layer for core the banking functionality for our application.
• ChatAPI this project wraps backend service layer for this solution. It exposes REST endpoints that are called by our frontend Angular app.
• ChatInfrastructure is the backend service layer itself. It provides the core wrappers for the underlying Azure Services and the Semantic Kernel Agent Framework and defines interfaces for each of the major service layers. Within this project the key entry point for the ChatAPI service above is the ChatService. All user interaction enters and leaves through this service.
• Common provides components that are shared across the entire solution including entity models, configuration, helpers as well as exception handling and debugging.

Here is what the structure of this solution appears like in Visual Studio. Spend a few moments to familiarize yourself with the structure as it will help you navigate as we go through the activities.

Solution Files（./media/module-01/solution-files.png）

Implement the SemanticKernelService

We are going to define an interface and the service layer for the Semantic Kernel Agent Framework that is central to our workshop. This interface defines the main functionality for our multi-agent service.

There are three functions we will implement as part of our multi-agent application.

• GetResponse() will be the entry point called by the front end to interact with the multi-agent service. Everything happens behind this function.
• Summarize() will be used to summarize the conversations users are having with the agent service.
• GenerateEmbedding() will be used to generate embeddings that are used to do vector searches for product data stored in Cosmos DB.

In your IDE, navigate to the ChatInfrastructure project in the solution.

Then navigate to the \Interfaces\ folder within it.

Create a new class, ISemanticKernelService.cs

Replace the contents of the new class with the code below.

```csharp
using MultiAgentCopilot.Common.Models.Chat;
using MultiAgentCopilot.Common.Models.Debug;

namespace MultiAgentCopilot.ChatInfrastructure.Interfaces
{
    public interface ISemanticKernelService
    {
        Task<Tuple<List<Message>, List<DebugLog>>> GetResponse(Message userMessage, List<Message> messageHistory, string tenantId, string userId);
        Task<string> Summarize(string sessionId, string userPrompt);
        Task<float[]> GenerateEmbedding(string text);
    }
}
```
Next we will create the service class itself for Semantic Kernel that implements this interface.

Navigate to ChatInfrastructure\Services\ in your solution.

Create a new class, SemanticKernelService.cs

Replace the contents of the new class with the code below.

```csharp:
using System;
using System.Runtime;
using System.Data;
using Newtonsoft.Json;
using Azure.Identity;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Options;

using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.Agents;
using Microsoft.SemanticKernel.Connectors.OpenAI;
using Microsoft.SemanticKernel.ChatCompletion;
using Microsoft.SemanticKernel.Embeddings;

using MultiAgentCopilot.Common.Models.Chat;
using MultiAgentCopilot.ChatInfrastructure.Interfaces;
using MultiAgentCopilot.Common.Models.Configuration;
using MultiAgentCopilot.Common.Models.Debug;
using Message = MultiAgentCopilot.Common.Models.Chat.Message;

namespace MultiAgentCopilot.ChatInfrastructure.Services;

public class SemanticKernelService : ISemanticKernelService, IDisposable
{
    readonly SemanticKernelServiceSettings settings;
    readonly ILoggerFactory loggerFactory;
    readonly ILogger<SemanticKernelService> logger;
    readonly Kernel semanticKernel;

    bool serviceInitialized = false;
    string prompt = string.Empty;
    string contextSelectorPrompt = string.Empty;

    List<LogProperty> promptDebugProperties;  

    public bool IsInitialized => serviceInitialized;

    public SemanticKernelService(
        IOptions<SemanticKernelServiceSettings> options,
        ILoggerFactory loggerFactory)
    {
        settings = options.Value;
        loggerFactory = loggerFactory;
        logger = loggerFactory.CreateLogger<SemanticKernelService>();
        promptDebugProperties = new List<LogProperty>();

        logger.LogInformation("Initializing the Semantic Kernel service...");

        var builder = Kernel.CreateBuilder();

        builder.Services.AddSingleton<ILoggerFactory>(loggerFactory);

        DefaultAzureCredential credential;
        if (string.IsNullOrEmpty(settings.AzureOpenAISettings.UserAssignedIdentityClientID))
        {
            credential = new DefaultAzureCredential();
        }
        else
        {
            credential = new DefaultAzureCredential(new DefaultAzureCredentialOptions
            {
                ManagedIdentityClientId = settings.AzureOpenAISettings.UserAssignedIdentityClientID
            });
        }
        builder.AddAzureOpenAIChatCompletion(
            settings.AzureOpenAISettings.CompletionsDeployment,
            settings.AzureOpenAISettings.Endpoint,
            credential);

        builder.AddAzureOpenAITextEmbeddingGeneration(
               settings.AzureOpenAISettings.EmbeddingsDeployment,
               settings.AzureOpenAISettings.Endpoint,
               credential);

        semanticKernel = builder.Build();

        Task.Run(Initialize).ConfigureAwait(false);
    }

    public void Dispose()
    {
        // Dispose resources if any
    }

    private Task Initialize()
    {
        try
        {
            serviceInitialized = true;
            logger.LogInformation("Semantic Kernel service initialized.");
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Semantic Kernel service was not initialized. The following error occurred: {ErrorMessage}.", ex.ToString());
        }
        return Task.CompletedTask;
    }

    private void LogMessage(string key, string value)
    {
        promptDebugProperties.Add(new LogProperty(key, value));
    }

    public async Task<Tuple<List<Message>, List<DebugLog>>> GetResponse(Message userMessage, List<Message> messageHistory,  string tenantId, string userId)
    {

        try
        {
            //GetResponse() code goes here

        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Error when getting response: {ErrorMessage}", ex.ToString());
            return new Tuple<List<Message>, List<DebugLog>>(new List<Message>(), new List<DebugLog>());
        }
    }

    public async Task<string> Summarize(string sessionId, string userPrompt)
    {
        try
        {
            // Use an AI function to summarize the text in 2 words
            var summarizeFunction = semanticKernel.CreateFunctionFromPrompt(
                "Summarize the following text into exactly two words:\n\n{{input}}",
                executionSettings: new OpenAIPromptExecutionSettings  MaxTokens = 10 
            );

            // Invoke the function
            var summary = await semanticKernel.InvokeAsync(summarizeFunction, new()  ["input"] = userPrompt );

            return summary.GetValue<string>() ?? "No summary generated";
        }
        catch (Exception ex)
        
            logger.LogError(ex, "Error when getting response: {ErrorMessage", ex.ToString());
            return string.Empty;
        }
    }

    public  async Task<float[]> GenerateEmbedding(string text)
    
        // Generate Embedding
        var embeddingModel = semanticKernel.Services.GetRequiredService<ITextEmbeddingGenerationService>();

        var embedding = await embeddingModel.GenerateEmbeddingAsync(text);

        // Convert ReadOnlyMemory<float> to IList<float>
       return embedding.ToArray();
    
}
```

===

### Activity 4: Create a Simple Agent

In this activity we are going to create a simple customer service agent. This agent is going to be the key agent that will interact with users. However, right now, we are going to only implement a very simple agent that will take your user input and translate it into French. Later on we will greatly expand this customer service agent's functinality.

In Semantic Kernel all agents are derived from an abstract Agent class. Semantic Kernel provides a few built-in Agent classes including a ChatCompletionAgent, OpenAI Assistant Agent which are both derived by the AgentChat class in Semantic Kernel. There are also some experimental agents such as Azure AI Agent and Agent Collaboration. You can explore these on your own.

Implement GetResponse() function

In our application the GetResponse() function is the key entry point for users.

In our example here we are going to implement a very simple ChatCompletionAgent. This is one of the built-in agents from Semantic Kernel Agent Framework and a key component for building these types of applications. The design for this agent is simple. It simply takes user input and translates the responses into French.

Within the GetResponse() function of  SemanticKernelService.cs, inside the Try..Catch block, copy the following code:

```csharp:
ChatCompletionAgent agent = new ChatCompletionAgent
            
                Name = "BasicAgent",
                Instructions = "Greet the user and translate the request into French",
                Kernel = semanticKernel.Clone()
            ;

            ChatHistory chatHistory = [];

            chatHistory.AddUserMessage(userMessage.Text);

            promptDebugProperties = new List<LogProperty>();

            List<Message> completionMessages = new();
            List<DebugLog> completionMessagesLogs = new();

            ChatMessageContent message = new(AuthorRole.User, userMessage.Text);
            chatHistory.Add(message);

            await foreach (ChatMessageContent response in agent.InvokeAsync(chatHistory))
            
                string messageId = Guid.NewGuid().ToString();
                completionMessages.Add(new Message(userMessage.TenantId, userMessage.UserId, userMessage.SessionId, response.AuthorName ?? string.Empty, response.Role.ToString(), response.Content ?? string.Empty, messageId));
                        
            return new Tuple<List<Message>, List<DebugLog>>(completionMessages, completionMessagesLogs);
```
Add Dependency Injection for SemanticKernel

We next need to add some boiler plate code to our solution to support the new SemanticKernelService classes we've imlemented.

Navigate to ChatInfrastructure\Services\

Open the DependencyInjection.cs class. Copy the following code inside the class

```csharp:
/// <summary>
        /// Registers the <see cref="ISemanticKernelService"/> implementation with the dependency injection container.
        /// </summary>
        /// <param name="builder">The hosted applications and services builder.</param>
        public static void AddSemanticKernelService(this IHostApplicationBuilder builder)
        
            builder.Services.AddOptions<SemanticKernelServiceSettings>()
                .Bind(builder.Configuration.GetSection("SemanticKernelServiceSettings"));
            builder.Services.AddSingleton<ISemanticKernelService, SemanticKernelService>();
```
Integrate SemanticKernelService to ChatService

As described earlier, the ChatService is the entry point for all user interaction with this application. The ChatAPI exposes all of the functions implemented by the ChatService and it subsequently calls every other servic within this application. Some of the functionality for this application we will implement here but that is largely centered on the user chat elements such as managing the chat history for the agents as well as integrating it into Semantic Kernel which it will forward requests on behalf of the user.

Let's begin to implement some of these elements for our application.

Within the same ChatInfrastructure\Services\ folder, open the ChatService.cs

At the top of the ChatService : IChatService class, add the following line to the class declarations under ICosmosDBService and ILogger

```csharp:
private readonly ISemanticKernelService skService;
```
Next, replace the existing ChatService constructor to the following code:

```csharp:
public ChatService(
        IOptions<CosmosDBSettings> cosmosOptions,
        IOptions<SemanticKernelServiceSettings> skOptions,
        ICosmosDBService cosmosDBService,
        ISemanticKernelService skService,
        ILoggerFactory loggerFactory)
    
        cosmosDBService = cosmosDBService;
        skService = skService;
        bankService = new BankingDataService(cosmosOptions.Value, skOptions.Value, loggerFactory);
        logger = loggerFactory.CreateLogger<ChatService>();
```

Integrate Semantic Kernel to Chat Service

In this step, you integrate the chat service to now call the GetResponse() method we pasted earlier for our a simple customer service agent.

Within the same ChatService.cs navigate to the GetChatCompletionAsync() function

Update the code within the function to this:

```csharp:
public async Task<List<Message>> GetChatCompletionAsync(string tenantId, string userId,string? sessionId, string userPrompt)
    
        try
        {
            ArgumentNullException.ThrowIfNull(sessionId);
            
            // Add both prompt and completion to cache, then persist in Cosmos DB
            var userMessage = new Message(tenantId, userId, sessionId, "User", "User", userPrompt);

            // Generate the completion to return to the user
            var result = await skService.GetResponse(userMessage, new List<Message>(), tenantId, userId);

            return result.Item1;
        
        catch (Exception ex)
        {
            logger.LogError(ex, "Error getting completion in session {sessionId} for user prompt [{userPrompt}].");
            return new List<Message> { new Message(tenantId, userId, sessionId!, "Error", "Error", "Error getting completion in session sessionId for user prompt [userPrompt].") };
        }
    }
```
This next function gets called by the service to rename chat session to give it a meaningful name. It does this by calling the semantic kernel service which passes the prompt text to an LLM and asks it to summarize the next. It then calls a second function that renames the chat and stores the new name in Cosmos DB.

In the next function below, SummarizeChatSessionNameAsync() replace the existing code for that function with this below.

```csharp:
public async Task<string> SummarizeChatSessionNameAsync(string tenantId, string userId,string? sessionId, string prompt)
    
        try
        {
            ArgumentNullException.ThrowIfNull(sessionId);

            var summary = await skService.Summarize(sessionId, prompt);

            var session = await RenameChatSessionAsync(tenantId, userId,sessionId, summary);

            return session.Name;
        
        catch (Exception ex)
        {
            logger.LogError(ex, "Error getting a summary in session {sessionId} for user prompt [{prompt}].");
            return "Error getting a summary in session sessionId for user prompt [prompt].";
        }
    }
```

Final step, initialize the Semantic Kernel service

We are though all of the implementation elements for this module. The last step is to initialize the new SemanticKernelService we've just created.

In your IDE, navigate to the ChatAPI project. Then open Program.cs

Within the file, search for builder.AddCosmosDBService(); and paste builder.AddSemanticKernelService(); in the line below it so the code looks like this:

```csharp:
builder.AddCosmosDBService();
builder.AddSemanticKernelService();
```
===

####Activity 5: Test your Work

With the activities in this module complete, it is time to test your work.

Running the ChatAPI and Frontend App

 Start the ChatAPI

• Codespaces open a terminal and type dotnet run
• In your IDE press F5 or select Run to start the application.

2. Run the Frontend App

• Open a new terminal or use an existing one that is open to the /frontend folder.

```sh:
ng serve
```
• Navigate to, <http://localhost:4200> in your browser

3. Start a Chat Session

 Login to the frontend app.
 Start a new conversation.
 Send the message:  

   ```text:
Hello, how are you?
```
 You should see something like the output below.

    Test output in French（./media/module-01/test-output.png）

4. Stop the Application

• In the frontend terminal, press Ctrl + C to stop the application.
• In your IDE press Shift + F5 or stop the debugger.

Validation Checklist

Your implementation is successful if:

• [ ] Your app compiles with no warnings or errors.
• [ ] Your agent successfully processes user input and generates and appropriate response.

Common Issues and Troubleshooting

 Frontend issues:
   • If frontend doesn't fully start, navigate to /frontend/src/environments/environment.ts and update apiUrl: 'https://localhost:63279/'
   • Frontend will restart

===

### Module Solution

The following sections include the completed code for this Module. Copy and paste these into your project if you run into issues and cannot resolve.

<details>
  <summary>Completed code for <strong>ChatInfrastructure/Interfaces/ISemanticKernelService.cs</strong></summary>
<br>

```csharp:
using MultiAgentCopilot.Common.Models.Chat;
using MultiAgentCopilot.Common.Models.Debug;

namespace MultiAgentCopilot.ChatInfrastructure.Interfaces

    public interface ISemanticKernelService
    {
        Task<Tuple<List<Message>, List<DebugLog>>> GetResponse(Message userMessage, List<Message> messageHistory, string tenantId, string userId);
        Task<string> Summarize(string sessionId, string userPrompt);
        Task<float[]> GenerateEmbedding(string text);
    
}
```
</details>

<details>
  <summary>Completed code for <strong>ChatInfrastructure/Services/SemanticKernelService.cs</strong></summary>
<br>

```csharp:
using System;
using System.Runtime;
using System.Data;
using Newtonsoft.Json;
using Azure.Identity;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Options;

using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.Agents;
using Microsoft.SemanticKernel.Connectors.OpenAI;
using Microsoft.SemanticKernel.ChatCompletion;
using Microsoft.SemanticKernel.Embeddings;

using MultiAgentCopilot.Common.Models.Chat;
using MultiAgentCopilot.ChatInfrastructure.Interfaces;
using MultiAgentCopilot.Common.Models.Configuration;
using MultiAgentCopilot.Common.Models.Debug;
using Message = MultiAgentCopilot.Common.Models.Chat.Message;

namespace MultiAgentCopilot.ChatInfrastructure.Services;

public class SemanticKernelService : ISemanticKernelService, IDisposable

    readonly SemanticKernelServiceSettings settings;
    readonly ILoggerFactory loggerFactory;
    readonly ILogger<SemanticKernelService> logger;
    readonly Kernel semanticKernel;

    bool serviceInitialized = false;
    string prompt = string.Empty;
    string contextSelectorPrompt = string.Empty;

    List<LogProperty> promptDebugProperties;

    public bool IsInitialized => serviceInitialized;

    public SemanticKernelService(
        IOptions<SemanticKernelServiceSettings> options,
        ILoggerFactory loggerFactory)
    {
        settings = options.Value;
        loggerFactory = loggerFactory;
        logger = loggerFactory.CreateLogger<SemanticKernelService>();
        promptDebugProperties = new List<LogProperty>();

        logger.LogInformation("Initializing the Semantic Kernel service...");

        var builder = Kernel.CreateBuilder();

        builder.Services.AddSingleton<ILoggerFactory>(loggerFactory);

        DefaultAzureCredential credential;
        if (string.IsNullOrEmpty(settings.AzureOpenAISettings.UserAssignedIdentityClientID))
        {
            credential = new DefaultAzureCredential();
        
        else
        
            credential = new DefaultAzureCredential(new DefaultAzureCredentialOptions
            {
                ManagedIdentityClientId = settings.AzureOpenAISettings.UserAssignedIdentityClientID
            );
        }
        builder.AddAzureOpenAIChatCompletion(
            settings.AzureOpenAISettings.CompletionsDeployment,
            settings.AzureOpenAISettings.Endpoint,
            credential);

        builder.AddAzureOpenAITextEmbeddingGeneration(
               settings.AzureOpenAISettings.EmbeddingsDeployment,
               settings.AzureOpenAISettings.Endpoint,
               credential);

        semanticKernel = builder.Build();

        Task.Run(Initialize).ConfigureAwait(false);
    }

    public void Dispose()
    
        // Dispose resources if any
    

    private Task Initialize()
    
        try
        {
            serviceInitialized = true;
            logger.LogInformation("Semantic Kernel service initialized.");
        
        catch (Exception ex)
        
            logger.LogError(ex, "Semantic Kernel service was not initialized. The following error occurred: {ErrorMessage.", ex.ToString());
        }
        return Task.CompletedTask;
    }

    private void LogMessage(string key, string value)
    
        promptDebugProperties.Add(new LogProperty(key, value));
    

    public async Task<Tuple<List<Message>, List<DebugLog>>> GetResponse(Message userMessage, List<Message> messageHistory, string tenantId, string userId)
    

        try
        {
            ChatCompletionAgent agent = new ChatCompletionAgent
            {
                Name = "BasicAgent",
                Instructions = "Greet the user and translate the request into French",
                Kernel = semanticKernel.Clone()
            ;

            ChatHistory chatHistory = [];

            chatHistory.AddUserMessage(userMessage.Text);

            promptDebugProperties = new List<LogProperty>();

            List<Message> completionMessages = new();
            List<DebugLog> completionMessagesLogs = new();

            ChatMessageContent message = new(AuthorRole.User, userMessage.Text);
            chatHistory.Add(message);

            await foreach (ChatMessageContent response in agent.InvokeAsync(chatHistory))
            
                string messageId = Guid.NewGuid().ToString();
                completionMessages.Add(new Message(userMessage.TenantId, userMessage.UserId, userMessage.SessionId, response.AuthorName ?? string.Empty, response.Role.ToString(), response.Content ?? string.Empty, messageId));
            
            return new Tuple<List<Message>, List<DebugLog>>(completionMessages, completionMessagesLogs);

        }
        catch (Exception ex)
        
            logger.LogError(ex, "Error when getting response: {ErrorMessage", ex.ToString());
            return new Tuple<List<Message>, List<DebugLog>>(new List<Message>(), new List<DebugLog>());
        }
    }

    public async Task<string> Summarize(string sessionId, string userPrompt)
    {
        try
        {
            // Use an AI function to summarize the text in 2 words
            var summarizeFunction = semanticKernel.CreateFunctionFromPrompt(
                "Summarize the following text into exactly two words:\n\n{{input}}",
                executionSettings: new OpenAIPromptExecutionSettings { MaxTokens = 10 }
            );

            // Invoke the function
            var summary = await semanticKernel.InvokeAsync(summarizeFunction, new() { ["input"] = userPrompt });

            return summary.GetValue<string>() ?? "No summary generated";
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Error when getting response: {ErrorMessage}", ex.ToString());
            return string.Empty;
        }
    }

    public async Task<float[]> GenerateEmbedding(string text)
    {
        // Generate Embedding
        var embeddingModel = semanticKernel.Services.GetRequiredService<ITextEmbeddingGenerationService>();

        var embedding = await embeddingModel.GenerateEmbeddingAsync(text);

        // Convert ReadOnlyMemory<float> to IList<float>
        return embedding.ToArray();
    }
}
```
</details>

<details>
  <summary>Completed code for <strong>ChatInfrastructure/Services/DependencyInjection.cs</strong></summary>
<br>

```csharp:
using MultiAgentCopilot.Common.Models.Configuration;
using MultiAgentCopilot.ChatInfrastructure.Interfaces;
using MultiAgentCopilot.ChatInfrastructure.Services;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.ApplicationInsights.AspNetCore.Extensions;
using Microsoft.ApplicationInsights.Extensibility;
using Azure.Identity;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging.ApplicationInsights;
namespace MultiAgentCopilot
{
    /// <summary>
    /// General purpose dependency injection extensions.
    /// </summary>
    public static partial class DependencyInjection
    {
        /// <summary>
        /// Registers the <see cref="ICosmosDBService"/> implementation with the dependency injection container.
        /// </summary>
        /// <param name="builder">The hosted applications and services builder.</param>
        public static void AddCosmosDBService(this IHostApplicationBuilder builder)
        {
            builder.Services.AddOptions<CosmosDBSettings>()
                .Bind(builder.Configuration.GetSection("CosmosDBSettings"));

            Console.WriteLine("Adding CosmosDBService:" + builder.Configuration["CosmosDBSettings:CosmosUri"]);
            builder.Services.AddSingleton<ICosmosDBService, CosmosDBService>();
        }


        /// <summary>
        /// Registers the <see cref="IChatService"/> implementation with the dependency injection container.
        /// </summary>
        /// <param name="builder">The hosted applications and services builder.</param>
        public static void AddChatService(this IHostApplicationBuilder builder)
        {
            builder.Services.AddOptions<CosmosDBSettings>()
                .Bind(builder.Configuration.GetSection("CosmosDBSettings"));
            builder.Services.AddSingleton<IChatService, ChatService>();
        }

        /// <summary>
        /// Registers the <see cref="ISemanticKernelService"/> implementation with the dependency injection container.
        /// </summary>
        /// <param name="builder">The hosted applications and services builder.</param>
        public static void AddSemanticKernelService(this IHostApplicationBuilder builder)
        {
            builder.Services.AddOptions<SemanticKernelServiceSettings>()
                .Bind(builder.Configuration.GetSection("SemanticKernelServiceSettings"));
            builder.Services.AddSingleton<ISemanticKernelService, SemanticKernelService>();
        }
    }
}
```
</details>

<details>
  <summary>Completed code for <strong>ChatInfrastructure/Services/ChatService.cs</strong></summary>
<br>

```csharp:
using MultiAgentCopilot.Common.Models.Chat;
using MultiAgentCopilot.ChatInfrastructure.Interfaces;
using Microsoft.Extensions.Logging;
using Microsoft.SemanticKernel.ChatCompletion;
using MultiAgentCopilot.Common.Models.Debug;
using System.Collections.Generic;
using Microsoft.Extensions.Options;
using MultiAgentCopilot.Common.Models.Configuration;
using Newtonsoft.Json.Linq;
using System.Text.Json;
using Newtonsoft.Json;
using Microsoft.Identity.Client;
using BankingServices.Interfaces;


namespace MultiAgentCopilot.ChatInfrastructure.Services;

public class ChatService : IChatService
{
    private readonly ICosmosDBService cosmosDBService;
    private readonly ILogger logger;
    private readonly IBankDataService bankService;
    private readonly ISemanticKernelService skService;

    public ChatService(
        IOptions<CosmosDBSettings> cosmosOptions,
        IOptions<SemanticKernelServiceSettings> skOptions,
        ICosmosDBService cosmosDBService,
        ISemanticKernelService ragService,
        ILoggerFactory loggerFactory)
    {
        cosmosDBService = cosmosDBService;
        skService = ragService;
        logger = loggerFactory.CreateLogger<ChatService>();
    }

    /// <summary>
    /// Returns list of chat session ids and names.
    /// </summary>
    public async Task<List<Session>> GetAllChatSessionsAsync(string tenantId, string userId)
    {
        return await cosmosDBService.GetUserSessionsAsync(tenantId, userId);
    }

    /// <summary>
    /// Returns the chat messages related to an existing session.
    /// </summary>
    public async Task<List<Message>> GetChatSessionMessagesAsync(string tenantId, string userId,string sessionId)
    {
        ArgumentNullException.ThrowIfNull(sessionId);
        return await cosmosDBService.GetSessionMessagesAsync(tenantId,userId,sessionId);
    }

    /// <summary>
    /// Creates a new chat session.
    /// </summary>
    public async Task<Session> CreateNewChatSessionAsync(string tenantId, string userId)
    {
        Session session = new(tenantId, userId);
        return await cosmosDBService.InsertSessionAsync(session);
    }

    /// <summary>
    /// Rename the chat session from its default (eg., "New Chat") to the summary provided by OpenAI.
    /// </summary>
    public async Task<Session> RenameChatSessionAsync(string tenantId, string userId,string sessionId, string newChatSessionName)
    {
        ArgumentNullException.ThrowIfNull(sessionId);
        ArgumentException.ThrowIfNullOrEmpty(newChatSessionName);

        return await cosmosDBService.UpdateSessionNameAsync( tenantId, userId,sessionId, newChatSessionName);
    }

    /// <summary>
    /// Delete a chat session and related messages.
    /// </summary>
    public async Task DeleteChatSessionAsync(string tenantId, string userId,string sessionId)
    {
        ArgumentNullException.ThrowIfNull(sessionId);
        await cosmosDBService.DeleteSessionAndMessagesAsync(tenantId, userId,sessionId);
    }

    /// <summary>
    /// Receive a prompt from a user, vectorize it from the OpenAI service, and get a completion from the OpenAI service.
    /// </summary>
    public async Task<List<Message>> GetChatCompletionAsync(string tenantId, string userId, string? sessionId, string userPrompt)
    {
        try
        {
            ArgumentNullException.ThrowIfNull(sessionId);

            // Add both prompt and completion to cache, then persist in Cosmos DB
            var userMessage = new Message(tenantId, userId, sessionId, "User", "User", userPrompt);

            // Generate the completion to return to the user
            var result = await skService.GetResponse(userMessage, new List<Message>(), tenantId, userId);

            return result.Item1;
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Error getting completion in session sessionId for user prompt [userPrompt].");
            return new List<Message> { new Message(tenantId, userId, sessionId!, "Error", "Error", "Error getting completion in session {sessionId} for user prompt [{userPrompt}].") };
        }
    }

    /// <summary>
    /// Generate a name for a chat message, based on the passed in prompt.
    /// </summary>
    public async Task<string> SummarizeChatSessionNameAsync(string tenantId, string userId, string? sessionId, string prompt)
    {
        try
        {
            ArgumentNullException.ThrowIfNull(sessionId);

            var summary = await skService.Summarize(sessionId, prompt);

            var session = await RenameChatSessionAsync(tenantId, userId, sessionId, summary);

            return session.Name;
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Error getting a summary in session sessionId for user prompt [prompt].");
            return "Error getting a summary in session {sessionId} for user prompt [{prompt}].";
        }
    }

    /// <summary>
    /// Rate an assistant message. This can be used to discover useful AI responses for training, discoverability, and other benefits down the road.
    /// </summary>
    public async Task<Message> RateChatCompletionAsync(string tenantId, string userId,string messageId, string sessionId, bool? rating)
    {
        ArgumentNullException.ThrowIfNull(messageId);
        ArgumentNullException.ThrowIfNull(sessionId);

        return await cosmosDBService.UpdateMessageRatingAsync(tenantId,userId, sessionId, messageId,rating);
    }

    public async Task<DebugLog> GetChatCompletionDebugLogAsync(string tenantId, string userId,string sessionId, string debugLogId)
    {
        ArgumentException.ThrowIfNullOrEmpty(sessionId);
        ArgumentException.ThrowIfNullOrEmpty(debugLogId);

        return await cosmosDBService.GetChatCompletionDebugLogAsync(tenantId,userId, sessionId, debugLogId);
    }

    public async Task<bool> AddDocument(string containerName, JsonElement document)
    {
        try
        {
            // Extract raw JSON from JsonElement
            var json = document.GetRawText();
            var docJObject = JsonConvert.DeserializeObject<JObject>(json);

            // Ensure "id" exists
            if (!docJObject.ContainsKey("id"))
            {
                throw new ArgumentException("Document must contain an 'id' property.");
            }
           
            return await cosmosDBService.InsertDocumentAsync(containerName, docJObject);

        }
        catch (Exception ex)
        {
            _logger.LogError(ex, $"Error adding document to container {containerName}.");
            return false;
        }
    }
}
```
</details>

<details>
  <summary>Completed code for <strong>ChatAPI/Program.cs</strong></summary>
<br>

```csharp:
using MultiAgentCopilot;
using Microsoft.Extensions.Configuration;

namespace ChatAPI
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var builder = WebApplication.CreateBuilder(args);

            // Load configuration from appsettings.json and environment variables
            builder.Configuration.SetBasePath(Directory.GetCurrentDirectory())
                                 .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
                                 .AddJsonFile("appsettings.development.json", optional: true, reloadOnChange: true)
                                 .AddEnvironmentVariables();

            // Add CORS policy
            builder.Services.AddCors(options =>
            {
                options.AddPolicy("AllowAllOrigins",
                    policy =>
                    {
                        policy.AllowAnyOrigin()
                              .AllowAnyMethod()
                              .AllowAnyHeader();
                    });
            });


            builder.Logging.ClearProviders();
            builder.Logging.AddConsole();

            if (!builder.Environment.IsDevelopment())                
                builder.Services.AddApplicationInsightsTelemetry();

            builder.Logging.SetMinimumLevel(LogLevel.Trace);
            builder.Services.Configure<LoggerFilterOptions>(options =>
            {
                options.MinLevel = LogLevel.Trace;
            });

            //builder.AddApplicationInsightsTelemetry();

            builder.AddCosmosDBService();
            builder.AddSemanticKernelService();

            builder.AddChatService();
            builder.Services.AddScoped<ChatEndpoints>();

            // Add services to the container.
            builder.Services.AddAuthorization();

            // Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
            builder.Services.AddEndpointsApiExplorer();
            builder.Services.AddSwaggerGen();

            var app = builder.Build();
            app.UseCors("AllowAllOrigins");

            app.UseExceptionHandler(exceptionHandlerApp
                    => exceptionHandlerApp.Run(async context
                        => await Results.Problem().ExecuteAsync(context)));

            // Configure the HTTP request pipeline.
            app.UseSwagger();
            app.UseSwaggerUI();

            app.UseAuthorization();

            // Map the chat REST endpoints:
            using (var scope = app.Services.CreateScope())
            {
                var service = scope.ServiceProvider.GetService<ChatEndpoints>();
                service?.Map(app);
            }

            app.Run();
        }
    }
}
```

===

</details>

### Next Steps

Proceed to Connecting Agents to Memory

Resources

• Semantic Kernel Agent Framework:https://learn.microsoft.com/semantic-kernel/frameworks/agent
• Azure OpenAI Service documentation:https://learn.microsoft.com/azure/cognitive-services/openai/
• Azure Cosmos DB Vector Database:https://learn.microsoft.com/azure/cosmos-db/vector-database

###Module 02 - Connecting Agents to Memory


Introduction

In this Module you'll connect your agent to Azure Cosmos DB to provide memory for chat history and state management for your agents to provide durability and context-awareness in your agent interactions.

Learning Objectives and Activities

• Learn the basics for Azure Cosmos DB for storing state and chat history
• Learn how to integrate agent frameworks to Azure Cosmos DB
• Test connectivity to Azure Cosmos DB works

Module Exercises

 * Activity 1: Session Memory Persistence in Agent Frameworks:activity-1-session-memory-persistence-in-agent-frameworks
 
 * Activity 2: Create a Simple Banking Agent:activity-2-create-a-simple-banking-agent
 
 * Activity 3: Connecting Agent Frameworks to Azure Cosmos DB:activity-3-connecting-agent-frameworks-to-azure-cosmos-db
 
 * Activity 4: Test your Work:activity-4-test-your-work

Activity 1: Session Memory Persistence in Agent Frameworks

In this session you will get an overview of memory and how it works for Semantic Kernel Agents and LangGraph and learn the basics for how to configure and connect both to Azure Cosmos DB as a memory store for both chat history and/or state management.

===

### Activity 2: Create a Simple Banking Agent

In this hands-on exercise, we will evolve the agent that translated responses into French into an agent that is intended for the banking scenario we are building here. We will also take the first step to learning about prompts here as well.

First, navigate to the ChatInfrastructure project in your IDE

Create Factories folder inside this project. Your project should look like this.

New Factories folder（./media/module-02/solution-factories-folder.png）

Add System Prompt Factory

System prompts provide instructions to the LLM, shaping its responses accordingly. This next step enables the creation of system prompts for various scenarios. We are going to implement a very simple agent to demonstrate how these work.

In your IDE, within the /Factories folder, create a new class, SystemPromptFactory.cs.

Then replace the code with this below:

```csharp:
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.SemanticKernel;
using MultiAgentCopilot.ChatInfrastructure.Services;
using MultiAgentCopilot.ChatInfrastructure.Models;

namespace MultiAgentCopilot.ChatInfrastructure.Factories
{
    internal static class SystemPromptFactory
    {
        public static string GetAgentName()
        {
            string name = "FrontDeskAgent";
            return name;
        }

        public static string GetAgentPrompts()
        {
            string prompt = "You are a front desk agent in a bank. Respond to the user queries professionally. Provide professional and helpful responses to user queries.Use your knowledge of banking services and procedures to address user queries accurately.";
            return prompt;
        }
    }
}
```
Add Chat Factory

Agents are autonomous systems that use LLMs to process inputs, make decisions, and generate responses based on predefined goals. They can integrate with external tools, retrieve information, and adapt dynamically to different tasks. We are next going to implement a ChatFactory class that enables the creation of agents for various scenarios. It uses the SystemPromptFactory to define the agent prompts.

In your IDE, within the /Factories folder, create a new class, ChatFactory.cs.

Then replace the code with this below:

```csharp:
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.Agents;
using Microsoft.SemanticKernel.Agents.Chat;
using Microsoft.SemanticKernel.ChatCompletion;
using Microsoft.SemanticKernel.Connectors.OpenAI;
using Microsoft.SemanticKernel.Connectors.AzureOpenAI;
using MultiAgentCopilot.ChatInfrastructure.Helper;
using MultiAgentCopilot.ChatInfrastructure.Models;
using BankingServices.Interfaces;
using OpenAI.Chat;
using System.Text.Json;
using Microsoft.Extensions.Logging;
using System.Reflection.Metadata;

namespace MultiAgentCopilot.ChatInfrastructure.Factories
{
    internal class ChatFactory
    {
        public delegate void LogCallback(string key, string value);

        public ChatCompletionAgent BuildAgent(Kernel kernel, ILoggerFactory loggerFactory, string tenantId, string userId)
        {
            ChatCompletionAgent agent = new ChatCompletionAgent
            {
                Name = SystemPromptFactory.GetAgentName(),
                Instructions = """SystemPromptFactory.GetAgentPrompts()""",
                Kernel = kernel.Clone()
            };

            return agent;
        }       
    }
}
```
We now have an agent that executes instructions provided as prompts.

Next lets add some memory so that the agent can remember the previous messages and doesn't loose context of the chat session.

===

### Activity 3: Connecting Agent Frameworks to Azure Cosmos DB

In this activity, you will learn how to initialize Azure Cosmos DB and integrate with an agent framework to provide persistent memory for chat history and state management.

In your IDE navigate to the ChatInfrastructure project.

Then navigate to the /Services folder.

Open SemanticKernelService.cs

At the top of the file with the other Using statements, add the following code:

```csharp:
using MultiAgentCopilot.ChatInfrastructure.Factories;
```
Update GetResponse() function in SemanticKernelService

The GetResponse() function is the main entry point for our multi-agent application. Within that function, a variable named, messageHistory stores a list of historical messages from the chat session. The chatHistory object is used to construct this history and passed to the Semantic Kernel Chat Completion Agent object. The completionMessages list is used to store the response received from the agent which then needs to be persisted in Cosmos DB for the next iteration of the agent.

We're going to modify this function to provide that persistence with Cosmos DB.

In your IDE, open the SemanticKernelService.cs and navigate to the GetResponse() function.

Replace all of the code within the Try block with the code below:

```csharp:
public async Task<Tuple<List<Message>, List<DebugLog>>> GetResponse(Message userMessage, List<Message> messageHistory,  string tenantId, string userId)
    
        try
        {
            //Replace from here
            ChatFactory agentChatGeneratorService = new ChatFactory();

            var agent = agentChatGeneratorService.BuildAgent(semanticKernel, loggerFactory,  tenantId, userId);

            ChatHistory chatHistory = [];

            // Load history
            foreach (var chatMessage in messageHistory)
            {                
                if(chatMessage.SenderRole == "User")
                {
                    chatHistory.AddUserMessage(chatMessage.Text);
                
                else
                
                    chatHistory.AddAssistantMessage(chatMessage.Text);
                
            }

            chatHistory.AddUserMessage(userMessage.Text);

            promptDebugProperties = new List<LogProperty>();

            List<Message> completionMessages = new();
            List<DebugLog> completionMessagesLogs = new();


            await foreach (ChatMessageContent response in agent.InvokeAsync(chatHistory))
            
                string messageId = Guid.NewGuid().ToString();
                completionMessages.Add(new Message(
                    userMessage.TenantId, 
                    userMessage.UserId, 
                    userMessage.SessionId, 
                    response.AuthorName ?? string.Empty, 
                    response.Role.ToString(), 
                    response.Content ?? string.Empty, 
                    messageId));
                        
            return new Tuple<List<Message>, List<DebugLog>>(completionMessages, completionMessagesLogs);
            //end replace
        }
        catch (Exception ex)
        
            logger.LogError(ex, "Error when getting response: {ErrorMessage", ex.ToString());
            return new Tuple<List<Message>, List<DebugLog>>(new List<Message>(), new List<DebugLog>());
        }
    }
```
Store user messages and responses in CosmosDB

The next few steps will walk you through how to persist our agent interactions in Cosmos DB. Azure Cosmos DB is used extensively for capturing user and agent interactions in these types of applications. It is a great choice for this scenario due to its schema-agnostic design as a NoSQL database that can scale out to support any number of users instantly.

To accomplish this task we will do the following tasks:

 Update the interface for Cosmos DB service to upsert new data
 Implement the new upsert feature
 Extend the upsert feature to our Chat Service

Update Cosmos DB Interface

To begin, navigate to the /Interfaces folder in the ChatInfrastructure project.

Open the ICosmosDBService.cs interface.

Within ICosmosDBService paste this block of code below:

```csharp:
/// <summary>
    /// Batch create or update chat messages and session.
    /// </summary>
    /// <param name="messages">Chat message and session items to create or replace.</param>
    Task UpsertSessionBatchAsync(List<Message> messages, List<DebugLog> debugLogs, Session session);
```
Update Cosmos DB Service

Next let's implement this interface in our service layer.

We are implementing this functionality using the transactional batch feature for Cosmos DB. This allows users to insert, update or delete multiple documents at the same time in the context of a transaction. In our scenario here, we are inserting or updating multiple chat history documents simultaneously, and optionally debug logs as well. Within the agent chat session, the user message and each response (there can be multiple) is stored as a separate document in Cosmos DB. These all share the same Cosmos DB partition key which means they can be committed atomically within a transaction.

To begin, navigate to the /Services folder in the ChatInfrastructure project.

Open the CosmosDBService.cs class.

Scroll to the end of the file. Before the second to the last curly brace copy and paste the code below:

```csharp:
public async Task UpsertSessionBatchAsync(List<Message> messages, List<DebugLog>debugLogs, Session session)
        
            try
            { 
                if (messages.Select(m => m.SessionId).Distinct().Count() > 1 || session.SessionId != messages.Select(m => m.SessionId).FirstOrDefault())
                {
                    throw new ArgumentException("All items must have the same partition key.");
                

                if (debugLogs.Count>0 && (debugLogs.Select(m => m.SessionId).Distinct().Count() > 1 || session.SessionId != debugLogs.Select(m => m.SessionId).FirstOrDefault()))
                
                    throw new ArgumentException("All items must have the same partition key as message.");
                

                PartitionKey partitionKey = PartitionManager.GetChatDataFullPK(session.TenantId, session.UserId, session.SessionId);
                var batch = chatData.CreateTransactionalBatch(partitionKey);
                foreach (var message in messages)
                
                    batch.UpsertItem(
                        item: message
                    );
                

                foreach (var log in debugLogs)
                
                    batch.UpsertItem(
                        item: log
                    );
                

                batch.UpsertItem(
                    item: session
                );

                await batch.ExecuteAsync();
            }
            catch (CosmosException ex)
            
                logger.LogError(ex.ToString());
                throw;
            
        }
```
Update Chat Service

With the implementation complete in the Cosmos DB service, we can now update our Chat Service to store the messages generated between users and agents. In this step, we will add a new function that first calls the Cosmos DB service to get a Session object from our database. The Session object is part of an object hierarchy that defines the conversations between users and agents. A session has a name and also an array of messages for that conversation topic. You can view this hierarchy by navigating to its definition stored in Common/Models/Chat/Session.cs

With a reference to the current session returned from the CosmosDBService, this function then calls our newly implemented function to update the messages within the session object with any new or updated messages. Typically, this would include a single user prompt, followed by one or more responses from the agents.

To begin, navigate to the /Services folder in the ChatInfrastructure project.

Open the ChatService.cs class.

Scroll to the end of the file. Before the the last curly brace copy and paste the code below:

```csharp:
/// <summary>
    /// Add user prompt and AI assistance response to the chat session message list object and insert into the data service as a transaction.
    /// </summary>
    private async Task AddPromptCompletionMessagesAsync(string tenantId, string userId,string sessionId, Message promptMessage, List<Message> completionMessages, List<DebugLog> completionMessageLogs)
    
        var session = await cosmosDBService.GetSessionAsync(tenantId, userId,sessionId);

        completionMessages.Insert(0, promptMessage);
            await cosmosDBService.UpsertSessionBatchAsync(completionMessages, completionMessageLogs, session);
```
We also need to implement a method to Get the conversation for the current session. To do that we will update an existing method, GetChatCompletionAsync() to include a call to the Cosmos DB service, GetSessionMessagesAsync() that has already been implemented.

To begin, within the same ChatServices.cs file, locate GetChatCompletionAsync()

Then update the function with the code in the Try block below:

```csharp:
public async Task<List<Message>> GetChatCompletionAsync(string tenantId, string userId,string? sessionId, string userPrompt)
    
        try
        {
            //Replace from here
            ArgumentNullException.ThrowIfNull(sessionId);

            // Retrieve conversation, including latest prompt.
            var archivedMessages = await cosmosDBService.GetSessionMessagesAsync(tenantId, userId, sessionId);

            // Add both prompt and completion to cache, then persist in Cosmos DB
            var userMessage = new Message(tenantId,userId,sessionId, "User","User", userPrompt);

            // Generate the completion to return to the user
            var result = await skService.GetResponse(userMessage, archivedMessages,tenantId,userId);

            await AddPromptCompletionMessagesAsync(tenantId, userId,sessionId, userMessage, result.Item1, result.Item2);

            return result.Item1;
            //end replace
        
        catch (Exception ex)
        {
            logger.LogError(ex, "Error getting completion in session {sessionId} for user prompt [{userPrompt}].");
            return new List<Message> { new Message(tenantId, userId, sessionId!, 
                "Error", "Error", "Error getting completion in session sessionId for user prompt [userPrompt].") };
        }
    }
```

===

### Activity 4: Test your Work

With the activities in this module complete, it is time to test your work.

Running the ChatAPI and Frontend App

 Start the ChatAPI

• Codespaces open a terminal and type dotnet run
• In your IDE press F5 or select Run to start the application.

2. Run the Frontend App

• Open a new terminal or use an existing one that is open to the /frontend folder.

```sh:
ng serve
```
• Navigate to, <http://localhost:4200> in your browser

3. Start a Chat Session

We have a new agent now that thinks it works for a bank. So in our test here we are going to ask it banking-related questions.

 Open the frontend app.
 Start a new conversation.
 Send the following message:

```text:
Can a senior citizen open a savings account?
```
 Wait for the Agent response.
 Send another message:

```text:
Does the interest rate vary?
```
 Expected response: The Agent's response is contextually correct for the  whole chat session.

 You should see something like the output below.

    Test output Module 2（./media/module-02/test-output.png）

4. Stop the Application

• In the frontend terminal, press Ctrl + C to stop the application.
• In your IDE press Shift + F5 or stop the debugger.

Validation Checklist

Your implementation is successful if:

• [ ] Your app compiles with no warnings or errors.
• [ ] Your agent successfully responds with contextually correct information.

Common Issues and Troubleshooting

 Issue 1:
    • TBD
    • TBD

 Issue 2:
    • TBD
    • TBD

 Issue 3:
    • TBD
    • TBD

Module Solution

The following sections include the completed code for this Module. Copy and paste these into your project if you run into issues and cannot resolve.

<details>
  <summary>Completed code for <strong>ChatInfrastructure/Factories/SystemPromptFactory.cs</strong></summary>

<br>

```csharp:
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.SemanticKernel;
using MultiAgentCopilot.ChatInfrastructure.Services;
using MultiAgentCopilot.ChatInfrastructure.Models;

namespace MultiAgentCopilot.ChatInfrastructure.Factories

    internal static class SystemPromptFactory
    {
        public static string GetAgentName()
        {
            string name = "FrontDeskAgent";
            return name;
        

        public static string GetAgentPrompts()
        
            string prompt = "You are a front desk agent in a bank. Respond to the user queries professionally. Provide professional and helpful responses to user queries.Use your knowledge of banking services and procedures to address user queries accurately.";
            return prompt;
        
    }
}
```
</details>

<details>
  <summary>Completed code for <strong>ChatInfrastructure/Factories/ChatFactory.cs</strong></summary>

<br>
```
csharp:
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.Agents;
using Microsoft.SemanticKernel.Agents.Chat;
using Microsoft.SemanticKernel.ChatCompletion;
using Microsoft.SemanticKernel.Connectors.OpenAI;
using Microsoft.SemanticKernel.Connectors.AzureOpenAI;
using MultiAgentCopilot.ChatInfrastructure.Helper;
using MultiAgentCopilot.ChatInfrastructure.Models;
using BankingServices.Interfaces;
using OpenAI.Chat;
using System.Text.Json;
using Microsoft.Extensions.Logging;
using System.Reflection.Metadata;

namespace MultiAgentCopilot.ChatInfrastructure.Factories
{
    internal class ChatFactory
    {
        public delegate void LogCallback(string key, string value);

        public ChatCompletionAgent BuildAgent(Kernel kernel, ILoggerFactory loggerFactory, string tenantId, string userId)
        {
            ChatCompletionAgent agent = new ChatCompletionAgent
            {
                Name = SystemPromptFactory.GetAgentName(),
                Instructions = """{SystemPromptFactory.GetAgentPrompts()}""",
                Kernel = kernel.Clone()
            };

            return agent;
        }
    }
}
```
</details>

<details>
  <summary>Completed code for <strong>ChatInfrastructure/Services/SemanticKernelService.cs</strong></summary>

<br>

```csharp:
using System;
using System.Runtime;
using System.Data;
using Newtonsoft.Json;
using Azure.Identity;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Options;

using Microsoft.SemanticKernel;
using Microsoft.SemanticKernel.Agents;
using Microsoft.SemanticKernel.Connectors.OpenAI;
using Microsoft.SemanticKernel.ChatCompletion;
using Microsoft.SemanticKernel.Embeddings;

using MultiAgentCopilot.Common.Models.Chat;
using MultiAgentCopilot.ChatInfrastructure.Interfaces;
using MultiAgentCopilot.Common.Models.Configuration;
using MultiAgentCopilot.Common.Models.Debug;
using Message = MultiAgentCopilot.Common.Models.Chat.Message;
using MultiAgentCopilot.ChatInfrastructure.Factories;

namespace MultiAgentCopilot.ChatInfrastructure.Services;

public class SemanticKernelService : ISemanticKernelService, IDisposable
{
    readonly SemanticKernelServiceSettings settings;
    readonly ILoggerFactory loggerFactory;
    readonly ILogger<SemanticKernelService> logger;
    readonly Kernel semanticKernel;

    bool serviceInitialized = false;
    string prompt = string.Empty;
    string contextSelectorPrompt = string.Empty;

    List<LogProperty> promptDebugProperties;

    public bool IsInitialized => serviceInitialized;

    public SemanticKernelService(
        IOptions<SemanticKernelServiceSettings> options,
        ILoggerFactory loggerFactory)
    {
        settings = options.Value;
        loggerFactory = loggerFactory;
        logger = loggerFactory.CreateLogger<SemanticKernelService>();
        promptDebugProperties = new List<LogProperty>();

        logger.LogInformation("Initializing the Semantic Kernel service...");

        var builder = Kernel.CreateBuilder();

        builder.Services.AddSingleton<ILoggerFactory>(loggerFactory);

        DefaultAzureCredential credential;
        if (string.IsNullOrEmpty(settings.AzureOpenAISettings.UserAssignedIdentityClientID))
        {
            credential = new DefaultAzureCredential();
        }
        else
        {
            credential = new DefaultAzureCredential(new DefaultAzureCredentialOptions
            {
                ManagedIdentityClientId = settings.AzureOpenAISettings.UserAssignedIdentityClientID
            });
        }
        builder.AddAzureOpenAIChatCompletion(
            settings.AzureOpenAISettings.CompletionsDeployment,
            settings.AzureOpenAISettings.Endpoint,
            credential);

        builder.AddAzureOpenAITextEmbeddingGeneration(
               settings.AzureOpenAISettings.EmbeddingsDeployment,
               settings.AzureOpenAISettings.Endpoint,
               credential);

        semanticKernel = builder.Build();

        Task.Run(Initialize).ConfigureAwait(false);
    }

    public void Dispose()
    {
        // Dispose resources if any
    }

    private Task Initialize()
    {
        try
        {
            serviceInitialized = true;
            logger.LogInformation("Semantic Kernel service initialized.");
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Semantic Kernel service was not initialized. The following error occurred: {ErrorMessage}.", ex.ToString());
        }
        return Task.CompletedTask;
    }

    private void LogMessage(string key, string value)
    {
        promptDebugProperties.Add(new LogProperty(key, value));
    }

    public async Task<Tuple<List<Message>, List<DebugLog>>> GetResponse(Message userMessage, List<Message> messageHistory, string tenantId, string userId)
    {
        try
        {
            //Replace from here
            ChatFactory agentChatGeneratorService = new ChatFactory();

            var agent = agentChatGeneratorService.BuildAgent(semanticKernel, loggerFactory, tenantId, userId);

            ChatHistory chatHistory = [];

            // Load history
            foreach (var chatMessage in messageHistory)
            {
                if (chatMessage.SenderRole == "User")
                {
                    chatHistory.AddUserMessage(chatMessage.Text);
                }
                else
                {
                    chatHistory.AddAssistantMessage(chatMessage.Text);
                }
            }

            chatHistory.AddUserMessage(userMessage.Text);

            promptDebugProperties = new List<LogProperty>();

            List<Message> completionMessages = new();
            List<DebugLog> completionMessagesLogs = new();


            await foreach (ChatMessageContent response in agent.InvokeAsync(chatHistory))
            {
                string messageId = Guid.NewGuid().ToString();
                completionMessages.Add(new Message(
                    userMessage.TenantId,
                    userMessage.UserId,
                    userMessage.SessionId,
                    response.AuthorName ?? string.Empty,
                    response.Role.ToString(),
                    response.Content ?? string.Empty,
                    messageId));
            }
            return new Tuple<List<Message>, List<DebugLog>>(completionMessages, completionMessagesLogs);
            //end replace
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Error when getting response: {ErrorMessage}", ex.ToString());
            return new Tuple<List<Message>, List<DebugLog>>(new List<Message>(), new List<DebugLog>());
        }
    }


    public async Task<string> Summarize(string sessionId, string userPrompt)
    {
        try
        {
            // Use an AI function to summarize the text in 2 words
            var summarizeFunction = semanticKernel.CreateFunctionFromPrompt(
                "Summarize the following text into exactly two words:\n\n{{input}}",
                executionSettings: new OpenAIPromptExecutionSettings  MaxTokens = 10 
            );

            // Invoke the function
            var summary = await semanticKernel.InvokeAsync(summarizeFunction, new()  ["input"] = userPrompt );

            return summary.GetValue<string>() ?? "No summary generated";
        }
        catch (Exception ex)
        
            logger.LogError(ex, "Error when getting response: {ErrorMessage", ex.ToString());
            return string.Empty;
        }
    }

    public async Task<float[]> GenerateEmbedding(string text)
    
        // Generate Embedding
        var embeddingModel = semanticKernel.Services.GetRequiredService<ITextEmbeddingGenerationService>();

        var embedding = await embeddingModel.GenerateEmbeddingAsync(text);

        // Convert ReadOnlyMemory<float> to IList<float>
        return embedding.ToArray();
    
}
```
</details>

<details>
  <summary>Completed code for <strong>ChatInfrastructure/Interfaces/ICosmosDBService.cs</strong></summary>

<br>

```csharp:
using MultiAgentCopilot.Common.Models.Chat;
using MultiAgentCopilot.Common.Models.Debug;
using Newtonsoft.Json.Linq;
using System.Text.Json;

namespace MultiAgentCopilot.ChatInfrastructure.Interfaces;

public interface ICosmosDBService


    /// <summary>
    /// Gets a list of all current chat sessions.
    /// </summary>
    /// <returns>List of distinct chat session items.</returns>
    Task<List<Session>> GetUserSessionsAsync(string tenantId, string userId);

    /// <summary>
    /// Gets a list of all current chat messages for a specified session identifier.
    /// </summary>
    /// <param name="sessionId">Chat session identifier used to filter messsages.</param>
    /// <returns>List of chat message items for the specified session.</returns>
    Task<List<Message>> GetSessionMessagesAsync(string tenantId, string userId, string sessionId);

    /// <summary>
    /// Performs a point read to retrieve a single chat session item.
    /// </summary>
    /// <returns>The chat session item.</returns>
    Task<Session> GetSessionAsync(string tenantId, string userId,string sessionId);

    /// <summary>
    /// Creates a new chat session.
    /// </summary>
    /// <param name="session">Chat session item to create.</param>
    /// <returns>Newly created chat session item.</returns>
    Task<Session> InsertSessionAsync(Session session);

    /// <summary>
    /// Creates a new chat message.
    /// </summary>
    /// <param name="message">Chat message item to create.</param>
    /// <returns>Newly created chat message item.</returns>
    Task<Message> InsertMessageAsync(Message message);

    /// <summary>
    /// Updates an existing chat message.
    /// </summary>
    /// <param name="message">Chat message item to update.</param>
    /// <returns>Revised chat message item.</returns>
    Task<Message> UpdateMessageAsync(Message message);

    /// <summary>
    /// Updates a message's rating through a patch operation.
    /// </summary>
    /// <param name="id">The message id.</param>
    /// <param name="sessionId">The message's partition key (session id).</param>
    /// <param name="rating">The rating to replace.</param>
    /// <returns>Revised chat message item.</returns>
    Task<Message> UpdateMessageRatingAsync(string tenantId, string userId, string sessionId, string messageId, bool? rating);

    /// <summary>
    /// Updates an existing chat session.
    /// </summary>
    /// <param name="session">Chat session item to update.</param>
    /// <returns>Revised created chat session item.</returns>
    Task<Session> UpdateSessionAsync(Session session);

    /// <summary>
    /// Updates a session's name through a patch operation.
    /// </summary>
    /// <param name="id">The session id.</param>
    /// <param name="name">The session's new name.</param>
    /// <returns>Revised chat session item.</returns>
    Task<Session> UpdateSessionNameAsync(string tenantId, string userId,string id, string name);

    /// <summary>
    /// Batch deletes an existing chat session and all related messages.
    /// </summary>
    /// <param name="sessionId">Chat session identifier used to flag messages and sessions for deletion.</param>
    Task DeleteSessionAndMessagesAsync(string tenantId, string userId,string sessionId);

   
    Task<DebugLog> GetChatCompletionDebugLogAsync(string tenantId, string userId,string sessionId, string debugLogId);

    Task<bool> InsertDocumentAsync(string containerName, JObject document);

    /// <summary>
    /// Batch create or update chat messages and session.
    /// </summary>
    /// <param name="messages">Chat message and session items to create or replace.</param>
    Task UpsertSessionBatchAsync(List<Message> messages, List<DebugLog> debugLogs, Session session);
```
</details>

<details>
  <summary>Completed code for <strong>ChatInfrastructure/Services/CosmosDBService.cs</strong></summary>

<br>

```csharp:
using Azure.Identity;
using Microsoft.Azure.Cosmos;
using Microsoft.Azure.Cosmos.Fluent;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Options;
using MultiAgentCopilot.ChatInfrastructure.Interfaces;
using MultiAgentCopilot.Common.Helper;
using MultiAgentCopilot.Common.Models.Chat;
using MultiAgentCopilot.Common.Models.Configuration;
using MultiAgentCopilot.Common.Models.Debug;
using Newtonsoft.Json.Linq;
using System.Diagnostics;
using Container = Microsoft.Azure.Cosmos.Container;
using Message = MultiAgentCopilot.Common.Models.Chat.Message;
using PartitionKey = Microsoft.Azure.Cosmos.PartitionKey;

namespace MultiAgentCopilot.ChatInfrastructure.Services

    /// <summary>
    /// Service to access Azure Cosmos DB for NoSQL.
    /// </summary>
    public class CosmosDBService : ICosmosDBService
    {
        private readonly Container chatData;
        private readonly Container userData;
        private readonly Container offersData;
        private readonly Container accountsData;

        private readonly Database database;
        private readonly CosmosDBSettings settings;
        private readonly ILogger logger;


        public CosmosDBService(
            IOptions<CosmosDBSettings> settings,
            ILogger<CosmosDBService> logger)
        {
            settings = settings.Value;
            ArgumentException.ThrowIfNullOrEmpty(settings.CosmosUri);

            logger = logger;
            logger.LogInformation("Initializing Cosmos DB service.");

            if (!settings.EnableTracing)
            {
                Type defaultTrace = Type.GetType("Microsoft.Azure.Cosmos.Core.Trace.DefaultTrace,Microsoft.Azure.Cosmos.Direct");
                TraceSource traceSource = (TraceSource)defaultTrace.GetProperty("TraceSource").GetValue(null);
                traceSource.Switch.Level = SourceLevels.All;
                traceSource.Listeners.Clear();
            

            CosmosSerializationOptions options = new()
            
                PropertyNamingPolicy = CosmosPropertyNamingPolicy.CamelCase
            ;


            DefaultAzureCredential credential;
            if (string.IsNullOrEmpty(settings.UserAssignedIdentityClientID))
            
                credential = new DefaultAzureCredential();
            
            else
            
                credential = new DefaultAzureCredential(new DefaultAzureCredentialOptions
                {
                    ManagedIdentityClientId = settings.UserAssignedIdentityClientID
                );

            }
            CosmosClient client = new CosmosClientBuilder(settings.CosmosUri, credential)
                .WithSerializerOptions(options)
                .WithConnectionModeGateway()
                .Build();

            database = client?.GetDatabase(settings.Database) ??
                        throw new ArgumentException("Unable to connect to existing Azure Cosmos DB database.");

            chatData = database.GetContainer(settings.ChatDataContainer.Trim());
            userData = database.GetContainer(settings.UserDataContainer.Trim());
            offersData = database.GetContainer(settings.OfferDataContainer.Trim());
            accountsData = database.GetContainer(settings.AccountsContainer.Trim());

            logger.LogInformation("Cosmos DB service initialized.");
        }

        public async Task<List<Session>> GetUserSessionsAsync(string tenantId, string userId)
        
            try
            {
                QueryDefinition query = new QueryDefinition("SELECT DISTINCT  FROM c WHERE c.type = @type")
                    .WithParameter("@type", nameof(Session));

                var partitionKey= PartitionManager.GetChatDataPartialPK(tenantId, userId);
                FeedIterator<Session> response = chatData.GetItemQueryIterator<Session>(query, null, new QueryRequestOptions() { PartitionKey = partitionKey );

                List<Session> output = new();
                while (response.HasMoreResults)
                
                    FeedResponse<Session> results = await response.ReadNextAsync();
                    output.AddRange(results);
                

                return output;
            }
            catch (CosmosException ex)
            
                logger.LogError(ex.ToString());
                throw;
            
        }

        public async Task<Session> GetSessionAsync(string tenantId, string userId, string sessionId)
        
            try
            { 
                var partitionKey = PartitionManager.GetChatDataFullPK(tenantId, userId,sessionId);

                return await chatData.ReadItemAsync<Session>(
                    id: sessionId,
                    partitionKey: partitionKey);
            
            catch (CosmosException ex)
            
                logger.LogError(ex.ToString());
                throw;
            
        }

        public async Task<List<Message>> GetSessionMessagesAsync(string tenantId, string userId,string sessionId)
        
            try
            {
                QueryDefinition query =
                    new QueryDefinition("SELECT  FROM c WHERE c.type = @type")
                        .WithParameter("@type", nameof(Message));

                var partitionKey = PartitionManager.GetChatDataFullPK(tenantId, userId, sessionId);

                FeedIterator<Message> results = chatData.GetItemQueryIterator<Message>(query, null, new QueryRequestOptions() { PartitionKey = partitionKey );

                List<Message> output = new();
                while (results.HasMoreResults)
                
                    FeedResponse<Message> response = await results.ReadNextAsync();
                    output.AddRange(response);
                

                return output;
            }
            catch (CosmosException ex)
            
                logger.LogError(ex.ToString());
                throw;
            
        }

        public async Task<Session> InsertSessionAsync(Session session)
        
            try
            { 
                var partitionKey = PartitionManager.GetChatDataFullPK(session.TenantId, session.UserId,session.SessionId);

                var response= await chatData.CreateItemAsync(
                    item: session,
                    partitionKey: partitionKey
                );

                return response;
            
            catch (CosmosException ex)
            
                logger.LogError(ex.ToString());
                throw;
            
        }

        public async Task<Message> InsertMessageAsync(Message message)
        
            try
            { 
                var partitionKey = PartitionManager.GetChatDataFullPK(message.TenantId, message.UserId, message.SessionId);

                return await chatData.CreateItemAsync(
                    item: message,
                    partitionKey: partitionKey
                );
            
            catch (CosmosException ex)
            
                logger.LogError(ex.ToString());
                throw;
            
        }

        public async Task<Message> UpdateMessageAsync(Message message)
        
            try
            { 
                var partitionKey = PartitionManager.GetChatDataFullPK(message.TenantId, message.UserId, message.SessionId);

                return await chatData.ReplaceItemAsync(
                    item: message,
                    id: message.Id,
                    partitionKey: partitionKey
                );
            
            catch (CosmosException ex)
            
                logger.LogError(ex.ToString());
                throw;
            
        }

        public async Task<Message> UpdateMessageRatingAsync(string tenantId, string userId, string sessionId,string messageId, bool? rating)
        
            try
            { 
                var partitionKey = PartitionManager.GetChatDataFullPK(tenantId, userId, sessionId);

                var response = await chatData.PatchItemAsync<Message>(
                id: messageId,
                partitionKey: partitionKey,
                    patchOperations: new[]
                    {
                            PatchOperation.Set("/rating", rating),
                    
                );
                return response.Resource;
            }
            catch (CosmosException ex)
            
                logger.LogError(ex.ToString());
                throw;
            
        }

        public async Task<Session> UpdateSessionAsync(Session session)
        
            try
            { 
                var partitionKey = PartitionManager.GetChatDataFullPK(session.TenantId, session.UserId, session.SessionId);

                return await chatData.ReplaceItemAsync(
                    item: session,
                    id: session.Id,
                    partitionKey: partitionKey
                );
            
            catch (CosmosException ex)
            
                logger.LogError(ex.ToString());
                throw;
            
        }

        public async Task<Session> UpdateSessionNameAsync(string tenantId, string userId,string sessionId, string name)
        
            try
            {
                var partitionKey = PartitionManager.GetChatDataFullPK(tenantId, userId, sessionId);

                var response = await chatData.PatchItemAsync<Session>(
                    id: sessionId,
                    partitionKey: partitionKey,
                    patchOperations: new[]
                    {
                        PatchOperation.Set("/name", name),
                    
                );


                return response.Resource;
            }
            catch (CosmosException ex)
            
                logger.LogError(ex.ToString());
                throw;
            
}



        

        public async Task DeleteSessionAndMessagesAsync(string tenantId, string userId,string sessionId)
        
            try
            { 
                var partitionKey = PartitionManager.GetChatDataFullPK(tenantId, userId, sessionId);

                var query = new QueryDefinition("SELECT c.id FROM c WHERE c.sessionId = @sessionId")
                    .WithParameter("@sessionId", sessionId);

                var response = chatData.GetItemQueryIterator<Message>(query);

                var batch = chatData.CreateTransactionalBatch(partitionKey);
                while (response.HasMoreResults)
                {
                    var results = await response.ReadNextAsync();
                    foreach (var item in results)
                    {
                        batch.DeleteItem(
                            id: item.Id
                        );
                    
                }

                await batch.ExecuteAsync();
            }
            catch (CosmosException ex)
            
                logger.LogError(ex.ToString());
                throw;
            
        }

        public async Task<DebugLog> GetChatCompletionDebugLogAsync(string tenantId, string userId,string sessionId, string debugLogId)
        
            try
            { 
                var partitionKey = PartitionManager.GetChatDataFullPK(tenantId, userId, sessionId);

                return await chatData.ReadItemAsync<DebugLog>(
                    id: debugLogId,
                    partitionKey: partitionKey);
            
            catch (CosmosException ex)
            
                logger.LogError(ex.ToString());
                throw;
            
        }
        

        public async Task<bool> InsertDocumentAsync(string containerName, JObject document)
        
            Container container=null;

            switch (containerName)
            {
                case "OfferData":
                    container = offersData;
                    break;
                case "AccountData":
                    container = accountsData;
                    break;
                case "UserData":
                    container = userData;
                    break;
            
            try
            

                // Insert cleaned document
                await container.CreateItemAsync(document);


                return true;
            
            catch (CosmosException ex)
            {
                // Ignore conflict errors.
                if (ex.StatusCode == System.Net.HttpStatusCode.Conflict)
                {
                    logger.LogInformation("Duplicate document detected.");
                }
                else
                {
                    logger.LogError(ex, "Error inserting document.");
                    throw;
                }
                return false;
            }
        }

        public async Task UpsertSessionBatchAsync(List<Message> messages, List<DebugLog> debugLogs, Session session)
        {
            try
            {
                if (messages.Select(m => m.SessionId).Distinct().Count() > 1 || session.SessionId != messages.Select(m => m.SessionId).FirstOrDefault())
                {
                    throw new ArgumentException("All items must have the same partition key.");
                }

                if (debugLogs.Count > 0 && (debugLogs.Select(m => m.SessionId).Distinct().Count() > 1 || session.SessionId != debugLogs.Select(m => m.SessionId).FirstOrDefault()))
                {
                    throw new ArgumentException("All items must have the same partition key as message.");
                }

                PartitionKey partitionKey = PartitionManager.GetChatDataFullPK(session.TenantId, session.UserId, session.SessionId);
                var batch = chatData.CreateTransactionalBatch(partitionKey);
                foreach (var message in messages)
                {
                    batch.UpsertItem(
                        item: message
                    );
                }

                foreach (var log in debugLogs)
                {
                    batch.UpsertItem(
                        item: log
                    );
                }

                batch.UpsertItem(
                    item: session
                );

                await batch.ExecuteAsync();
            }
            catch (CosmosException ex)
            {
                logger.LogError(ex.ToString());
                throw;
            }
        }
    }
}
```
</details>

<details>
  <summary>Completed code for <strong>ChatInfrastructure/Services/ChatService.cs</strong></summary>

<br>

```csharp:
using MultiAgentCopilot.Common.Models.Chat;
using MultiAgentCopilot.ChatInfrastructure.Interfaces;
using Microsoft.Extensions.Logging;
using Microsoft.SemanticKernel.ChatCompletion;
using MultiAgentCopilot.Common.Models.Debug;
using System.Collections.Generic;
using Microsoft.Extensions.Options;
using MultiAgentCopilot.Common.Models.Configuration;
using Newtonsoft.Json.Linq;
using System.Text.Json;
using Newtonsoft.Json;
using Microsoft.Identity.Client;
using BankingServices.Interfaces;
using BankingServices.Services;


namespace MultiAgentCopilot.ChatInfrastructure.Services;

public class ChatService : IChatService
{
    private readonly ICosmosDBService cosmosDBService;
    private readonly ILogger logger;
    private readonly IBankDataService bankService;
    private readonly ISemanticKernelService skService;

    public ChatService(
        IOptions<CosmosDBSettings> cosmosOptions,
        IOptions<SemanticKernelServiceSettings> skOptions,
        ICosmosDBService cosmosDBService,
        ISemanticKernelService skService,
        ILoggerFactory loggerFactory)
    {
        cosmosDBService = cosmosDBService;
        skService = skService;
        bankService = new BankingDataService(cosmosOptions.Value, skOptions.Value, loggerFactory);
        logger = loggerFactory.CreateLogger<ChatService>();
    }

    /// <summary>
    /// Returns list of chat session ids and names.
    /// </summary>
    public async Task<List<Session>> GetAllChatSessionsAsync(string tenantId, string userId)
    {
        return await cosmosDBService.GetUserSessionsAsync(tenantId, userId);
    }

    /// <summary>
    /// Returns the chat messages related to an existing session.
    /// </summary>
    public async Task<List<Message>> GetChatSessionMessagesAsync(string tenantId, string userId,string sessionId)
    {
        ArgumentNullException.ThrowIfNull(sessionId);
        return await cosmosDBService.GetSessionMessagesAsync(tenantId,userId,sessionId);
    }

    /// <summary>
    /// Creates a new chat session.
    /// </summary>
    public async Task<Session> CreateNewChatSessionAsync(string tenantId, string userId)
    {
        Session session = new(tenantId, userId);
        return await cosmosDBService.InsertSessionAsync(session);
    }

    /// <summary>
    /// Rename the chat session from its default (eg., "New Chat") to the summary provided by OpenAI.
    /// </summary>
    public async Task<Session> RenameChatSessionAsync(string tenantId, string userId,string sessionId, string newChatSessionName)
    {
        ArgumentNullException.ThrowIfNull(sessionId);
        ArgumentException.ThrowIfNullOrEmpty(newChatSessionName);

        return await cosmosDBService.UpdateSessionNameAsync( tenantId, userId,sessionId, newChatSessionName);
    }

    /// <summary>
    /// Delete a chat session and related messages.
    /// </summary>
    public async Task DeleteChatSessionAsync(string tenantId, string userId,string sessionId)
    {
        ArgumentNullException.ThrowIfNull(sessionId);
        await cosmosDBService.DeleteSessionAndMessagesAsync(tenantId, userId,sessionId);
    }

    /// <summary>
    /// Receive a prompt from a user, vectorize it from the OpenAI service, and get a completion from the OpenAI service.
    /// </summary>
    public async Task<List<Message>> GetChatCompletionAsync(string tenantId, string userId, string? sessionId, string userPrompt)
    {
        try
        {
            //Replace from here
            ArgumentNullException.ThrowIfNull(sessionId);

            // Retrieve conversation, including latest prompt.
            var archivedMessages = await cosmosDBService.GetSessionMessagesAsync(tenantId, userId, sessionId);

            // Add both prompt and completion to cache, then persist in Cosmos DB
            var userMessage = new Message(tenantId, userId, sessionId, "User", "User", userPrompt);

            // Generate the completion to return to the user
            var result = await skService.GetResponse(userMessage, archivedMessages, tenantId, userId);

            await AddPromptCompletionMessagesAsync(tenantId, userId, sessionId, userMessage, result.Item1, result.Item2);

            return result.Item1;
            //end replace
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Error getting completion in session sessionId for user prompt [userPrompt].");
            return new List<Message> { new Message(tenantId, userId, sessionId!,
                "Error", "Error", "Error getting completion in session {sessionId} for user prompt [{userPrompt}].") };
        }
    }

    /// <summary>
    /// Generate a name for a chat message, based on the passed in prompt.
    /// </summary>
    public async Task<string> SummarizeChatSessionNameAsync(string tenantId, string userId, string? sessionId, string prompt)
    {
        try
        {
            ArgumentNullException.ThrowIfNull(sessionId);

            var summary = await skService.Summarize(sessionId, prompt);

            var session = await RenameChatSessionAsync(tenantId, userId, sessionId, summary);

            return session.Name;
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "Error getting a summary in session sessionId for user prompt [prompt].");
            return "Error getting a summary in session {sessionId} for user prompt [{prompt}].";
        }
    }


    /// <summary>
    /// Rate an assistant message. This can be used to discover useful AI responses for training, discoverability, and other benefits down the road.
    /// </summary>
    public async Task<Message> RateChatCompletionAsync(string tenantId, string userId,string messageId, string sessionId, bool? rating)
    {
        ArgumentNullException.ThrowIfNull(messageId);
        ArgumentNullException.ThrowIfNull(sessionId);

        return await cosmosDBService.UpdateMessageRatingAsync(tenantId,userId, sessionId, messageId,rating);
    }

    public async Task<DebugLog> GetChatCompletionDebugLogAsync(string tenantId, string userId,string sessionId, string debugLogId)
    {
        ArgumentException.ThrowIfNullOrEmpty(sessionId);
        ArgumentException.ThrowIfNullOrEmpty(debugLogId);

        return await cosmosDBService.GetChatCompletionDebugLogAsync(tenantId,userId, sessionId, debugLogId);
    }


    public async Task<bool> AddDocument(string containerName, JsonElement document)
    {
        try
        {
            // Extract raw JSON from JsonElement
            var json = document.GetRawText();
            var docJObject = JsonConvert.DeserializeObject<JObject>(json);

            // Ensure "id" exists
            if (!docJObject.ContainsKey("id"))
            {
                throw new ArgumentException("Document must contain an 'id' property.");
            }
           
            return await cosmosDBService.InsertDocumentAsync(containerName, docJObject);

        }
        catch (Exception ex)
        {
            logger.LogError(ex, $"Error adding document to container {containerName}.");
            return false;
        }
    }

    /// <summary>
    /// Add user prompt and AI assistance response to the chat session message list object and insert into the data service as a transaction.
    /// </summary>
    private async Task AddPromptCompletionMessagesAsync(string tenantId, string userId, string sessionId, Message promptMessage, List<Message> completionMessages, List<DebugLog> completionMessageLogs)
    {
        var session = await cosmosDBService.GetSessionAsync(tenantId, userId, sessionId);

        completionMessages.Insert(0, promptMessage);
        await _cosmosDBService.UpsertSessionBatchAsync(completionMessages, completionMessageLogs, session);
    }
}
```
</details>



Resources

• Semantic Kernel Agent Framework:https://learn.microsoft.com/semantic-kernel/frameworks/agent

• LangGraph:https://langchain-ai.github.io/langgraph/concepts/

• Azure OpenAI Service documentation:https://learn.microsoft.com/azure/cognitive-services/openai/

• Azure Cosmos DB Vector Database:https://learn.microsoft.com/azure/cosmos-db/vector-database