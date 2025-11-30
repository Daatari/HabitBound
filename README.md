# HabitBound – A Habitica-Powered Game Time Manager

HabitBound is a desktop companion app that converts Habitica productivity into
real-world gaming time. You “buy” game minutes with Habitica gold, and the
launcher enforces those minutes with a built-in game timer.

This project aims to bridge **habit-building** and **game self-regulation** using
the Habitica API and an optional Playnite/Steam integration.

---

## 🧠 Concept

**Productivity → Gold → Game Time → Enforced Play Sessions**

- You earn **gold** in Habitica by completing tasks.
- You spend that gold to purchase **game minutes**.
- HabitBound launches your games and enforces the time limit with an overlay or
  background timer.
- When time is up, HabitBound warns you — and optionally closes the game.

Core idea: **turn gaming into a reward loop powered by your real-life habits.**

---

## ✨ Features (Planned)

### **MVP**
- Connect to Habitica using User ID + API Token
- Read current gold balance
- Customizable “exchange rate” (e.g., 10 gold = 30 minutes)
- Purchase Habitica rewards programmatically (spend gold)
- Timer-based launcher for any PC game (EXE path)
- Always-on-top timer overlay with remaining time
- Optional auto-close game when time expires
- Session log saved locally (JSON/CSV)

### **Phase 2**
- Playnite integration: hook into `OnGameStarting`
- Steam “wrapper” profiles for controlled launching
- Bank time ahead of sessions (via reward purchases)
- UI themes (dark/light)

### **Phase 3**
- Weekly analytics (gold earned → hours played)
- Habit streak integration (bonus minutes)
- Friends party boost (if party completes dailies)
- Full cross-platform builds (Windows/Linux/macOS via Tauri)

---

## 🏗 Architecture Overview

### **1. Habitica API layer**
Handles:
- Authentication
- Retrieving user gold
- Purchasing reward tasks
- Reading reward task history (optional)

Habitica endpoints used:
- `GET /user`
- `POST /tasks/:id/score` (to “purchase” a reward)
- Potentially `GET /tasks/user` to track reward purchases

---

### **2. Time Conversion Logic**

Configurable formula:
