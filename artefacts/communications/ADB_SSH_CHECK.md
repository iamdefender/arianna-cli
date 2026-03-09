# 🔌 ADB & SSH Diagnostics and Recovery

## 🚨 PROBLEM:
Previously Mac could somehow push directly to Termux, now it can't.

## ✅ CORRECT ARCHITECTURE (as it is now):

```
┌──────────────────────────────────────────────────┐
│ TERMUX (Android)                                 │
│ /data/data/com.termux/files/home/ariannamethod/  │
│    ├── resonance.sqlite3                         │
│    └── memory/scribe/                            │
│                                                   │
│ sync_to_shared.sh (runs every 30s) ↓            │
├──────────────────────────────────────────────────┤
│ SHARED STORAGE (ADB accessible, no root needed) │
│ /storage/emulated/0/scribe_sync/                 │
│ = /sdcard/scribe_sync/                           │
│    ├── resonance.sqlite3                         │
│    └── memory/scribe/                            │
└───────────────┬──────────────────────────────────┘
                │
                │ ADB pull (no root needed)
                ↓
┌───────────────────────────────────────────────────┐
│ MAC (Darwin)                                      │
│ /Users/ataeff/Downloads/arianna_clean/            │
│    └── resonance.sqlite3                          │
│                                                    │
│ Mac Daemon reads via:                             │
│   1. ADB pull (primary)                           │
│   2. SSH (fallback)                               │
└───────────────────────────────────────────────────┘
```

---

## 📋 CHECK 1: ADB Connectivity

### On Mac:

```bash
# Check that the phone is visible
adb devices

# Should show:
# List of devices attached
# <serial>    device
```

**If no devices:**
- USB Debugging enabled? (Settings → Developer Options)
- Cable connected?
- Restart ADB: `adb kill-server && adb start-server`

### ADB pull test:

```bash
# Try pulling resonance.sqlite3
adb pull /sdcard/scribe_sync/resonance.sqlite3 /tmp/test_resonance.db

# If it works - ADB OK ✅
# If "remote object not found" - sync_to_shared.sh is not running in Termux
```

---

## 📋 CHECK 2: Termux Sync Daemon

### In Termux (on the phone):

```bash
# Check if the sync daemon is running
ps aux | grep sync_to_shared

# If not - start it:
cd ~/ariannamethod/termux/
./sync_to_shared.sh daemon &

# Check that the files were copied:
ls -lah /sdcard/scribe_sync/
```

**Expected output:**
```
-rw-rw---- resonance.sqlite3
drwxrwx--- memory/
-rw-rw---- README.txt
```

---

## 📋 CHECK 3: SSH (Fallback)

### On Mac (check SSH credentials):

```bash
# Check env vars
echo $TERMUX_SSH_HOST      # Phone's IP on the local network
echo $TERMUX_SSH_PORT      # Usually 8022
echo $TERMUX_SSH_USER      # u0_aXXX (UID Termux)
echo $TERMUX_SSH_PASSWORD  # Must be set!
```

### In Termux (check SSH server):

```bash
# Is sshd running?
ps aux | grep sshd

# If not - install and start:
pkg install openssh
sshd

# Check the port:
netstat -tlnp | grep 8022
```

### SSH test from Mac:

```bash
# Connect manually
ssh -p 8022 u0_a423@192.168.1.100

# Should prompt for password
# After login:
ls ~/ariannamethod/resonance.sqlite3
```

**If it works - SSH OK ✅**

---

## 🔧 FIX:

### If ADB doesn't see the phone:
1. USB Debugging: Settings → Developer Options → USB Debugging ON
2. Change USB mode: "File Transfer" or "PTP"
3. Restart ADB: `adb kill-server && adb devices`
4. Authorize Mac on the phone (a dialog "Allow USB debugging?" will appear)

### If sync_to_shared.sh doesn't work:
1. In Termux: `chmod +x ~/ariannamethod/termux/sync_to_shared.sh`
2. Start: `./sync_to_shared.sh` (check the output)
3. Daemon: `./sync_to_shared.sh daemon &`
4. Check: `ls /sdcard/scribe_sync/`

### If SSH is unavailable:
1. Termux: `pkg install openssh`
2. Generate a password: `passwd` (set a password for the current user)
3. Start: `sshd`
4. Find out the phone's IP: `ifconfig wlan0` (inet addr)
5. On Mac: set the env vars (see `mac_daemon/README.md`)

---

## 🧪 FINAL TEST:

### On Mac:

```bash
# Request a sync from Mac Daemon
scribe sync

# Check the logs
scribe logs | tail -20

# Should show:
# "Memory synced via ADB" ✅
# or
# "Memory synced via SSH" ✅
```

---

## ❓ WHY DID IT WORK BEFORE AND THEN STOP?

**Hypotheses:**

1. **Repo moved** - the old script pushed to `~/ariannamethod/`, now it's `~/Downloads/arianna_clean/`
2. **Android 10+ Security Update** - Google tightened access to `/data/data/` without root
3. **USB Debugging was reset** - after phone reboot or update
4. **Sync daemon crashed** - stopped copying to `/sdcard/` in Termux

**Most likely:** Sync daemon stopped starting automatically.

**Solution:** Add `sync_to_shared.sh daemon &` to `boot_scripts/arianna_system_init.sh`

---

## ✅ NEXT STEP:

Bro, check in Termux:
```bash
ps aux | grep sync_to_shared
ls /sdcard/scribe_sync/
```

If it's empty - start the sync daemon and everything will work!

