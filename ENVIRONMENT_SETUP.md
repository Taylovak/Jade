# 🔑 JADE - Environment Setup Guide

**This guide helps you set up your system for Jade AI Agent**

---

## 🎯 What You Need

Before launching Jade, you need:

1. ✅ OpenAI API Key (for AI reasoning)
2. ✅ Docker Desktop (for PostgreSQL database)
3. ✅ Desktop shortcuts (for easy access)

---

## 📋 Step-by-Step Setup

### Step 1: Get OpenAI API Key

**Time**: 2 minutes

1. Go to: https://platform.openai.com/account/api-keys
2. Click "Create new secret key"
3. Copy the key (starts with `sk-`)
4. **Save it somewhere safe** - you'll need it in Step 2

---

### Step 2: Set Environment Variable (Windows)

**Time**: 5 minutes

#### Method A: Using GUI (Recommended)

1. Press **Windows Key + Pause** (or right-click "This PC" → Properties)
2. Click **"Advanced system settings"** on the left
3. Click **"Environment Variables"** button
4. Under "User variables", click **"New..."**
5. Variable name: `OPENAI_API_KEY`
6. Variable value: `sk-your-actual-key-here` (paste from Step 1)
7. Click **OK** three times
8. **Restart your computer** for changes to take effect

#### Method B: Using PowerShell

1. Open PowerShell as Administrator
2. Run:
```powershell
[Environment]::SetEnvironmentVariable("OPENAI_API_KEY", "sk-your-actual-key-here", "User")
```
3. Restart your computer

#### Method C: Quick Test (Temporary)

In any PowerShell window:
```powershell
$env:OPENAI_API_KEY = "sk-your-actual-key-here"
```
**Note**: This only works for the current PowerShell session

---

### Step 3: Verify Docker Desktop

**Time**: 2 minutes

1. Download & install: https://www.docker.com/products/docker-desktop
2. Start Docker Desktop
3. Verify it's running:
```powershell
docker --version
docker ps
```

---

### Step 4: Create Desktop Shortcuts

**Time**: 2 minutes

1. Open PowerShell
2. Run:
```powershell
cd "C:\Users\toddg\OneDrive\Desktop\AI_Agent"
.\create-shortcuts.ps1
```

3. Check your desktop - you should see:
   - 🤖 Launch Jade AI Agent
   - 📖 Jade Documentation
   - 📂 Jade Project Folder

---

## ✅ Verification Checklist

- [ ] OpenAI API key obtained
- [ ] `OPENAI_API_KEY` environment variable set
- [ ] Computer restarted
- [ ] Docker Desktop installed and running
- [ ] Desktop shortcuts created
- [ ] `docker ps` shows no errors

---

## 🚀 Ready to Launch!

Once all above steps are complete:

1. **Double-click**: 🤖 Launch Jade AI Agent
2. **Wait** for startup message
3. **Open browser** to: https://localhost:7299
4. **Send test message**: POST /api/chat/message

---

## 🆘 Troubleshooting

### "OPENAI_API_KEY not found"
- Did you restart your computer after setting environment variable?
- Verify it was set: Open cmd and run `echo %OPENAI_API_KEY%`

### "Docker is not running"
- Start Docker Desktop from Windows menu
- Wait 30 seconds for it to fully initialize
- Try again

### "Connection refused on port 7299"
- API might not have started
- Check PowerShell window for error messages
- Verify OpenAI key is set

### "Permission denied" errors
- Run PowerShell as Administrator
- Run: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`

---

## 📚 Next Steps

After verification:

1. **Read**: 📖 START_HERE.md
2. **Launch**: 🤖 Launch Jade AI Agent
3. **Test**: Send your first message
4. **Explore**: Read QUICKSTART.md
5. **Build**: Add custom tools

---

## 🎁 What Happens When You Launch

When you click "🤖 Launch Jade AI Agent":

1. PowerShell opens
2. Checks OpenAI API key ✅
3. Starts PostgreSQL database 🐳
4. Applies database migrations 📚
5. Starts Jade API server 🚀
6. Displays API endpoints

**You'll see**:
```
📡 API READY
━━━━━━━━━━━━━━━━━━━━━━━
🌐 Base URL:   https://localhost:7299
📝 API Docs:   https://localhost:7299/swagger
💬 Chat:       POST /api/chat/message
🔧 Tools:      GET /api/chat/tools
❤️  Health:     GET /health
```

---

## ⚡ Common Commands

### View Environment Variables
```powershell
# All variables
Get-ChildItem -Path Env:

# Just OpenAI key
$env:OPENAI_API_KEY
```

### Check Docker
```powershell
# Docker version
docker --version

# Running containers
docker ps

# PostgreSQL logs
docker logs jade-postgres
```

### Stop Jade
- Close the PowerShell window where it's running
- Or press Ctrl+C

---

## 🔐 Security Notes

- ✅ API key is set as environment variable (not in code)
- ✅ Database has default password (change in production)
- ✅ API runs on HTTPS (https://localhost:7299)
- ✅ Don't share your OpenAI API key!

---

## 📞 Need Help?

See documentation files:
- **Quick help**: START_HERE.md
- **Setup details**: QUICKSTART.md
- **Architecture**: README.md
- **All files**: FILE_INDEX.md

---

**Ready?** Create shortcuts and launch Jade! 🤖
