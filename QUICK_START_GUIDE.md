# 🎯 JADE AI AGENT - QUICK START VISUAL GUIDE

## 🚀 THREE STEPS TO LAUNCH

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  STEP 1: SET ENVIRONMENT VARIABLE                              │
│  ───────────────────────────────────────────────────────────   │
│                                                                 │
│  Windows Key → Type "Environment"                              │
│  Click "Edit the system environment variables"                 │
│  Click "Environment Variables..." button                       │
│  Click "New..." under "User variables"                         │
│                                                                 │
│  Variable name:  OPENAI_API_KEY                                │
│  Variable value: sk-your-actual-key-from-openai               │
│                                                                 │
│  Click OK (3 times), then RESTART YOUR COMPUTER               │
│                                                                 │
│  ⏱️  TIME: 5 minutes                                             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

							  👇

┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  STEP 2: START DOCKER DESKTOP                                  │
│  ───────────────────────────────────────────────────────────   │
│                                                                 │
│  Windows Start Menu → Search "Docker Desktop"                  │
│  Click to launch                                               │
│  Wait 30-60 seconds for startup                                │
│  Check system tray for Docker icon                             │
│                                                                 │
│  ⏱️  TIME: 2 minutes                                             │
│                                                                 │
│  📌 NOTE: Keep Docker running while using Jade                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

							  👇

┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  STEP 3: LAUNCH JADE (INSTANT!)                                │
│  ───────────────────────────────────────────────────────────   │
│                                                                 │
│  Look on your Desktop for:                                     │
│  "Jade AI Agent.lnk"                                           │
│                                                                 │
│  👉 DOUBLE-CLICK IT                                             │
│                                                                 │
│  PowerShell opens and shows:                                   │
│  ✅ OpenAI API key verified                                     │
│  ✅ PostgreSQL started                                          │
│  ✅ Migrations applied                                          │
│  ✅ API started                                                 │
│                                                                 │
│  Wait for this message:                                        │
│  "📡 API READY"                                                 │
│                                                                 │
│  ⏱️  TIME: 20-30 seconds                                         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

							  ✨ DONE!
```

---

## 💬 SEND YOUR FIRST MESSAGE

### Option A: Easy Web Interface (Recommended)

```
1. Open your web browser
2. Go to: https://localhost:7299/swagger
3. Find: POST /api/chat/message
4. Click: "Try it out"
5. Enter in request body:

   {
	 "message": "What is 2 + 2?"
   }

6. Click: "Execute"
7. View: Jade's response with reasoning process
```

### Option B: PowerShell Command

```powershell
$body = @{
	message = "Hello Jade, what is 25 * 4?"
} | ConvertTo-Json

curl -k -X POST https://localhost:7299/api/chat/message `
  -Headers @{"Content-Type"="application/json"} `
  -Body $body
```

### Option C: Testing Tools
- **Postman**: https://www.postman.com/downloads/
- **Insomnia**: REST API client
- **VS Code**: REST Client extension

---

## 🎯 WHAT JADE CAN DO

### Simple Questions
```
You: "What is 2 + 2?"
Jade: Uses Calculator tool
Result: ✅ "2 + 2 = 4"
```

### Time Questions
```
You: "What time will it be in 3 hours?"
Jade: Uses DateTime tool
Result: ✅ Calculates and returns time
```

### Complex Reasoning
```
You: "Plan steps to build an AI system"
Jade: Reasons through architecture
Result: ✅ Provides structured plan with reasoning
```

### Multi-Turn Conversations
```
You: "What is an AI?"
Jade: Explains AI concepts

You: "How does machine learning work?"
Jade: Continues discussion based on context
Result: ✅ Remembers previous messages
```

---

## 📱 API ENDPOINTS

```
┌─────────────────────────────────────────────────────────────────┐
│                                                                 │
│  📡 JADE API - AVAILABLE ENDPOINTS                              │
│  https://localhost:7299                                        │
│                                                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  🏥 HEALTH CHECK                                                 │
│  GET /health                                                   │
│  → Check if API is running                                     │
│                                                                 │
│  💬 SEND MESSAGE                                                 │
│  POST /api/chat/message                                        │
│  → Send message to Jade                                        │
│  → Returns: Jade's response with reasoning                     │
│                                                                 │
│  📋 LIST CONVERSATIONS                                           │
│  GET /api/chat/conversations                                   │
│  → Get all conversations                                       │
│                                                                 │
│  ➕ CREATE CONVERSATION                                          │
│  POST /api/chat/conversations                                  │
│  → Start new conversation                                      │
│                                                                 │
│  📖 GET CONVERSATION                                             │
│  GET /api/chat/conversations/{id}                              │
│  → Get specific conversation with history                      │
│                                                                 │
│  🗑️  DELETE CONVERSATION                                         │
│  DELETE /api/chat/conversations/{id}                           │
│  → Remove conversation                                         │
│                                                                 │
│  🔧 LIST TOOLS                                                   │
│  GET /api/chat/tools                                           │
│  → See available tools Jade can use                            │
│                                                                 │
│  📚 API DOCUMENTATION                                            │
│  GET /swagger                                                  │
│  → Interactive API documentation                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🎯 WHAT YOU HAVE

```
✅ BACKEND
   ├─ ReAct Cognitive Engine (autonomous reasoning)
   ├─ GPT-4 Integration (OpenAI)
   ├─ PostgreSQL Database (persistent memory)
   ├─ Tool Registry (extensible tools)
   └─ REST API (8 endpoints)

✅ INFRASTRUCTURE
   ├─ Docker Compose (PostgreSQL + pgAdmin)
   ├─ EF Core (database ORM)
   ├─ ASP.NET Core (web framework)
   └─ .NET 10.0 (runtime)

✅ DEPLOYMENT
   ├─ Published API (ready to run)
   ├─ PowerShell launcher (automated)
   ├─ Batch file launcher (alternative)
   ├─ Desktop shortcut (one-click)
   └─ Comprehensive docs (15 files)

✅ TOOLS
   ├─ Calculator (math expressions)
   ├─ Web Search (extensible)
   └─ DateTime (time operations)
```

---

## 🆘 QUICK TROUBLESHOOTING

### "API Key not found"
```
Fix: 
1. Open CMD and type: echo %OPENAI_API_KEY%
2. If empty, set it in Environment Variables
3. Restart computer
4. Try again
```

### "Docker not running"
```
Fix:
1. Open Windows Start
2. Search "Docker Desktop"
3. Launch it
4. Wait 1-2 minutes
5. Try launching Jade again
```

### "Connection refused"
```
Fix:
1. Wait 30 seconds (API may still be starting)
2. Check PowerShell window for error messages
3. Verify Docker containers running: docker ps
4. Check logs: docker logs jade-postgres
```

### "Port 7299 already in use"
```
Fix:
1. Stop other Jade instance (close PowerShell)
2. Or use different port: edit deploy-jade.ps1
3. Change: $ApiPort = 7299 to different port
```

---

## 📊 SYSTEM ARCHITECTURE

```
┌─────────────────────────────────────────────────────────────────┐
│                          USER/CLIENT                            │
│                                                                 │
│         Web Browser / REST Client / PowerShell Curl             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
							  ↓ HTTP/HTTPS
┌─────────────────────────────────────────────────────────────────┐
│                      ASP.NET CORE API                            │
│                    https://localhost:7299                       │
│                                                                 │
│         Controllers: Chat, Health, Swagger                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
							  ↓
┌─────────────────────────────────────────────────────────────────┐
│                  JADE COGNITIVE ENGINE                           │
│                                                                 │
│         Thought → Action → Observation → FinalAnswer            │
│                                                                 │
│         ReAct Loop with Autonomous Reasoning                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
							  ↓
		┌─────────────────────┼─────────────────────┐
		↓                     ↓                     ↓
   ┌─────────┐      ┌────────────────┐      ┌─────────────┐
   │ TOOLS   │      │ MEMORY/CONTEXT │      │ OPENAI API  │
   ├─────────┤      ├────────────────┤      ├─────────────┤
   │Calculator│      │ EF Core DbCtx  │      │ GPT-4       │
   │WebSearch │      │ PostgreSQL     │      │ Reasoning   │
   │DateTime  │      │ Conversations  │      │ Planning    │
   └─────────┘      │ Messages       │      └─────────────┘
					└────────────────┘
							  ↓
					┌─────────────────────┐
					│    POSTGRESQL DB    │
					│   (Docker Container)│
					│                     │
					│ Persistent Storage  │
					│ of Conversations &  │
					│ Message History     │
					└─────────────────────┘
```

---

## 📚 DOCUMENTATION ROADMAP

```
START HERE
	↓
	START_HERE.md (2 min) ← First read!
	↓
	LAUNCH_CHECKLIST.md (10 min) ← Before launching
	↓
	DEPLOYMENT_COMPLETE.md (reference) ← Setup help
	↓
	ARCHITECTURE.md (15 min) ← Understand design
	↓
	README.md (reference) ← Full documentation
	↓
	Other files (reference) ← As needed
```

---

## ⏱️ TIMELINE

```
Today:
  5 min  → Set environment variable + restart computer
  2 min  → Start Docker Desktop
  20 sec → Double-click desktop shortcut

  ✅ Jade is running!

Next Steps (optional):
  5 min  → Explore Swagger UI
  10 min → Send test messages
  30 min → Add custom tools
  1 hour → Plan extensions
```

---

## 🎓 LEARNING RESOURCES

### Built Into This Package
- **Swagger UI**: https://localhost:7299/swagger
  - Interactive API documentation
  - Try endpoints in browser
  - See real request/response

- **Documentation Files**: 15 comprehensive guides
  - Setup guides
  - Architecture overview
  - Troubleshooting
  - Reference materials

### External Resources
- **OpenAI API**: https://platform.openai.com/docs
- **.NET Docs**: https://docs.microsoft.com/dotnet
- **Entity Framework**: https://docs.microsoft.com/ef/core
- **ASP.NET Core**: https://docs.microsoft.com/aspnet/core

---

## ✨ YOU'RE ALL SET!

Everything is ready to go. No more installation needed.

**Next step: Double-click the desktop shortcut!** 🚀

---

## 🎉 SUMMARY

| What | Where | Status |
|------|-------|--------|
| Desktop Shortcut | Desktop | ✅ Ready |
| API | https://localhost:7299 | ✅ After launch |
| Database | Docker + PostgreSQL | ✅ Auto-started |
| Docs | 15 files in project folder | ✅ Ready |
| Swagger UI | https://localhost:7299/swagger | ✅ After launch |

---

**Created:** December 2024  
**Status:** ✅ Ready for Launch  
**Next Action:** Double-click "Jade AI Agent.lnk" on your desktop  

**Enjoy your autonomous AI agent!** 🤖✨
