# Project Structure - Jade AI Agent

```
Jade/
│
├── 📄 Jade.slnx                        ← Solution file (modern format)
├── 📄 Jade.sln                         ← Alternative solution file
│
├── 📂 src/                             ← Source code directory
│   │
│   ├── 📂 Jade.Models/                 ← SHARED DATA TRANSFER OBJECTS
│   │   ├── Jade.Models.csproj
│   │   ├── ChatMessage.cs              # Chat message entity
│   │   ├── Conversation.cs             # Conversation entity
│   │   ├── ChatDto.cs                  # Request/Response DTOs
│   │   └── Class1.cs                   # Default template (can delete)
│   │
│   ├── 📂 Jade.Core/                   ← 🧠 COGNITIVE ENGINE (THE BRAIN)
│   │   ├── Jade.Core.csproj
│   │   ├── JadeCognitiveEngine.cs      # ReAct loop implementation
│   │   │                                # - IJadeCognitiveEngine interface
│   │   │                                # - OpenAIReActEngine implementation
│   │   │                                # - AgentResponse class
│   │   │                                # - ReActStep class
│   │   │                                # - ToolExecution class
│   │   ├── bin/                        # Compiled output
│   │   └── obj/                        # Build artifacts
│   │
│   ├── 📂 Jade.Memory/                 ← 💾 MEMORY SYSTEM (THE CONTEXT)
│   │   ├── Jade.Memory.csproj
│   │   ├── JadeMemoryContext.cs        # DbContext & Storage
│   │   │                                # - JadeMemoryContext (EF Core)
│   │   │                                # - IMemoryStore interface
│   │   │                                # - PostgreSQLMemoryStore impl.
│   │   ├── bin/
│   │   └── obj/
│   │
│   ├── 📂 Jade.Tools/                  ← 🔧 TOOLS & EXECUTION (THE HANDS)
│   │   ├── Jade.Tools.csproj
│   │   ├── ToolRegistry.cs             # Tool framework & built-in tools
│   │   │                                # - ITool interface
│   │   │                                # - IToolRegistry interface
│   │   │                                # - ToolRegistry implementation
│   │   │                                # - CalculatorTool
│   │   │                                # - WebSearchTool (stub)
│   │   │                                # - DateTimeTool
│   │   ├── bin/
│   │   └── obj/
│   │
│   └── 📂 Jade.API/                    ← 🌐 REST API (THE NERVOUS SYSTEM)
│       ├── Jade.API.csproj
│       ├── Program.cs                  # Startup & DI configuration
│       │                                # - Service registration
│       │                                # - CORS setup
│       │                                # - Database migrations
│       ├── appsettings.json            # Configuration
│       │                                # - Connection strings
│       │                                # - OpenAI settings
│       ├── 📂 Controllers/
│       │   └── ChatController.cs       # REST API endpoints
│       │                                # - POST /api/chat/message
│       │                                # - GET /api/chat/conversations/*
│       │                                # - POST /api/chat/conversations
│       │                                # - DELETE /api/chat/conversations
│       │                                # - GET /api/chat/tools
│       │                                # - GET /health
│       ├── 📂 Properties/
│       │   └── launchSettings.json     # Debug configuration
│       ├── bin/
│       └── obj/
│
├── 📂 tests/                           ← UNIT TESTS & INTEGRATION TESTS
│   └── 📂 Jade.Tests/
│       ├── Jade.Tests.csproj           # xUnit test project
│       ├── UnitTest1.cs                # Placeholder test
│       ├── bin/
│       └── obj/
│
├── 📂 deploy/                          ← DEPLOYMENT ARTIFACTS
│   ├── docker-compose.yml              # PostgreSQL + pgAdmin services
│   ├── Dockerfile                      # Container image (create as needed)
│   └── kubernetes/                     # K8s manifests (create as needed)
│
├── 📄 docker-compose.yml               ← 🐳 DATABASE SETUP
│                                        # - PostgreSQL 16 (alpine)
│                                        # - pgAdmin for management
│                                        # - Volumes for data persistence
│
├── 📄 README.md                         ← PROJECT OVERVIEW
│                                        # - Architecture summary
│                                        # - Quick start guide
│                                        # - API endpoints reference
│                                        # - Extension guide
│
├── 📄 QUICKSTART.md                    ← ⚡ SETUP INSTRUCTIONS
│                                        # - Prerequisites checklist
│                                        # - Step-by-step setup
│                                        # - API testing examples
│                                        # - Troubleshooting guide
│
├── 📄 ARCHITECTURE.md                  ← 🏗️ DETAILED DESIGN
│                                        # - Four Pillars explained
│                                        # - Data flow diagrams
│                                        # - ReAct loop details
│                                        # - Database schema
│                                        # - API endpoint specs
│                                        # - Tool creation guide
│                                        # - Deployment checklist
│
├── 📄 start-jade.ps1                   ← 🚀 STARTUP SCRIPT
│                                        # - One-command startup
│                                        # - Database initialization
│                                        # - API server launch
│
├── 📄 .gitignore                       ← Git ignore patterns
├── 📄 global.json                      ← .NET SDK version
│
└── 📂 .vs/                             ← Visual Studio cache
	├── AI_Agent/                       # Solution artifacts
	├── AI_Agent.slnx/                  # Solution config
	└── (various cache files)
```

---

## 📊 File Count Summary

| Category | Count | Purpose |
|----------|-------|---------|
| **C# Source Files** | 8 | Core logic & API |
| **Configuration Files** | 5 | Settings & setup |
| **Documentation** | 4 | Guides & reference |
| **Project Files** | 6 | .csproj & .sln |
| **Infrastructure** | 2 | Docker & deployment |
| **Scripts** | 1 | Automation |
| **Generated** | 6+ | Build output (bin/obj) |

---

## 🔗 Key Dependencies Between Projects

```
Jade.API (Orchestrator)
  ├─ → Jade.Core (Cognitive Engine)
  │     └─ → Jade.Models
  │
  ├─ → Jade.Memory (Storage)
  │     └─ → Jade.Models
  │
  ├─ → Jade.Tools (Tools)
  │
  └─ → Jade.Models (DTOs)

Jade.Tests (Quality Assurance)
  ├─ → Jade.Core
  ├─ → Jade.Memory
  ├─ → Jade.Tools
  └─ → Jade.Models
```

---

## 📝 Important Configuration Files

### appsettings.json
Located: `src/Jade.API/appsettings.json`
```json
{
  "ConnectionStrings": {
	"JadeDb": "Host=localhost;Database=jade_db;..."
  },
  "OpenAI": {
	"ApiKey": ""  // Set via environment variable
  }
}
```

### docker-compose.yml
Located: Root directory
```yaml
services:
  postgres:
	image: postgres:16-alpine
	ports:
	  - "5432:5432"
  pgadmin:
	image: dpage/pgadmin4:latest
	ports:
	  - "5050:80"
```

### Jade.API.csproj
Located: `src/Jade.API/Jade.API.csproj`
- Framework: net10.0
- References: All 4 core projects
- Key Packages: OpenAI, EntityFrameworkCore, Npgsql

---

## 🗂️ Generated Directories (After Build)

```
Each project creates on build:

├── bin/
│   └── Release/
│       └── net10.0/
│           ├── *.dll          # Compiled assemblies
│           ├── *.deps.json    # Dependencies
│           ├── *.runtimeconfig.json
│           └── publish/       # Ready to deploy
│
└── obj/
	├── project.assets.json    # Package resolution
	├── project.nuget.g.props  # NuGet properties
	└── (various build files)
```

---

## 🚀 Building & Output

### Debug Build
```bash
dotnet build Jade.slnx --configuration Debug
```
Output: `bin/Debug/net10.0/`

### Release Build
```bash
dotnet build Jade.slnx --configuration Release
```
Output: `bin/Release/net10.0/`

### Publish Ready
```bash
dotnet publish src/Jade.API/Jade.API.csproj -c Release
```
Output: `bin/Release/net10.0/publish/`

---

## 📚 Next Steps

1. **Explore Code**: Start with `src/Jade.API/Program.cs` to see DI setup
2. **Test API**: Use `QUICKSTART.md` to run and test endpoints
3. **Extend Tools**: Add custom tools to `src/Jade.Tools/ToolRegistry.cs`
4. **Implement Database**: Run migrations and explore `JadeMemoryContext.cs`
5. **Build Mobile**: Create MAUI app referencing `Jade.API` endpoints

---

**Generated**: May 29, 2026
**Total Lines of Code**: ~1,200+ (Core + API + Tests)
**Build Status**: ✅ Compiling Successfully
**Runtime**: .NET 10 (ASP.NET Core 10)
