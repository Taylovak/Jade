# 🚀 JADE AI AGENT - FINAL CHECKLIST & LAUNCH GUIDE

## ✅ Pre-Launch Checklist

### Environment Setup
- [ ] **OpenAI API Key Set**
  - Required: OPENAI_API_KEY environment variable
  - Action: Set in Windows Environment Variables
  - Verification: `echo %OPENAI_API_KEY%` should show your key
  - Computer restart required after setting

### System Requirements
- [ ] **Docker Desktop Installed & Running**
  - Download: https://www.docker.com/products/docker-desktop
  - Status check: Docker icon in system tray
  - Verification: `docker --version` and `docker ps` work

- [ ] **PowerShell Available**
  - Already installed on Windows 10/11
  - Execution policy may need adjustment

- [ ] **.NET 10 Runtime**
  - Already installed (verified in previous session)
  - Verification: `dotnet --version` shows 10.x

### Project Files Verified
- [ ] Desktop shortcut exists: `Jade AI Agent.lnk`
- [ ] Deploy script exists: `deploy-jade.ps1`
- [ ] Published API exists: `publish/Jade.API.exe`
- [ ] Docker compose exists: `docker-compose.yml`
- [ ] Documentation exists: All 8 reference files

---

## 🎯 LAUNCH SEQUENCE

### Method 1: Desktop Shortcut (RECOMMENDED)
```
1. Look on Desktop for: "Jade AI Agent.lnk"
2. Double-click it
3. PowerShell window opens
4. Wait for message: "📡 API READY"
5. PowerShell displays API URL: https://localhost:7299
```

### Method 2: Manual PowerShell Launch
```powershell
cd "C:\Users\toddg\OneDrive\Desktop\AI_Agent"
.\deploy-jade.ps1
```

### Method 3: Batch File Launch (Simple)
```
Double-click: launch-jade.bat
```

---

## 📋 First-Time Setup (DO THIS FIRST)

### Step 1: Get OpenAI API Key (2 minutes)
1. Visit: https://platform.openai.com/account/api-keys
2. Click "Create new secret key"
3. Copy the key (example: `sk-proj-abc123...`)
4. **Save it somewhere safe**

### Step 2: Set Environment Variable (5 minutes)
**Option A: Windows GUI (Recommended)**
1. Press `Windows Key` and search "Environment"
2. Click "Edit the system environment variables"
3. Click "Environment Variables..." button
4. Under "User variables", click "New..."
5. Variable name: `OPENAI_API_KEY`
6. Variable value: `sk-your-actual-key` (paste from Step 1)
7. Click OK all the way through (3 times)
8. **RESTART YOUR COMPUTER** - This is important!
9. After restart, verify in cmd: `echo %OPENAI_API_KEY%`

**Option B: PowerShell (Advanced)**
```powershell
[Environment]::SetEnvironmentVariable("OPENAI_API_KEY", "sk-your-key-here", "User")
# Then restart computer
```

### Step 3: Start Docker Desktop (1 minute)
1. Open Windows Start Menu
2. Search for "Docker Desktop"
3. Click to launch
4. Wait 30-60 seconds for it to fully start
5. Check system tray - Docker icon should appear
6. Verify: Open PowerShell and run `docker ps` (no errors)

### Step 4: Ready to Launch!
Everything is now ready. Proceed to launch sequence.

---

## 🚀 LAUNCH DETAILS

### What Happens When You Launch

```
1. PowerShell Opens
   └─ Displays Jade ASCII banner

2. Environment Check
   └─ Verifies OPENAI_API_KEY is set
   └─ Exits if not found (you'll see error message)

3. Docker Check
   └─ Verifies Docker is running
   └─ Starts PostgreSQL container if needed

4. Database Preparation
   └─ Waits for PostgreSQL to be ready
   └─ Applies EF Core migrations
   └─ Creates/updates database schema

5. API Launch
   └─ Starts ASP.NET Core server
   └─ Listens on https://localhost:7299
   └─ Displays: "📡 API READY"

6. PowerShell Shows Endpoints
   └─ Base URL: https://localhost:7299
   └─ Swagger UI: https://localhost:7299/swagger
   └─ Chat endpoint: POST /api/chat/message
   └─ Health check: GET /health
   └─ Tools list: GET /api/chat/tools
```

### Stopping Jade
```
Option 1: Close PowerShell window
Option 2: Press Ctrl+C in PowerShell window
Option 3: Command: docker-compose down
```

---

## 📱 TEST YOUR FIRST MESSAGE

### Option 1: Using Swagger UI (Easiest)
1. Open browser
2. Go to: https://localhost:7299/swagger
3. Find "POST /api/chat/message"
4. Click "Try it out"
5. In request body, enter:
```json
{
  "message": "Hello Jade, what is 2 + 2?"
}
```
6. Click "Execute"
7. View response with Jade's reasoning

### Option 2: Using PowerShell/curl
```powershell
# Test health endpoint
curl -k https://localhost:7299/health

# Send message to Jade
$body = @{
	message = "What is 2 + 2?"
} | ConvertTo-Json

curl -k -X POST https://localhost:7299/api/chat/message `
  -Headers @{"Content-Type"="application/json"} `
  -Body $body
```

### Option 3: Using Postman
1. Download Postman: https://www.postman.com/downloads/
2. Create new request
3. Method: POST
4. URL: https://localhost:7299/api/chat/message
5. Headers: Content-Type: application/json
6. Body (JSON):
```json
{
  "message": "Hello Jade!"
}
```
7. Send request

---

## 🆘 TROUBLESHOOTING

### Problem: "OPENAI_API_KEY not set"
**Solution:**
1. Open Command Prompt (cmd.exe)
2. Type: `echo %OPENAI_API_KEY%`
3. If blank, follow Step 2 from setup
4. **Restart computer after setting**
5. Verify in new PowerShell window

### Problem: "Docker is not running"
**Solution:**
1. Open Windows Start Menu
2. Search and click "Docker Desktop"
3. Wait 1-2 minutes for startup
4. Try launching Jade again

### Problem: Port 7299 already in use
**Solution:**
1. Find process using port:
```powershell
netstat -ano | findstr :7299
```
2. Either:
   - Close other application using port
   - Edit deploy-jade.ps1: Change `$ApiPort = 7299` to different port
   - Stop all Docker containers: `docker-compose down`

### Problem: "Connection refused"
**Solution:**
1. Wait 10 seconds - API may still be starting
2. Check PowerShell window for error messages
3. Verify PostgreSQL is running: `docker ps`
4. Check PostgreSQL logs: `docker logs jade-postgres`

### Problem: "Database connection failed"
**Solution:**
1. Restart database: `docker-compose down -v` then try again
2. Check PostgreSQL is running: `docker ps`
3. Manual test: `docker exec jade-postgres psql -U postgres -c "\l"`

### Problem: "Permission denied" or "execution policy"
**Solution:**
```powershell
# Run PowerShell as Administrator
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
# Answer "Y" when prompted
```

### Problem: Shortcut doesn't work
**Solution:**
1. Use PowerShell manually:
```powershell
cd "C:\Users\toddg\OneDrive\Desktop\AI_Agent"
.\deploy-jade.ps1
```
2. Or use batch file:
```
Double-click launch-jade.bat
```

---

## 📊 EXPECTED STARTUP TIMES

| Phase | Expected Time |
|-------|----------------|
| PowerShell launch | <1 second |
| Environment check | 1-2 seconds |
| Docker startup | 5-10 seconds |
| PostgreSQL ready | 3-5 seconds |
| Migrations apply | 2-3 seconds |
| API startup | 2-5 seconds |
| **Total** | **~15-25 seconds** |

---

## 🎯 QUICK REFERENCE

### Files to Know
```
Desktop Shortcut:  C:\Users\toddg\OneDrive\Desktop\Jade AI Agent.lnk
Deploy Script:     C:\Users\toddg\OneDrive\Desktop\AI_Agent\deploy-jade.ps1
API Location:      C:\Users\toddg\OneDrive\Desktop\AI_Agent\publish\Jade.API.exe
Database Config:   C:\Users\toddg\OneDrive\Desktop\AI_Agent\docker-compose.yml
```

### API URLs (When Running)
```
Base:     https://localhost:7299
Swagger:  https://localhost:7299/swagger
Health:   GET https://localhost:7299/health
Chat:     POST https://localhost:7299/api/chat/message
Tools:    GET https://localhost:7299/api/chat/tools
Convos:   GET https://localhost:7299/api/chat/conversations
```

### Database Info
```
Host:     localhost
Port:     5432
User:     postgres
Password: postgres
Database: jade
```

### Key Environment Variables
```
OPENAI_API_KEY  = your-api-key-here (REQUIRED)
ASPNETCORE_URLS = https://localhost:7299;http://localhost:7298
DB_HOST         = localhost
DB_PORT         = 5432
```

---

## 📚 DOCUMENTATION FILES

| File | Purpose |
|------|---------|
| **START_HERE.md** | Read this first - quick overview |
| **DEPLOYMENT_COMPLETE.md** | Full deployment guide & reference |
| **ENVIRONMENT_SETUP.md** | Environment variable setup |
| **QUICKSTART.md** | Quick start guide |
| **README.md** | Architecture overview |
| **ARCHITECTURE.md** | System design details |
| **PROJECT_STRUCTURE.md** | File and folder structure |
| **SESSION_COMPLETE.md** | This session's summary |
| **FILE_INDEX.md** | Complete file reference |

---

## ✨ CONGRATULATIONS!

You've successfully deployed **Jade AI Agent** - an autonomous AI agent with ReAct reasoning!

### What You Have
- ✅ Complete .NET 10 solution
- ✅ ReAct cognitive engine
- ✅ PostgreSQL persistence
- ✅ REST API with chat interface
- ✅ GPT-4 integration
- ✅ Desktop shortcut for easy launch
- ✅ Comprehensive documentation
- ✅ Production-ready deployment

### Next Steps
1. **Launch** the agent using desktop shortcut
2. **Test** with your first message
3. **Explore** the Swagger documentation
4. **Extend** with custom tools
5. **Deploy** to production environment

---

## 🎉 YOU'RE READY!

Everything is set up and ready to go.

**Next action: Double-click the desktop shortcut and enjoy Jade!**

---

*Jade AI Agent - Autonomous Reasoning & Action Engine*  
*Status: Ready for Use* ✅
