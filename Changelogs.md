# Changelog

## Beta v2.5 (19.02.2026)

### Captcha System Reworked
- **CapSolver removed, Anti-Captcha added**
  - Anti-Captcha (https://anti-captcha.com) replaces CapSolver as the second captcha service option
  - API endpoints: `createTask`, `getTaskResult`, `getBalance`, `reportIncorrectImageCaptcha`
  - Config menu: `[D]` Select service > 2Captcha / Anti-Captcha / Manual
  - Config menu: `[K]` Set Anti-Captcha API key (was previously CapSolver)
  - Balance check `[G]` now shows 2Captcha + Anti-Captcha balance
  - Migration: Old `capsolver_api_key` entries are automatically removed

- **Report Feedback for both services (New)**
  - **2Captcha:** `reportCorrect` + `reportIncorrect` — Reports back to the service whether the solution was correct or incorrect
  - **Anti-Captcha:** `reportIncorrectImageCaptcha` — Reports incorrect solutions for possible refund (within 60 seconds)
  - Reports are sent automatically when PokeMeow responds with "thank you" (correct) or "incorrect" (wrong)
  - Improves solution quality over time, as the services use the reports to optimize their workers

- **Captcha numbers optimized**
  - `minLength` adjusted: 3 (was 3)
  - `maxLength` adjusted: 6 (was 8) — PokeMeow captchas only have 3-6 digits
  - `numeric: 1` — Numbers only 1-9 (no letters)
  - More accurate description in the `comment` field for better recognition rate

### Changes
- Version bumped to 2.5
- Config migration: `capsolver_api_key` is automatically removed, service reset to "Manual" if CapSolver was previously active

---

## Beta v2.4 (19.02.2026)

### New Features
- **Start menu reworked**
  - `[1] Start` — Starts the bot directly (hunting + auto-catch only)
  - `[2] Start + Daily Tasks` — Runs all dailies first (;daily, ;h, ;swap, ;q), then hunting
  - Daily tasks are no longer individually toggleable in the config — instead you decide at startup
  - Fewer clicks, faster workflow

- **Config menu simplified**
  - Daily task toggles ([1]-[4]) removed — now controlled via start menu
  - Auto-Catch is now `[1]`, Ball Rules `[2]`, Fish `[3]`
  - Cleaner and fewer options

### Proxy Warning (important!)
- **Only use a proxy if browser + bot use the same proxy!**
  - If the bot runs through a proxy in Tokyo, but the browser (where you manually use the account) uses your real IP in Germany, Discord sees IP jumps between continents — this is a ban risk!
  - Every device and browser using the same account must use the same proxy
  - Exception: Residential proxies from your own country are less suspicious
  - No proxy is safer than a misconfigured proxy

### Changes
- Main menu: Options rearranged (Config=[3], Logs=[4], Update Checker=[5], Exit=[6])
- Version bumped to 2.4

---

## Beta v2.3 (19.02.2026)

### New Features
- **Proxy Support**
  - HTTP/HTTPS and SOCKS5 proxy support per account
  - Config > `[Y]` Set proxy (URL format: `http://host:port` or `socks5://user:pass@host:port`)
  - Proxy authentication with user:pass supported
  - Proxy status displayed in the launcher for each account (`[HTTP: host:port]` / `[No Proxy]`)
  - SOCKS5 support via optional `aiohttp_socks` dependency
  - Proxy status shown on bot startup in console + warning for multi-acc without proxy
  - **Recommended for multi-acc:** Each account with its own IP > significantly less ban risk

- **IP Check**
  - Config > `[Z]` shows real IP and proxy IP
  - Automatically compares both IPs: Different = Proxy is working
  - Warning if IPs are identical (proxy is not forwarding)
  - Uses fallback chain (ipify, icanhazip, checkip.amazonaws) for reliability
  - Works with HTTP and SOCKS5 proxies

### Changes
- `requirements.txt` extended with `aiohttp_socks` (optional, only for SOCKS proxies)
- Config menu extended: `[Y]` Proxy, `[Z]` IP Check in the settings section
- Proxy info displayed in the config overview under Token/Channel (type + masked URL)

---

## Beta v2.2 (18.02.2026)

### New Features
- **Multi-Account Launcher**
  - New central launcher (`launcher.py` / `start_multi-acc.bat`)
  - `[A]` Add account (e.g. "main", "alt1", "alt2")
  - `[K]` Configure account (opens the normal CatchBot config menu)
  - `[S]` Start all ready accounts simultaneously (each in its own console window)
  - `[1-9]` Start or configure individual account
  - `[P]` Show running processes and optionally terminate all
  - `[R]` Remove account from list
  - Automatically checks if token and channel ID are set
  - Each account gets its own files: `config_<name>.json`, `stats_<name>.json`, `logs/<name>/`
  - Workflow: `start_multi-acc.bat` > Add account > `[K]` Set token & channel > `[S]` Start all

- **Auto-Release Duplicates**
  - Automatically release duplicate Pokemon with `;release duplicates`
  - Keeps Legendary and Shiny automatically
  - Configurable interval (default: every 50 catches)
  - Config > [X] on/off, [V] set interval

- **Captcha Balance Check**
  - Check 2Captcha/CapSolver balance directly in the config menu
  - Config > [G] shows current balance of both services
  - Color-coded display: Green (>$1), Yellow (>$0.20), Red (<$0.20)

- **Pokemon Name Recognition**
  - Pokemon names are now displayed in the log and console
  - `Pokemon_Names.txt` as database with all Pokemon names
  - Automatically cleans Discord markdown and emojis
  - Also recognizes multi-part names (Tapu Koko, Mr. Mime, Iron Hands, etc.)
  - Example: `[23:49:57] UNCOMMON (Wingull) > Pokeball clicked! (Button 0) Caught`

### Bug Fixes & Stability
- Various bug fixes and stability improvements
- Auto-Catch guard logging for better debugging
- Main loop now also pauses during auto-release
- Multi-Account support: `--account` argument for separate configs per account

---

## Beta v2.1 (18.02.2026)

### New Features
- **Daily Catch Limit Detection**
  - Automatically detects "you have reached the daily catch limit"
  - Bot pauses completely + large red warning + alarm + webhook alert
  - Continue with [P] after the limit has been reset

- **Catch Result Detection (Caught/Fled)**
  - Clean single-line log output with result appended directly:
    ```
    [23:49:57] UNCOMMON > Pokeball clicked! (Button 0) Caught
    [23:50:12] RARE > Greatball clicked! (Button 1) Fled
    ```
  - Waits up to 8 seconds for PokeMeow response

- **Session Statistics & Tracking**
  - **[I] Hotkey** shows current session summary at any time
  - Total caught/fled, catch rate %, catch rate per rarity, session duration, best catches
  - **Shiny/Legendary Counter** — Session + All-Time
  - **Persistent Stats** in `stats.json`

- **Discord Webhook Notifications**
  - Send catch results to a Discord webhook
  - Configurable which rarities are reported
  - Catch limit warning via webhook
  - **Dedicated configuration window** via Config > [W]

### Bug Fixes
- AutoBuyer fix: 5s cooldown after purchase
- AutoBuyer also on on_message_edit

---

## Beta v2.0 (17.02.2026)

### New Features
- **AutoEgg System**
  - Automatic egg hatch + hold on bot startup
  - Detects "egg is ready to hatch" during hunting automatically
  - Toggleable in Config under [E]

- **Captcha Auto-Solve (2Captcha + CapSolver)**
  - Optimized for PokeMeow: Numbers only (numeric=1), 3-8 digits
  - Configurable retry system (1-5 attempts)

- **Temp-Ban Detection**
  - Bot pauses completely + warning + alarm
  - Continue with [P] after ban expires

- **Colored Catch Messages**
  - Common/Uncommon = Blue | Rare = Orange/Yellow | Super Rare = Light Yellow
  - Legendary = Purple | Shiny = Pink

- **AutoBuyer System**
  - Automatically monitors ball inventory after each catch
  - Dedicated configuration window via Config > [B]

### Bug Fixes
- Lootbox emoji false positives fixed
- Egg startup improved
- Captcha retry fix, captcha numeric fix
- Deprecated asyncio fix, code cleanup

---

## Beta v1.9 (16.02.2026)

### New Features
- Captcha detection with auto-pause
- Captcha alarm: Desktop notification + sound
- Captcha auto-resume on edited messages
- Pause/Resume hotkey [P], Back hotkey [Q/ESC]

---

## Beta v1.8

### Features
- Auto-Catch system with rarity detection (button click)
- Configurable ball rules
- Live logging in `logs/` folder
- Daily tasks: ;daily, ;h, ;swap, ;q
- Hunting loop with random intervals (11-15s)
- Optional fishing (;f)

---

## Beta v1.3

### Initial Features
- Automatic daily tasks
- Automatic Pokemon hunting
- Auto-Catch based on rarity (text commands)
- Basic logging system
