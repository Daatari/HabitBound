# HabitBound

HabitBound is a desktop companion app that connects to your Habitica account and lets you **spend in-game rewards (gold, level, HP) to unlock real-world game time**.

You earn progress in Habitica → that progress turns into gaming minutes → HabitBound enforces the timer so you don’t overplay.

This transforms gaming into a **reward loop**, not a distraction.

---

#  Core Concept

> “Complete real habits and tasks in Habitica.  
> Earn gold, XP, and levels.  
> Spend that progress to buy real-world gaming time.  
> HabitBound enforces your session and keeps you honest.”

HabitBound does **not** modify Habitica’s game code.  
It uses Habitica’s official API to:

- Read your gold, XP, level, and HP  
- Spend gold via reward tasks  
- Apply HP penalties for snoozing  
- Scale your daily allowance based on your level  

---

#  Features

##  MVP (v1.0)

###  Habitica Integration
- Connect using Habitica User ID + API Token  
- Read stats: **gold, level, XP, HP**  
- Spend gold via Habitica reward tasks  
- Validate credentials using `/api/v3/user`

###  Gold → Minutes Conversion
Define your own conversion rate, e.g.:

- **10 gold → 30 minutes**  
- **1 gold → 3 minutes**  
- or any custom ratio  

Before starting a session:
1. HabitBound checks how much gold you have  
2. You choose how much to spend  
3. HabitBound converts that gold into gaming minutes  

###  Game Session Enforcement
HabitBound:
- Launches your game executable  
- Tracks the running process  
- Shows time remaining  
- Enforces the timer when minutes run out  

At timeout:
- **Strict mode:** game closes immediately  
- **Soft mode:** you may snooze (see below)

---

#  Nice-to-Have / v2+ Features

##  Level-Based Daily Allowance Scaling
Your Habitica level increases your **free daily gaming allowance**, even before spending gold.

Example defaults:

| Level | Free Daily Minutes |
|-------|---------------------|
| 1–10  | 20–45 min           |
| 11–20 | 45–60 min           |
| 21–50 | 60–120 min          |
| 50+   | Prestige scaling    |

You get rewarded for long-term habit building by unlocking more natural playtime.

---

##  Snooze Feature (Costs Habitica HP)
When time expires, you can **snooze** instead of quitting — but it costs HP.

Example:
- Snooze 5 minutes → **–5 HP**
- Snooze 10 minutes → **–10 HP**

Additional options:
- Cost increases with each consecutive snooze  
- Cost scales with level  
- HP never drops below 1 unless user opts into “Hardcore Mode”  

This reproduces the “risk” feeling of Habitica — snoozing is possible, but not free.

---

## 🎛️ Per-Game Profiles
Configure each game independently:
- Gold → minute conversion
- Daily caps
- Weekday vs weekend rules
- Strict/soft mode overrides
- Blacklisted or cooldown-required games

---

##  Banked Time Mode
Alternative model:
- You buy “+30 minutes game time” as a Habitica reward  
- HabitBound detects purchases and converts them into a time bank  
- Sessions deduct from banked time instead of live gold

---

##  Logs & Analytics
Track:
- Game played  
- Minutes purchased  
- Actual time spent  
- Gold spent  
- Snoozes used  
- HP lost  
- Whether a session ended willingly or via force-quit  

Analytics:
- Daily/weekly gaming totals  
- Snooze trends  
- Productivity vs gaming ratios  

---

#  How It Works

## 1. Authentication
User enters:
- Habitica User ID  
- Habitica API Token  

HabitBound sends:
GET /api/v3/user

yaml
Copy code
If valid → load stats and enable the dashboard.

---

## 2. Determine Allowance
**Total allowed time = free (level-scaled) minutes + purchased minutes**

Example:
- Base (level 18): 60 minutes  
- User spends 20 gold  
- Conversion: 10 gold → 30 minutes  
- Purchased = 60 minutes  

**Result = 120 minutes allowed today**

---

## 3. Game Launch
HabitBound:
- Launches the selected game  
- Begins countdown  
- Shows time remaining in app or system tray  

---

## 4. Timeout Logic
At zero minutes left:
Your session has ended.
Quit now or snooze?

yaml
Copy code

Options:
- Quit now → game closes  
- Snooze → lose HP, extend timer  
- In strict mode → snooze option removed  

---

#  Technical Stack

###  UI
- **React + TypeScript**  
- TailwindCSS or similar  
- Desktop wrapper via **Tauri** (preferred) or Electron  

### ⚙️ Backend Logic (inside app)
- Habitica API client  
- Session timer engine  
- Process management (start, monitor, close EXEs)  
- Local JSON storage for config and logs  
- Level-scaling calculator  
- HP penalty system  

### 🔌 Optional Integrations
- Playnite extension  
- Steam wrapper EXE mode  
- System tray mini-UI  

---

#  Example Configuration (`config.json`)

```json
{
  "habitica": {
    "userId": "YOUR_ID",
    "apiToken": "YOUR_TOKEN",
    "clientLabel": "habitbound-app"
  },
  "economy": {
    "goldPer30Minutes": 10,
    "levelScalingEnabled": true,
    "strictMode": false
  },
  "games": [
    {
      "id": "fnv",
      "name": "Fallout: New Vegas",
      "exePath": "C:\\Games\\FNV\\fnv.exe"
    }
  ]
}
```
🛣️ Roadmap
v0.1 — Planning & Documentation
Finalize API boundaries

Define conversion models

Write UI flow and diagrams

Add docs

v0.2 — CLI Prototype
Habitica API test

Gold → minutes conversion

Simple countdown

Manual EXE launch

v0.3 — Core Session Manager
Game launching

Process monitoring

Timeout enforcement

HP snooze penalties

v0.4 — Desktop UI Prototype
Connect Habitica screen

Dashboard with games

Session start UI

Timer overlay or tray icon

v1.0 — First Fully Usable Release
Complete UI

Level-scaling

Snooze penalties

Session logs

Installers

v2.0 — Expanded Features
Playnite support

Steam wrapper EXE

Banked-time model

Multi-platform support

 License
MIT License

 Contributing
Pull requests and issue discussions are welcome.

 Author

Danny Atari
Toronto, Canada
Danny Atari
Toronto, Canada
