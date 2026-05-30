# JADE - Complete Architecture & Implementation Guide

## 🏗️ Four Pillars Architecture

Jade is built on the Four Pillars of AI Agents:

```
┌─────────────────────────────────────────────────────────────┐
│              JADE AI AGENT SYSTEM (PRODUCTION)              │
├─────────────────────────────────────────────────────────────┤
│                                                               │
│  ┌──────────────────────────────────────────────────────┐   │
│  │            1. COGNITIVE ENGINE (THE BRAIN)           │   │
│  │                                                      │   │
│  │  • ReAct (Reasoning + Acting) Loop                 │   │
│  │  • OpenAI GPT-4 Model Integration                  │   │
│  │  • Prompt Engineering & System Instructions        │   │
│  │  • Thought → Action → Observation Cycle            │   │
│  │                                                      │   │
│  │  Location: src/Jade.Core/JadeCognitiveEngine.cs   │   │
│  └──────────────────────────────────────────────────────┘   │
│                           │                                  │
│  ┌──────────────────────────────────────────────────────┐   │
│  │         2. MEMORY SYSTEM (THE CONTEXT)              │   │
│  │                                                      │   │
│  │  • Conversation History Storage                     │   │
│  │  • PostgreSQL Database with pgvector               │   │
│  │  • Entity Framework Core ORM                        │   │
│  │  • RAG (Retrieval-Augmented Generation) Ready      │   │
│  │  • Semantic Search Capability                       │   │
│  │                                                      │   │
│  │  Location: src/Jade.Memory/JadeMemoryContext.cs   │   │
│  └──────────────────────────────────────────────────────┘   │
│                           │                                  │
│  ┌──────────────────────────────────────────────────────┐   │
│  │         3. TOOLS & EXECUTION (THE HANDS)            │   │
│  │                                                      │   │
│  │  • Tool Registry Interface                          │   │
│  │  • Built-in Tools:                                  │   │
│  │    - Calculator (Math Operations)                   │   │
│  │    - Web Search (Stub - Ready to Integrate)         │   │
│  │    - DateTime (Current Time)                        │   │
│  │  • Extensible Tool Framework                        │   │
│  │  • Safe Tool Execution                              │   │
│  │                                                      │   │
│  │  Location: src/Jade.Tools/ToolRegistry.cs         │   │
│  └──────────────────────────────────────────────────────┘   │
│                           │                                  │
│  ┌──────────────────────────────────────────────────────┐   │
│  │      4. ORCHESTRATION (THE NERVOUS SYSTEM)          │   │
│  │                                                      │   │
│  │  • ASP.NET Core REST API                            │   │
│  │  • Dependency Injection (DI)                        │   │
│  │  • HTTP Endpoints & CORS                            │   │
│  │  • Error Handling & Validation                      │   │
│  │  • Async/Await Pattern                              │   │
│  │                                                      │   │
│  │  Location: src/Jade.API/Controllers/               │   │
│  └──────────────────────────────────────────────────────┘   │
│                                                               │
└─────────────────────────────────────────────────────────────┘
```

---

## 📊 Data Flow Diagram

```
┌─────────────┐
│  User/App   │
└──────┬──────┘
	   │
	   ▼ POST /api/chat/message
┌─────────────────────────────────┐
│   ChatController                │
│  (Request Validation)           │
└──────┬──────────────────────────┘
	   │
	   ├─── Load Conversation ──────────┐
	   │                                 │
	   ▼                                 ▼
┌──────────────────────┐       ┌──────────────────┐
│  Cognitive Engine    │       │  Memory Store    │
│  (IJadeCognitiveEng.)│       │  (PostgreSQL)    │
│                      │       │                  │
│  1. Build Prompt     │       │  • Conversations│
│  2. Call OpenAI      │       │  • Messages     │
│  3. Parse Response   │       │  • Embeddings   │
│  4. Extract Answer   │       │  (Future: RAG)  │
│                      │       │                  │
└──────────────────────┘       └──────────────────┘
	   │                                 ▼
	   │                         Save Chat Message
	   │
	   └─────┬───────────────────┘
			 │
			 ▼ Build ChatResponse
	   ┌──────────────────────┐
	   │  ChatResponse DTO    │
	   │  • Message           │
	   │  • ReActSteps        │
	   │  • ToolsCalled       │
	   │  • ConversationId    │
	   └──────┬───────────────┘
			  │
			  ▼ HTTP 200 OK
		┌──────────────┐
		│  User / App  │
		└──────────────┘
```

---

## 🧠 ReAct (Reasoning + Acting) Loop

### How Jade Thinks

```
USER INPUT
	│
	▼
┌──────────────────────────────────────┐
│  THOUGHT: Analyze the question       │
│  "I need to find the capital of      │
│   France and provide a response"     │
└──────────────────────────────────────┘
	│
	▼
┌──────────────────────────────────────┐
│  ACTION: Use web_search tool         │
│  "search for: capital of France"     │
└──────────────────────────────────────┘
	│
	▼
┌──────────────────────────────────────┐
│  OBSERVATION: Tool returned result   │
│  "Paris is the capital of France"    │
└──────────────────────────────────────┘
	│
	▼
┌──────────────────────────────────────┐
│  FINAL ANSWER:                       │
│  "The capital of France is Paris"    │
└──────────────────────────────────────┘
	│
	▼
RETURN TO USER
```

### Code Implementation
See: `src/Jade.Core/JadeCognitiveEngine.cs`
- `ProcessMessageAsync()` - Main entry point
- `BuildReActPrompt()` - Creates ReAct system prompt
- `ParseFinalAnswer()` - Extracts final response

---

## 💾 Database Schema

### Conversations Table
```sql
CREATE TABLE "Conversations" (
	"Id" UUID PRIMARY KEY,
	"Title" VARCHAR(500),
	"SystemPrompt" TEXT,
	"Tags" VARCHAR(500),  -- Comma-separated
	"CreatedAt" TIMESTAMP,
	"UpdatedAt" TIMESTAMP
);
```

### ChatMessages Table
```sql
CREATE TABLE "ChatMessages" (
	"Id" UUID PRIMARY KEY,
	"ConversationId" UUID FOREIGN KEY,
	"Role" VARCHAR(50),      -- 'user', 'assistant', 'system', 'tool'
	"Content" TEXT,
	"ToolName" VARCHAR(255),
	"ToolCallId" VARCHAR(255),
	"CreatedAt" TIMESTAMP,
	"UpdatedAt" TIMESTAMP,
	INDEX "ConversationId"
);
```

### Embeddings Table (Future: Vector Search)
```sql
CREATE TABLE "Embeddings" (
	"Id" UUID PRIMARY KEY,
	"ConversationId" UUID FOREIGN KEY,
	"Content" TEXT,
	"Vector" VECTOR(1536),   -- OpenAI Ada Embedding Dimension
	"CreatedAt" TIMESTAMP
);
```

---

## 🔗 API Endpoints

### Chat Operations
```
POST   /api/chat/message
	   Send a message to Jade
	   Body: { message, conversationId?, systemPrompt? }
	   Response: { conversationId, message, reActSteps, toolsCalled, generatedAt }

GET    /api/chat/conversations/{conversationId}
	   Get conversation with all messages
	   Response: Conversation object

POST   /api/chat/conversations?title=MyChat
	   Create new conversation
	   Response: { id, title, systemPrompt, messages, tags, createdAt }

GET    /api/chat/conversations?pageSize=20&pageNumber=0
	   List conversations (paginated)
	   Response: Conversation[] (ordered by updatedAt DESC)

DELETE /api/chat/conversations/{conversationId}
	   Delete a conversation

GET    /api/chat/tools
	   List available tools
	   Response: [ { name, description }, ... ]

GET    /health
	   Health check
	   Response: { status: "Jade is ready!" }
```

---

## 🛠️ Tool Extension Guide

### Creating a Custom Tool

```csharp
using Jade.Tools;

namespace Jade.Tools;

public class MyCustomTool : ITool
{
	// 1. Unique tool name
	public string Name => "my_tool";

	// 2. Description for LLM to understand when to use it
	public string Description => 
		"Description of what this tool does. Used by Jade to decide when to call it.";

	// 3. Execution logic
	public async Task<string> ExecuteAsync(string input)
	{
		// Process input
		var result = DoSomething(input);

		// Return result as string
		return result;
	}
}
```

### Registering the Tool

Edit `src/Jade.API/Program.cs`:
```csharp
var registry = new ToolRegistry();
registry.Register(new CalculatorTool());
registry.Register(new WebSearchTool());
registry.Register(new DateTimeTool());
registry.Register(new MyCustomTool());  // ← Add your tool
```

### Built-in Tools Reference

#### Calculator
```csharp
// Input: "2 + 3 * 4"
// Output: "14"
// Pattern: Any valid math expression
```

#### Web Search (Stub)
```csharp
// Input: "latest AI news"
// Output: "Would connect to Bing/Tavily API"
// To implement: Add API integration
```

#### DateTime
```csharp
// Input: (any text)
// Output: "Current date and time: 2026-05-29 10:30:45"
```

---

## 📦 Project Dependencies

### Jade.Core (Cognitive Engine)
```xml
<PackageReference Include="OpenAI" Version="2.3.0" />
```

### Jade.Memory (Storage)
```xml
<PackageReference Include="Microsoft.EntityFrameworkCore" Version="8.0.5" />
<PackageReference Include="Npgsql.EntityFrameworkCore.PostgreSQL" Version="8.0.4" />
```

### Jade.Tools (Tool Registry)
```xml
<!-- No external dependencies - pure .NET -->
```

### Jade.API (REST API)
```xml
<PackageReference Include="Newtonsoft.Json" Version="13.0.3" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="8.0.5" />
```

---

## 🚀 Deployment Checklist

### Before Production

- [ ] Set `OPENAI_API_KEY` environment variable securely
- [ ] Configure PostgreSQL connection string
- [ ] Run `dotnet ef database update` migrations
- [ ] Test all endpoints with valid OpenAI API key
- [ ] Enable HTTPS/SSL certificates
- [ ] Set up logging and monitoring
- [ ] Implement authentication/authorization
- [ ] Add rate limiting
- [ ] Set up CI/CD pipeline

### Docker Deployment

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:10.0
WORKDIR /app
COPY bin/Release/net10.0/publish/ .
ENTRYPOINT ["dotnet", "Jade.API.dll"]
```

### Kubernetes Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: jade-api
spec:
  replicas: 3
  selector:
	matchLabels:
	  app: jade-api
  template:
	metadata:
	  labels:
		app: jade-api
	spec:
	  containers:
	  - name: jade
		image: your-registry/jade:latest
		env:
		- name: OPENAI_API_KEY
		  valueFrom:
			secretKeyRef:
			  name: openai-secrets
			  key: api-key
		ports:
		- containerPort: 80
```

---

## 🔄 Future Enhancements

### Phase 2: Advanced Memory
- [ ] Vector embeddings using OpenAI Embeddings API
- [ ] Semantic search with pgvector
- [ ] Conversation summarization
- [ ] Long-term memory management

### Phase 3: Extended Tools
- [ ] Web search (Tavily/Bing integration)
- [ ] Document upload & processing
- [ ] File operations
- [ ] Database queries
- [ ] External API calls

### Phase 4: Mobile & Web UI
- [ ] .NET MAUI mobile app (iOS/Android)
- [ ] React/Vue web dashboard
- [ ] Real-time chat with WebSockets
- [ ] Conversation management UI

### Phase 5: Enterprise Features
- [ ] Multi-user support with authentication
- [ ] Role-based access control (RBAC)
- [ ] Audit logging
- [ ] Compliance & security hardening
- [ ] Custom model fine-tuning

---

## 📚 Key Files Reference

| File | Purpose | Type |
|------|---------|------|
| `JadeCognitiveEngine.cs` | ReAct loop implementation | Core Logic |
| `JadeMemoryContext.cs` | Database context & ORM | Data Layer |
| `ToolRegistry.cs` | Tool management system | Plugin System |
| `ChatController.cs` | HTTP API endpoints | API Layer |
| `Program.cs` | Dependency injection setup | Configuration |
| `appsettings.json` | Configuration settings | Config |
| `docker-compose.yml` | Database services | Infrastructure |

---

## 🎓 Learning Resources

- **ReAct Paper**: https://arxiv.org/abs/2210.03629
- **OpenAI API Docs**: https://platform.openai.com/docs
- **ASP.NET Core**: https://learn.microsoft.com/en-us/aspnet/core/
- **Entity Framework Core**: https://learn.microsoft.com/en-us/ef/core/

---

**Version**: 1.0.0-beta  
**Last Updated**: May 29, 2026  
**Status**: ✅ Production Ready for Testing
