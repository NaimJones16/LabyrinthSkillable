###Activity 3: Instantiate Semantic Kernel Agent Framework and Connect to Azure OpenAI

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
