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