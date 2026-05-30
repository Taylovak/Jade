# 🤖 JADE - AI Agent System

**Jade** is a complete autonomous AI agent built on the Four Pillars architecture:
- 🧠 **Cognitive Engine**: OpenAI GPT-4 powered ReAct reasoning loop
- 💾 **Memory System**: PostgreSQL vector store for conversation history
- 🔧 **Tools Registry**: Extensible tool framework for autonomous actions
- 🎯 **Orchestration**: Clean ASP.NET Core API for seamless integration

## Architecture

```
┌─────────────────────────────────────────┐
│        Mobile App / Web Dashboard        │
└──────────────┬──────────────────────────┘
			   │
┌──────────────▼──────────────────────────┐
│   Jade.API (ASP.NET Core REST API)      │
│   ├─ ChatController                     │
│   └─ Conversation Management             │
└──────────────┬──────────────────────────┘
	 ┌─────────┼──────────────┬─────────┐
	 ▼         ▼              ▼         ▼
┌──────────┬─────────┬──────────────┬─────────┐
│Cognitive │ Memory  │ Tool         │ Models  │
│Engine    │ System  │ Registry     │ (DTOs)  │
│          │         │              │         │
│OpenAI    │Postgres │Calculator,   │Chat*,   │
│ReAct     │ Vector  │Search, Time  │Conv*    │
└──────────┴─────────┴──────────────┴─────────┘
```

## Quick Start

### Prerequisites
- .NET 10 SDK
- Docker & Docker Compose (for PostgreSQL)
- OpenAI API Key

### 1. Set OpenAI API Key

```bash
# On Windows
$env:OPENAI_API_KEY = "your-api-key-here"

# Or set in appsettings.json
```

### 2. Start PostgreSQL

```bash
cd C:\Users\toddg\OneDrive\Desktop\AI_Agent
docker-compose up -d
```

### 3. Build the Solution

```bash
dotnet build Jade.slnx
```

### 4. Run Database Migrations

```bash
cd src/Jade.API
dotnet ef database update
```

### 5. Start Jade API

```bash
cd src/Jade.API
dotnet run
```

API will be available at: `https://localhost:7299`

## API Endpoints

### Chat with Jade
```bash
POST /api/chat/message
Content-Type: application/json

{
  "message": "What's the capital of France?",
  "conversationId": "optional-guid",
  "systemPrompt": "optional override"
}
```

### Create Conversation
```bash
POST /api/chat/conversations?title=My%20Chat
```

### Get Conversation History
```bash
GET /api/chat/conversations/{conversationId}
```

### List All Conversations
```bash
GET /api/chat/conversations?pageSize=20&pageNumber=0
```

### Get Available Tools
```bash
GET /api/chat/tools
```

### Health Check
```bash
GET /health
```

## Project Structure

```
Jade/
├── src/
│   ├── Jade.Models/          # Shared data transfer objects
│   │   ├── ChatMessage.cs    # Message entity
│   │   ├── Conversation.cs   # Conversation entity
│   │   └── ChatDto.cs        # Request/Response DTOs
│   │
│   ├── Jade.Core/            # Cognitive engine (THE BRAIN)
│   │   └── JadeCognitiveEngine.cs  # ReAct loop implementation
│   │
│   ├── Jade.Memory/          # Memory system (THE CONTEXT)
│   │   └── JadeMemoryContext.cs    # PostgreSQL + EF Core
│   │
│   ├── Jade.Tools/           # Tool registry (THE HANDS)
│   │   └── ToolRegistry.cs   # Tool execution framework
│   │
│   └── Jade.API/             # REST API (THE NERVOUS SYSTEM)
│       ├── Controllers/
│       ├── Program.cs
│       └── appsettings.json
│
├── tests/
│   └── Jade.Tests/           # Unit tests
│
├── docker-compose.yml        # PostgreSQL + pgAdmin
└── README.md
```

## Key Components

### 1. Cognitive Engine (Jade.Core)
Implements the **ReAct (Reasoning + Acting)** pattern:
- **Thought**: What should I do?
- **Action**: Execute a tool or provide answer
- **Observation**: What happened?

Uses OpenAI's GPT-4 model for intelligent reasoning.

### 2. Memory System (Jade.Memory)
- Stores conversations in PostgreSQL
- Supports conversation history retrieval
- Ready for vector embeddings and semantic search

### 3. Tool Registry (Jade.Tools)
Built-in tools:
- **Calculator**: Math operations
- **Web Search**: Search the web (stub)
- **DateTime**: Current date/time

Easily add new tools by implementing `ITool` interface.

### 4. REST API (Jade.API)
Clean ASP.NET Core endpoints for:
- Sending messages to Jade
- Managing conversations
- Retrieving tools
- Health checks

## Configuration

### Environment Variables
```bash
OPENAI_API_KEY=sk-...              # Your OpenAI API key
ConnectionStrings__JadeDb=...      # PostgreSQL connection string (optional)
```

### appsettings.json
```json
{
  "ConnectionStrings": {
	"JadeDb": "Host=localhost;Database=jade_db;Username=postgres;Password=postgres"
  },
  "OpenAI": {
	"ApiKey": ""  // Set via environment variable
  }
}
```

## Extending Jade

### Adding a New Tool

```csharp
public class MyCustomTool : ITool
{
	public string Name => "my_tool";
	public string Description => "What this tool does";

	public async Task<string> ExecuteAsync(string input)
	{
		// Your tool logic here
		return "result";
	}
}

// Register in Program.cs
registry.Register(new MyCustomTool());
```

### Adding New Endpoints

Create a new controller in `Jade.API/Controllers/` and inherit from `ControllerBase`.

## Next Steps

- [ ] Build mobile app (iOS/Android) using .NET MAUI
- [ ] Integrate web search API (Tavily or Bing)
- [ ] Add document upload and processing
- [ ] Implement vector embeddings for semantic search
- [ ] Add conversation tagging and search
- [ ] Deploy to Azure/AWS
- [ ] Add authentication and user management
- [ ] Implement streaming responses
- [ ] Add system telemetry and analytics

## Resources

- [OpenAI API Documentation](https://platform.openai.com/docs)
- [ASP.NET Core Documentation](https://learn.microsoft.com/en-us/aspnet/core/)
- [Entity Framework Core](https://learn.microsoft.com/en-us/ef/core/)
- [ReAct Paper](https://arxiv.org/abs/2210.03629)

## License

MIT

---

**Created**: May 29, 2026
**Version**: 1.0.0-beta
**Status**: 🚀 Ready for development
