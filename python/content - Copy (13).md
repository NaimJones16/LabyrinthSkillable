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