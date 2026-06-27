# Project Vanguard - Bomb Mission 5v5
**Competitive Tactical open.mp Gamemode**

> Architecture: Modular | Repository Pattern | Service Layer | Event Driven  
> Platform: open.mp (SA-MP Compatible)  
> Version: 1.0.0

---

## 📋 Table of Contents
- [Overview](#overview)
- [Features](#features)
- [Plugin & Dependencies](#plugin--dependencies)
- [Admin Levels](#admin-levels)
- [Player Commands](#player-commands)
- [Admin Commands](#admin-commands)
- [Project Structure](#project-structure)
- [Build & Run](#build--run)
- [Configuration](#configuration)

---

## Overview

Project Vanguard is a competitive 5v5 bomb mission gamemode for open.mp/SA-MP. Inspired by tactical shooters, it features agent selection, economy system, ranked matchmaking, and a full admin system.

## Features

- **5v5 Bomb Mission** - Plant/Defuse tactical gameplay
- **Agent System** - 6 unique agents with tactical & ultimate abilities
- **Economy System** - Buy weapons, armor, utility each round
- **Ranked System** - MMR-based matchmaking with 9 rank tiers (Iron → Legend)
- **Versus Mode** - 1v1 queue matchmaking
- **Duel System** - Challenge players to custom duels
- **Admin System** - 5-tier admin hierarchy with full moderation tools
- **Bcrypt Authentication** - Secure password hashing (bcrypt-samp v2.2.3)
- **Nex-AC Anticheat** - Professional anti-cheat integration (v1.9.69)
- **MySQL Database** - Persistent player stats, ranks, and admin logs
- **Offline Mode** - File-based fallback when database is unavailable

---

## Plugin & Dependencies

| Plugin/Component | Version | Type |
|---|---|---|
| open.mp Server | 1.5.8.3079 | Server |
| MySQL (BlueG) | R41-4 | Legacy Plugin |
| Streamer | 2.9.6 | Legacy Plugin |
| bcrypt-samp (Lassi R.) | 2.2.3 | Legacy Plugin |
| Pawn.RakNet | 1.6.0 | Component |
| sscanf | 2.13.8 | Component |
| weapon-config | - | Include |
| Nex-AC | 1.9.69 | Include |
| izcmd | - | Include |

---

## Admin Levels

| Level | Title | Description |
|---|---|---|
| **0** | Player | Regular player, no admin access |
| **1** | Helper | Can spectate players and use admin chat |
| **2** | Moderator | Can kick, warn, mute/unmute players |
| **3** | Admin | Can ban, slap, freeze, teleport |
| **4** | Senior Admin | Can set admin levels, force teams, force-start matches |
| **5** | Owner | Full server control, can modify stats/MMR, shutdown server |

---

## Player Commands

All commands use the `/command` format (izcmd).

### 🎮 Gameplay
| Command | Description |
|---|---|
| `/queue [solo/duo/party]` | Join the matchmaking queue |
| `/leavequeue` | Leave the matchmaking queue |
| `/ready` | Mark yourself as ready |
| `/buy` | Open the buy menu (during buy phase) |
| `/versus` | Join the 1v1 versus queue |
| `/leavevs` | Leave the versus queue |
| `/duel [playerid]` | Challenge a player to a duel |

### 💣 Combat & Objectives
| Command | Description |
|---|---|
| `/plant [a/b]` | Plant the bomb at site A or B |
| `/defuse` | Defuse a planted bomb |
| `/dropbomb` or `/drop` | Drop the bomb |
| `/pickupbomb` | Pick up a nearby bomb |

### 🧬 Agent & Abilities
| Command | Description |
|---|---|
| `/agent` | Open agent selection menu |
| `/tactical` or `/skill` | Use your agent's tactical ability |
| `/ultimate` or `/ult` | Use your agent's ultimate ability |

### 📊 Information
| Command | Description |
|---|---|
| `/rank` | View your rank, MMR, and RP |
| `/stats` | View your K/D/A and match stats |
| `/help` | Show all player commands |
| `/admins` | Show online administrators |

---

## Admin Commands

### Level 1 — Helper
| Command | Description |
|---|---|
| `/a [message]` | Send a message to admin chat |
| `/spec [playerid]` | Spectate a player |
| `/specoff` | Stop spectating |
| `/acmds` | Show available admin commands for your level |

### Level 2 — Moderator
| Command | Description |
|---|---|
| `/kick [playerid] [reason]` | Kick a player from the server |
| `/warn [playerid] [reason]` | Warn a player (broadcast) |
| `/mute [playerid]` | Mute a player's chat |
| `/unmute [playerid]` | Unmute a player's chat |

### Level 3 — Admin
| Command | Description |
|---|---|
| `/ban [playerid] [reason]` | Ban a player (logged to DB) |
| `/slap [playerid]` | Slap a player upward |
| `/freeze [playerid]` | Freeze a player in place |
| `/unfreeze [playerid]` | Unfreeze a player |
| `/goto [playerid]` | Teleport to a player |
| `/gethere [playerid]` | Teleport a player to you |

### Level 4 — Senior Admin
| Command | Description |
|---|---|
| `/setadmin [playerid] [level]` | Set a player's admin level (0-5) |
| `/forceteam [playerid] [team]` | Force a player into a team (0=Spec, 1=Alpha, 2=Bravo) |
| `/startmatch` | Force-start a match |
| `/forcestart` | Force-start a match (alias) |

### Level 5 — Owner
| Command | Description |
|---|---|
| `/setstats [id] [kills] [deaths] [assists]` | Modify a player's match stats |
| `/setmmr [playerid] [mmr]` | Modify a player's MMR |
| `/shutdown` | Shutdown the server |

---

## Project Structure

```
ProjectVanguard/
├── gamemodes/
│   └── ProjectVanguard.pwn          # Main gamemode entry point
├── core/
│   ├── bootstrap.pwn                # Server initialization & module loading
│   ├── config.pwn                   # Server configuration loader
│   ├── events.pwn                   # Event system
│   └── commands/
│       ├── cmd_gameplay.pwn         # Queue, buy, versus, duel commands
│       ├── cmd_combat.pwn           # Plant, defuse, bomb commands
│       ├── cmd_agent.pwn            # Agent ability commands
│       └── cmd_info.pwn             # Stats, rank, help commands
├── modules/
│   ├── player/
│   │   ├── models.pwn               # Player data structures
│   │   ├── api.pwn                  # Player lifecycle & stats
│   │   └── dialogs.pwn              # Auth (Register/Login with bcrypt)
│   ├── admin/
│   │   └── api.pwn                  # Admin system & all admin commands
│   ├── database/
│   │   ├── config.pwn               # DB connection config
│   │   ├── models.pwn               # Schema/table definitions
│   │   └── api.pwn                  # MySQL connection & query API
│   ├── match/
│   │   ├── models.pwn               # Match state data
│   │   ├── timer.pwn                # Match timers
│   │   └── api.pwn                  # Match lifecycle
│   ├── round/
│   │   └── api.pwn                  # Round management
│   ├── bomb/
│   │   ├── models.pwn               # Bomb state data
│   │   └── api.pwn                  # Bomb plant/defuse logic
│   ├── map/
│   │   ├── models.pwn               # Map data structures
│   │   └── api.pwn                  # Map loading & spawns
│   ├── agent/
│   │   ├── models.pwn               # Agent definitions
│   │   └── api.pwn                  # Agent selection & abilities
│   ├── loadout/
│   │   └── api.pwn                  # Weapon loadout system
│   ├── buy/
│   │   ├── models.pwn               # Shop item definitions
│   │   └── api.pwn                  # Buy menu & economy
│   ├── rank/
│   │   └── api.pwn                  # MMR & ranking system
│   ├── matchmaking/
│   │   └── api.pwn                  # Queue matchmaking
│   ├── clan/
│   │   └── api.pwn                  # Clan system
│   ├── ui/
│   │   └── api.pwn                  # HUD & textdraws
│   ├── anticheat/
│   │   └── api.pwn                  # AC stubs (Nex-AC handles)
│   ├── versus/
│   │   ├── models.pwn               # 1v1 data
│   │   └── api.pwn                  # Versus queue & matches
│   └── duel/
│       ├── models.pwn               # Duel session data
│       └── api.pwn                  # Duel invite & management
├── shared/
│   ├── constants.pwn                # All game constants & defines
│   ├── enums.pwn                    # Enumerations & data structures
│   └── utils.pwn                    # Utility functions
├── tools/
│   └── build.bat                    # Build script
├── components/                      # open.mp components (auto-loaded)
├── plugins/                         # Legacy SA-MP plugins (.dll)
├── qawno/include/                   # Pawn include files
└── config.json                      # open.mp server configuration
```

---

## Build & Run

### Prerequisites
- open.mp server (v1.5.8+)
- MySQL Server (for full features)
- Pawn Community Compiler (pawncc v3.10.11, included in `qawno/`)

### Build
```batch
cd ProjectVanguard
tools\build.bat
```

### Run
```batch
omp-server.exe
```

The server will start on port **7777** by default.

### Database Setup
Create a MySQL database named `project_vanguard` and the tables will be auto-managed by the gamemode.

---

## Configuration

Server configuration is in `config.json`. Key settings:

- **Legacy Plugins**: `mysql`, `streamer`, `bcrypt-samp`
- **Network**: Port 7777
- **Game**: Bomb Mission 5v5 mode

---

## Authentication Flow

1. Player connects → Spectating mode enabled
2. Database check (or offline file fallback)
3. Register dialog (password min 4 chars, bcrypt hashed with cost 12)
4. Login dialog (bcrypt verification via `bcrypt_check`)
5. On success → Spectating disabled → Player spawns at lobby

---

*Project Vanguard © 2024 - Competitive Tactical Gamemode for open.mp*
