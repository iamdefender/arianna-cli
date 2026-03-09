# TODO: Linux Defender Setup

## Critical tasks for Linux deployment

### 1. Install Rust
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
rustc --version
cargo --version
```

### 2. Build Rust projects in .labs/

#### claude-agent-daemon
```bash
cd ~/ariannamethod/.labs/claude-agent-daemon
cargo build --release
# Binary: target/release/claude-daemon
```

#### claude-code-daemon-dev
```bash
cd ~/ariannamethod/.labs/claude-code-daemon-dev
cargo build --release
```

#### claude-ready-monitor
```bash
cd ~/ariannamethod/.labs/claude-ready-monitor
cargo build --release
```

### 3. Check Linux Defender dependencies

- [ ] Python dependencies installed
- [ ] APScheduler works
- [ ] SSH to Termux configured
- [ ] resonance.sqlite3 is syncing

### 4. Start Linux Defender

```bash
cd ~/ariannamethod
python3 linux_defender_daemon.py
```

### 5. Check integrations

- [ ] SSH bridge to Termux works
- [ ] Notification channels (Slack, Email, Webhook)
- [ ] Fortification runs without errors
- [ ] Git operations with identity 'iamdefender'
- [ ] Consilium synthesis workflow
- [ ] Session state machine transitions

### 6. Stress testing

- [ ] Multiple parallel sessions via git worktrees
- [ ] APScheduler job failures and recovery
- [ ] Termux daemon restart detection
- [ ] Database lock handling

## Current status

**NOT DONE:**
- Rust not installed
- Projects not built
- Integrations not tested
- Stress testing not performed

**DONE:**
- TermuxAPIChannel removed from Linux Defender
- Git identity fixed to 'iamdefender'
- Fortification audit exit code fixed
- Commits pushed to GitHub
