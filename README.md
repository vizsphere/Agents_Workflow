# Multi-Agent Translation Workflow

A simple example demonstrating agent orchestration using Microsoft Agent Framework with Azure OpenAI.

## Overview

This sample creates a translation workflow that chains two specialized agents:
- English translation agent
- French translation agent

The workflow processes text through both agents sequentially, demonstrating how to build coordinated multi-agent systems.

## Features

- **Agent Composition**: Multiple specialized agents working in sequence
- **Persistent Context**: Thread management for conversation continuity
- **Azure Integration**: Secure authentication with Azure CLI credentials
- **Clean Architecture**: Declarative workflow building with type safety

## Prerequisites

- .NET 8.0 or higher
- Azure OpenAI service deployment
- Azure CLI installed and authenticated

## Configuration

Set environment variables:
```bash
AZURE_MODEL=your-model-name
AZURE_ENDPOINT=your-azure-openai-endpoint
```

## Code Structure

```csharp
// Create specialized agents
AIAgent englishAgent = await GetTranslationAgentAsync("English", client, model);
AIAgent frenchAgent = await GetTranslationAgentAsync("French", client, model);

// Build workflow
var workflow = new WorkflowBuilder(englishAgent)
    .AddEdge(englishAgent, frenchAgent)
    .Build();

// Execute
AIAgent workflowAgent = workflow.AsAgent();
AgentThread thread = workflowAgent.GetNewThread();
await workflowAgent.RunAsync("Greeting users!", thread);
```

## Key Concepts

- **Workflow**: Orchestrates agent execution sequence
- **Thread**: Maintains conversation context across agent interactions
- **PersistentAgentsClient**: Manages agent lifecycle and Azure connectivity

## Cleanup

The sample includes proper resource cleanup, deleting agents after execution.

