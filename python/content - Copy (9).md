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
