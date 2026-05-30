# 🚀 JADE - START HERE

**Welcome to Jade, your autonomous AI agent!**

This file will get you running in **5 minutes**.

---

## ⚡ 5-Minute Quickstart

### Step 1: Verify Prerequisites (1 min)

```powershell
# Check .NET is installed
dotnet --version
# Should show: 10.0.300 or higher

# Check Docker is running
docker ps
# Should show: containers table (may be empty)
```

If either fails, see "Prerequisites" section below.

### Step 2: Set Your OpenAI Key (1 min)

```powershell
# Replace 'sk-...' with your actual API key
$env:OPENAI_API_KEY = "sk-your-api-key-here"

# Verify it was set
$env:OPENAI_API_KEY
# Should output: sk-your-api-key-here
```

**Don't have an API key?** Get one free at: https://platform.openai.com

### Step 3: Run the Startup Script (2 min)

```powershell
cd "C:\Users\toddg\OneDrive\Desktop\AI_Agent"
.\start-jade.ps1
```

This will:
- ✅ Start PostgreSQL database
- ✅ Wait for it to be ready
- ✅ Run database migrations
- ✅ Start the Jade API server

**Expected output:**
```
Now listening on: https://localhost:7299
```

### Step 4: Test It! (1 min)

**Open NEW PowerShell window** and run:

```powershell
$body = @{ message = "What is 2 + 2?" } | ConvertTo-Json

curl -X POST https://localhost:7299/api/chat/message `
  -H "Content-Type: application/json" `
  -d $body `
  --insecure
```

**Response should be JSON with Jade's answer!**

---

## 📋 Prerequisites

### What You Need

- **Windows 10/11** with PowerShell 7+
- **.NET 10 SDK** - [Download](https://dotnet.microsoft.com/download)
- **Docker Desktop** - [Download](https://www.docker.com/products/docker-desktop)
- **OpenAI API Key** - [Get Free](https://platform.openai.com)

### Verify Installation

```powershell
# .NET
dotnet --version
# Expected: 10.0.300+

# Docker
docker --version
# Expected: Docker version 20.10+

# PowerShell
$PSVersionTable.PSVersion
# Expected: 7.0+
```

### If Missing

**Install .NET 10:**
```powershell
# Download from https://dotnet.microsoft.com/download
# Or use package manager:
winget install Microsoft.DotNet.SDK.10
```

**Install Docker:**
```powershell
winget install Docker.DockerDesktop
```

**Restart computer after installation**

---

## 🎮 Testing Jade (After Startup)

### Using PowerShell

```powershell
# Test 1: Simple question
$body = @{ message = "Hello Jade!" } | ConvertTo-Json
curl -X POST https://localhost:7299/api/chat/message `
  -H "Content-Type: application/json" `
  -d $body `
  --insecure

# Test 2: Math question
$body = @{ message = "What is 15 * 3?" } | ConvertTo-Json
curl -X POST https://localhost:7299/api/chat/message `
  -H "Content-Type: application/json" `
  -d $body `
  --insecure

# Test 3: Get current time
$body = @{ message = "What time is it?" } | ConvertTo-Json
curl -X POST https://localhost:7299/api/chat/message `
  -H "Content-Type: application/json" `
  -d $body `
  --insecure

# Test 4: List available tools
curl -X GET https://localhost:7299/api/chat/tools `
  --insecure
```

### Using Postman

1. Open Postman (or install from https://postman.com)
2. Create new **POST** request
3. URL: `https://localhost:7299/api/chat/message`
4. Headers: `Content-Type: application/json`
5. Body (raw JSON):
```json
{
  "message": "What is 2 + 2?",
  "conversationId": null,
  "systemPrompt": null
}
```
6. Click **Send**
7. See response!

### Using VS Code REST Client

1. Install extension: REST Client
2. Create file `test.http`:
```http
### Test Jade Chat
POST https://localhost:7299/api/chat/message HTTP/1.1
Content-Type: application/json

{
  "message": "What is 2 + 2?"
}
```
3. Click **Send Request** above the POST line
4. See response in side panel

---

## 📂 What Got Created

```
C:\Users\toddg\OneDrive\Desktop\AI_Agent\
│
├── 📚 DOCUMENTATION (start here!)
│   ├── DELIVERY_SUMMARY.md          ← Complete overview
│   ├── QUICKSTART.md                ← Detailed setup
│   ├── README.md                    ← Architecture
│   ├── ARCHITECTURE.md              ← Technical details
│   ├── PROJECT_STRUCTURE.md         ← File layout
│   ├── FILE_INDEX.md                ← File reference
│   └── COMPLETE.md                  ← Next steps
│
├── 💻 SOURCE CODE
│   └── src/
│       ├── Jade.Core/               ← AI brain
│       ├── Jade.Memory/             ← Database
│       ├── Jade.Tools/              ← Tools
│       ├── Jade.Models/             ← Data
│       └── Jade.API/                ← REST API
│
├── 🧪 TESTS
│   └── tests/Jade.Tests/
│
├── ⚙️  CONFIGURATION
│   ├── docker-compose.yml           ← Database setup
│   ├── global.json                  ← .NET version
│   ├── Jade.slnx                    ← Solution file
│   └── start-jade.ps1               ← Startup script
│
└── 📄 PROJECT FILES
	└── Jade.sln, *.csproj files
```

---

## 🔄 Managing Jade

### Start Jade
```powershell
.\start-jade.ps1
```

### Stop Jade
Press **Ctrl+C** in the PowerShell terminal

### Stop Database (Optional)
```powershell
docker-compose down
```

### Restart Everything
```powershell
docker-compose down
.\start-jade.ps1
```

### View Logs
```powershell
# PostgreSQL logs
docker-compose logs postgres

# Database management UI
# Open browser to http://localhost:5050
# Email: admin@example.com
# Password: admin
```

---

## 🎯 Common Tasks

### Create New Conversation
```powershell
curl -X POST https://localhost:7299/api/chat/conversations?title=MyChat `
  --insecure
```

### Get All Conversations
```powershell
curl -X GET https://localhost:7299/api/chat/conversations `
  --insecure
```

### Send Message to Existing Conversation
```powershell
$body = @{ 
	message = "Follow up question"
	conversationId = "guid-from-above"
} | ConvertTo-Json

curl -X POST https://localhost:7299/api/chat/message `
  -H "Content-Type: application/json" `
  -d $body `
  --insecure
```

### Delete Conversation
```powershell
curl -X DELETE https://localhost:7299/api/chat/conversations/{conversation-id} `
  --insecure
```

---

## 🚨 Troubleshooting

### "OpenAI API Key not found"
```powershell
# Set it again in current PowerShell window:
$env:OPENAI_API_KEY = "sk-your-key"
```

### "Connection refused" on port 7299
```powershell
# API might not have started
# Check the startup output for errors
# Common cause: Missing OpenAI key
```

### "PostgreSQL container won't start"
```powershell
# Reset everything:
docker-compose down -v
docker-compose up -d postgres
```

### "Port 5432 already in use"
```powershell
# Stop other PostgreSQL instances:
docker ps
docker stop container-name
docker-compose up -d postgres
```

### Build errors
```powershell
# Clean and rebuild:
dotnet clean Jade.slnx
dotnet build Jade.slnx
```

---

## 📚 Next Steps

### 1. Learn the Basics (15 min)
Read: `README.md`

### 2. Understand Architecture (30 min)
Read: `ARCHITECTURE.md`

### 3. Add Custom Tool (1 hour)
See: `ARCHITECTURE.md` → "Tool Extension Guide"

### 4. Explore Code (1 hour)
Start: `src/Jade.API/Program.cs`
Then: `src/Jade.Core/JadeCognitiveEngine.cs`

### 5. Deploy (2 hours)
Follow: `ARCHITECTURE.md` → "Deployment Checklist"

---

## 🎁 What You Get

✅ Working AI agent with GPT-4  
✅ REST API with 7 endpoints  
✅ PostgreSQL database  
✅ Conversation history  
✅ Tool system (extensible)  
✅ Complete documentation  
✅ Docker setup  
✅ Production-ready code  

---

## 💡 Tips

### Speed Things Up
```powershell
# Run in Release mode (faster)
cd src/Jade.API
dotnet run --configuration Release
```

### Debug Issues
```powershell
# Run in Debug mode (more logging)
cd src/Jade.API
dotnet run --configuration Debug
```

### Access Database Directly
```bash
docker exec -it jade-postgres psql -U postgres -d jade_db

# Common queries:
SELECT * FROM "Conversations";
SELECT * FROM "ChatMessages";
```

### Use Better HTTP Client
Instead of cURL, install VS Code REST Client:
- Open VS Code
- Extensions → Search "REST Client"
- Install by Huachao Mao
- See example in `test.http` file

---

## ❓ FAQ

**Q: Can I use a different OpenAI model?**  
A: Yes! Edit `src/Jade.Core/JadeCognitiveEngine.cs` line with `_modelId = "gpt-4o"`

**Q: Can I change the database password?**  
A: Yes! Edit `docker-compose.yml` and change `postgres:postgres`

**Q: How do I deploy to production?**  
A: See `ARCHITECTURE.md` → "Deployment Checklist"

**Q: Can I add more tools?**  
A: Yes! See `ARCHITECTURE.md` → "Tool Extension Guide"

**Q: How do I make it faster?**  
A: Use Release build: `dotnet run --configuration Release`

**Q: Where's the mobile app?**  
A: Ready to build! Use .NET MAUI pointing to these API endpoints

---

## 🎉 You're Ready!

**1. Run**: `.\start-jade.ps1`  
**2. Test**: Send a message using curl/Postman  
**3. Explore**: Read the documentation  
**4. Build**: Extend with custom tools  
**5. Deploy**: Use Docker for production  

---

**Questions?** See the documentation files.  
**Ready to go?** Run `.\start-jade.ps1` now!  

**Enjoy Jade! 🤖**

---

Generated: May 29, 2026  
Status: ✅ Complete & Ready to Use
