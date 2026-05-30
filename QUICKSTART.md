# JADE - Quick Setup Guide

## Prerequisites Checklist
- ✅ .NET 10 SDK installed
- ✅ Docker Desktop installed
- ✅ OpenAI API Key (https://platform.openai.com/account/api-keys)
- ✅ Visual Studio Community 2026

## 📋 Step-by-Step Setup

### Step 1: Set Your OpenAI API Key

**Option A - Windows PowerShell (Recommended)**
```powershell
$env:OPENAI_API_KEY = "sk-your-api-key-here"
```

**Option B - Permanently (Windows)**
```powershell
# Set environment variable globally
[Environment]::SetEnvironmentVariable("OPENAI_API_KEY", "sk-your-api-key-here", "User")
```

**Option C - appsettings.json**
Edit `src/Jade.API/appsettings.json` and add your key directly (not recommended for production).

### Step 2: Start PostgreSQL Database

```bash
cd "C:\Users\toddg\OneDrive\Desktop\AI_Agent"
docker-compose up -d
```

Verify it's running:
```bash
docker ps
```

### Step 3: Build the Solution

```bash
dotnet build Jade.slnx --configuration Release
```

### Step 4: Apply Database Migrations (First Time Only)

```bash
cd src/Jade.API
dotnet ef database update
```

### Step 5: Run Jade API

```bash
cd src/Jade.API
dotnet run
```

**Expected output:**
```
Building...
info: Microsoft.Hosting.Lifetime[14]
	  Now listening on: https://localhost:7299
	  Now listening on: http://localhost:5000
```

### Step 6: Test Jade is Running

```bash
curl https://localhost:7000/health --insecure
# Response: {"status":"Jade is ready!"}
```

## 🧪 Quick Test - Send First Message

### Using PowerShell/cURL

```bash
$body = @{
	message = "What is the capital of France?"
} | ConvertTo-Json

curl -X POST https://localhost:7299/api/chat/message `
  -H "Content-Type: application/json" `
  -d $body `
  --insecure
```

### Using VS Code REST Client

Create `test.http`:
```http
POST https://localhost:7299/api/chat/message HTTP/1.1
Content-Type: application/json

{
  "message": "What's 2 + 2?",
  "systemPrompt": "You are a helpful math tutor."
}
```

Right-click and select "Send Request"

### Using Postman

1. Create POST request to `https://localhost:7299/api/chat/message`
2. Headers: `Content-Type: application/json`
3. Body (JSON):
```json
{
  "message": "Tell me a joke",
  "conversationId": null,
  "systemPrompt": null
}
```

## 📊 Available API Endpoints

### Chat
- `POST /api/chat/message` - Send message to Jade
- `GET /api/chat/conversations/{id}` - Get conversation history
- `POST /api/chat/conversations?title=MyChat` - Create new conversation
- `GET /api/chat/conversations?pageSize=20&pageNumber=0` - List conversations
- `DELETE /api/chat/conversations/{id}` - Delete conversation

### Tools
- `GET /api/chat/tools` - List available tools

### Health
- `GET /health` - Check if Jade is running

## 🐳 Docker & Database Commands

### View Database Status
```bash
docker ps
```

### Access PostgreSQL via CLI
```bash
docker exec -it jade-postgres psql -U postgres -d jade_db
```

### View PgAdmin (optional)
Open browser: http://localhost:5050
- Email: admin@example.com
- Password: admin
- Add server connection to `jade-postgres:5432`

### Stop Everything
```bash
docker-compose down
```

### Stop & Remove All Data
```bash
docker-compose down -v
```

## 🛠️ Development Tips

### Run in Debug Mode
```bash
cd src/Jade.API
dotnet run --configuration Debug
```

### Run Tests
```bash
dotnet test Jade.slnx
```

### View Live Logs
```bash
docker-compose logs -f postgres
```

### Connection String for External Tools
```
Host=localhost;Database=jade_db;Username=postgres;Password=postgres;Port=5432
```

## 📱 Next: Build Mobile App

After API is working, build .NET MAUI app:
```bash
dotnet new maui -n JadeApp
cd JadeApp
dotnet build -f net10.0-ios  # for iOS
dotnet build -f net10.0-android  # for Android
```

## ❌ Troubleshooting

### "Connection refused" to PostgreSQL
- Verify Docker is running: `docker ps`
- Check PostgreSQL container: `docker logs jade-postgres`
- Restart: `docker-compose restart postgres`

### "OpenAI API Key not found"
- Verify environment variable: `$env:OPENAI_API_KEY`
- Or set in `appsettings.json`
- Restart the API after setting

### Port Already in Use (5432, 7299)
```bash
# Find process using port 7299
netstat -ano | findstr :7299
# Kill it (replace PID)
taskkill /PID <PID> /F
```

### Database Migration Errors
```bash
# Reset database
dotnet ef database drop --force
dotnet ef database update
```

## 📚 Project Structure

```
Jade/
├── src/
│   ├── Jade.Core/              🧠 ReAct Cognitive Engine
│   ├── Jade.Memory/            💾 PostgreSQL Storage
│   ├── Jade.Tools/             🔧 Tool Registry
│   ├── Jade.Models/            📦 Shared DTOs
│   └── Jade.API/               🌐 REST API
├── tests/
│   └── Jade.Tests/             ✅ Unit Tests
├── docker-compose.yml          🐳 PostgreSQL Setup
└── README.md
```

## 🚀 What's Next?

1. ✅ API running - send messages to Jade
2. ⬜ Integrate more tools (Web Search, Document Upload)
3. ⬜ Add authentication
4. ⬜ Build mobile app (MAUI)
5. ⬜ Deploy to Azure/AWS
6. ⬜ Add vector embeddings for semantic search

---

**Need Help?**
- Check `README.md` for architecture details
- Review `Program.cs` for dependency injection
- Inspect `ChatController.cs` for API logic

**Enjoy Jade! 🤖**
