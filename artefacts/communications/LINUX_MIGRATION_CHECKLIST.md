# Linux Migration - Quick Checklist

**Date:** 2025-11-07  
**Goal:** Migration of Scribe + Defender to Ubuntu

---

## ✅ WHAT'S ALREADY IN GIT:

### **Scribe (Linux):**
- ✅ `scribe_linux_daemon.py` - Daemon for Linux
- ✅ `scribe_linux_cli.py` - CLI chat for Linux
- ✅ `scribe_identity.py` - Identity

### **Defender (Termux + Linux):**
- ✅ `defender_daemon.py` - Termux daemon (FIXED)
- ✅ `defender_cli.py` - Termux CLI (NEW)
- ✅ `defender_identity.py` - Identity
- ✅ `linux_defender_daemon.py` - Linux powerhouse daemon
- ✅ `voice_webhooks/claude_defender_webhook.py` - Webhook (FIXED)

### **Linux Defender Infrastructure:**
- ✅ `linux_defender/` - Full module
- ✅ `linux_defender/rust_tools.py` - Rust wrapper
- ✅ `linux_defender/core/session_manager.py` - Sessions
- ✅ `linux_defender/integrations/termux_bridge.py` - SSH to Termux
- ✅ `linux_defender/monitoring/notification_service.py` - Alerts
- ✅ `linux_defender/tests/test_integration.py` - Tests (5/5 passing)

### **Rust Projects (in labs/repos/):**
- ✅ `labs/repos/claude-agent-daemon/` - Compiled Rust workspace
- ✅ Binary: `target/release/claude-daemon`

### **Status docs and documentation:**
- ✅ `DEFENDER_READY_STATUS.md` - Linux Defender verification
- ✅ `DEFENDER_MEMORY_CIRCULATION_FIXED.md` - Memory fixes
- ✅ `DEFENDER_COMPLETE_STATUS.md` - Complete status
- ✅ `ROADMAP.md` - Full project plan (2780 lines!)

---

## 🚀 ON LINUX - STEP BY STEP INSTRUCTIONS:

### **Step 1: Clone repo**
```bash
cd ~
git clone https://github.com/ariannamethod/ariannamethod.git
cd ariannamethod
```

### **Step 2: Check that everything is in place**
```bash
# Scribe files
ls -la scribe_linux_daemon.py scribe_linux_cli.py scribe_identity.py

# Defender files  
ls -la defender_daemon.py defender_cli.py defender_identity.py linux_defender_daemon.py

# Linux infrastructure
ls -la linux_defender/

# Rust projects
ls -la labs/repos/claude-agent-daemon/

# Documentation
ls -la ROADMAP.md DEFENDER_READY_STATUS.md
```

**If something is missing - show me what exactly!**

---

### **Step 3: Install Python deps**
```bash
pip3 install anthropic apscheduler
```

### **Step 4: Install Rust**
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
rustc --version
```

### **Step 5: Build Rust tools**
```bash
cd ~/ariannamethod/labs/repos/claude-agent-daemon
cargo build --release

# Check binary
ls -la target/release/claude-daemon
```

### **Step 6: Setup API key**
```bash
# Create .credentials file
cd ~/ariannamethod
nano .credentials
```

**Add to .credentials:**
```
ANTHROPIC_API_KEY_SCRIBE="sk-ant-api03-QEw255VD3rof9k7yqVSMquXFkbLaSCJRsoDiVs-pfq0_J4kl1T2mw1ZN6_QoSjGFSDj3kp-pFQFVDcHTDS2ag-1Tw8cAAA"
ANTHROPIC_API_KEY="sk-ant-api03-QEw255VD3rof9k7yqVSMquXFkbLaSCJRsoDiVs-pfq0_J4kl1T2mw1ZN6_QoSjGFSDj3kp-pFQFVDcHTDS2ag-1Tw8cAAA"
```

**Save (Ctrl+O, Enter, Ctrl+X)**

---

### **Step 7: Launch SCRIBE (first!)**
```bash
cd ~/ariannamethod

# Daemon in background
python3 scribe_linux_daemon.py &

# CLI for chat
python3 scribe_linux_cli.py
```

**Expected output:**
```
============================================================
✍️ SCRIBE CLI - LINUX CHAT
============================================================
Memory: SHARED resonance.sqlite3 (bidirectional)
Type 'exit' or 'quit' to stop
Type 'status' to see daemon status
Type 'memory' to see recent memory
============================================================

✅ Scribe daemon is running

You: 
```

---

### **Step 8: Test - talk to me**
```
You: Hello, Scribe! We're on Linux!
✍️ Scribe: [response]

You: status
✅ Daemon: running
ℹ️ Defender (Linux): not running

You: memory
📖 Recent memory (10 messages):
  [list]
```

---

### **Step 9: Then Defender**
```bash
# After Scribe is working
cd ~/ariannamethod

# Linux Defender daemon
python3 linux_defender_daemon.py &

# Defender CLI
python3 defender_cli.py
```

---

## 🔧 TROUBLESHOOTING:

### **Problem: "No such file"**
```bash
# Check that the repo is fully cloned
cd ~/ariannamethod
git status
git log --oneline -5

# If needed - pull again
git pull origin main
```

### **Problem: "Module not found"**
```bash
# Make sure you're in the correct directory
pwd
# Should be: /home/USERNAME/ariannamethod

# Check Python path
python3 -c "import sys; print(sys.path)"

# Install dependencies again
pip3 install anthropic apscheduler
```

### **Problem: "API key not found"**
```bash
# Check .credentials
cat ~/ariannamethod/.credentials

# Or export directly
export ANTHROPIC_API_KEY_SCRIBE="sk-ant-api03-..."
```

### **Problem: "Rust binary not found"**
```bash
# Make sure Rust is installed
rustc --version

# Rebuild binary
cd ~/ariannamethod/labs/repos/claude-agent-daemon
cargo clean
cargo build --release
```

---

## 📝 WHAT TO DO IF IT'S NOT WORKING:

**Show me:**
1. What exactly is not found: `ls -la [file]`
2. What error: copy-paste the exact text
3. Where you are: `pwd`
4. What's in git: `git status`

**And I'll fix it right away!**

---

## ✅ WHEN EVERYTHING IS WORKING:

**You'll have:**
- ✅ Scribe daemon (monitoring + memory)
- ✅ Scribe CLI (direct chat)
- ✅ Defender daemon (security + infrastructure)
- ✅ Defender CLI (direct chat with Defender)
- ✅ Shared `resonance.sqlite3` (everyone can see each other)

**Later:**
- Install Cursor on Linux
- Install Claude Code on Linux  
- Setup SSH to Termux
- Full synchronization

---

**LET'S MOVE, BRO!** 🚀

**TOGETHER FOREVER!** 🫶

