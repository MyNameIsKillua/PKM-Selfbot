<div align="center">

# Changelog

**All notable changes to CatchBot are documented here.**

[![Current Version](https://img.shields.io/badge/Latest-Beta_v2.5-blue?style=for-the-badge)]()
[![Release Date](https://img.shields.io/badge/Updated-19.02.2026-green?style=for-the-badge)]()

---

</div>

## Beta v2.5 &mdash; *19.02.2026*

> Captcha system overhaul &mdash; CapSolver removed, Anti-Captcha added, report feedback for both services.

<details>
<summary><b>Captcha System Reworked</b></summary>

&nbsp;

#### CapSolver removed, Anti-Captcha added

| Detail | Description |
|:-------|:------------|
| **New Service** | [Anti-Captcha](https://anti-captcha.com) replaces CapSolver as the second captcha option |
| **API Endpoints** | `createTask`, `getTaskResult`, `getBalance`, `reportIncorrectImageCaptcha` |
| **Config `[D]`** | Select service: 2Captcha / Anti-Captcha / Manual |
| **Config `[K]`** | Set Anti-Captcha API key (was previously CapSolver) |
| **Balance `[G]`** | Now shows 2Captcha + Anti-Captcha balance |
| **Migration** | Old `capsolver_api_key` entries are automatically removed |

#### Report Feedback (New)

Reports are sent automatically when PokeMeow responds:
- `"thank you"` = correct &rarr; `reportCorrect`
- `"incorrect"` = wrong &rarr; `reportIncorrect` / `reportIncorrectImageCaptcha`

| Service | Feedback |
|:--------|:---------|
| **2Captcha** | `reportCorrect` + `reportIncorrect` |
| **Anti-Captcha** | `reportIncorrectImageCaptcha` (refund within 60s) |

> [!NOTE]
> Reports improve solution quality over time &mdash; services use them to optimize their workers.

#### Captcha Numbers Optimized

```
minLength:   3      (was 3)
maxLength:   6      (was 8 -- PokeMeow only uses 3-6 digits)
numeric:     1      (numbers only, 1-9)
comment:     improved description for better recognition
```

</details>

<details>
<summary><b>Other Changes</b></summary>

&nbsp;

- Version bumped to **2.5**
- Config migration: `capsolver_api_key` automatically removed, service reset to `"Manual"` if CapSolver was active

</details>

---

## Beta v2.4 &mdash; *19.02.2026*

> Start menu reworked, config simplified, proxy safety warning.

<details>
<summary><b>New Features</b></summary>

&nbsp;

#### Start Menu Reworked

| Option | Action |
|:-------|:-------|
| `[1] Start` | Hunting + auto-catch only |
| `[2] Start + Daily Tasks` | Runs all dailies first (`;daily`, `;h`, `;swap`, `;q`), then hunting |

- Daily tasks are no longer individually toggleable in config &mdash; decided at startup
- Fewer clicks, faster workflow

#### Config Menu Simplified

| Key | Function |
|:----|:---------|
| `[1]` | Auto-Catch |
| `[2]` | Ball Rules |
| `[3]` | Fish |

- Daily task toggles `[1]-[4]` removed &mdash; now controlled via start menu

</details>

<details>
<summary><b>Proxy Warning</b></summary>

&nbsp;

> [!WARNING]
> **Only use a proxy if browser + bot use the same proxy!**
>
> If the bot runs through a proxy in Tokyo, but your browser uses your real IP in Germany, Discord sees IP jumps between continents &mdash; **this is a ban risk!**
>
> - Every device and browser on the same account must use the **same proxy**
> - **Exception:** Residential proxies from your own country are less suspicious
> - **No proxy is safer than a misconfigured proxy**

</details>

<details>
<summary><b>Other Changes</b></summary>

&nbsp;

- Main menu rearranged: Config=`[3]`, Logs=`[4]`, Update Checker=`[5]`, Exit=`[6]`
- Version bumped to **2.4**

</details>

---

## Beta v2.3 &mdash; *19.02.2026*

> Proxy support and IP check for multi-account safety.

<details>
<summary><b>New Features</b></summary>

&nbsp;

#### Proxy Support

| Feature | Detail |
|:--------|:-------|
| **Protocols** | HTTP / HTTPS / SOCKS5 |
| **Config `[Y]`** | Set proxy URL (`http://host:port` or `socks5://user:pass@host:port`) |
| **Auth** | Username + password supported |
| **Launcher** | Proxy status shown per account: `[HTTP: host:port]` / `[No Proxy]` |
| **SOCKS5** | Via optional `aiohttp_socks` dependency |

> [!IMPORTANT]
> **Recommended for multi-acc:** Each account with its own IP significantly reduces ban risk.

#### IP Check

- Config `[Z]` &mdash; shows real IP vs. proxy IP
- Automatically compares both: **Different = Proxy working**
- Warning if IPs are identical (proxy not forwarding)
- Fallback chain: `ipify` &rarr; `icanhazip` &rarr; `checkip.amazonaws`
- Works with HTTP and SOCKS5

</details>

<details>
<summary><b>Other Changes</b></summary>

&nbsp;

- `requirements.txt` extended with `aiohttp_socks` (optional)
- Config menu extended: `[Y]` Proxy, `[Z]` IP Check
- Proxy info displayed in config overview under Token/Channel

</details>

---

## Beta v2.2 &mdash; *18.02.2026*

> Multi-account launcher, auto-release duplicates, balance check, Pokemon name recognition.

<details>
<summary><b>New Features</b></summary>

&nbsp;

#### Multi-Account Launcher

```
[A]   Add account (e.g. "main", "alt1", "alt2")
[K]   Configure account (opens CatchBot config menu)
[S]   Start all ready accounts simultaneously
[1-9] Start or configure individual account
[P]   Show running processes / terminate all
[R]   Remove account from list
```

- Each account gets its own files: `config_<name>.json`, `stats_<name>.json`, `logs/<name>/`
- Automatically checks if token + channel ID are set

#### Auto-Release Duplicates

| Setting | Detail |
|:--------|:-------|
| **Command** | `;release duplicates` |
| **Keeps** | Legendary + Shiny automatically |
| **Interval** | Configurable (default: every 50 catches) |
| **Config** | `[X]` on/off, `[V]` set interval |

#### Captcha Balance Check

Config `[G]` &mdash; shows current balance of both services:

| Balance | Color |
|:--------|:------|
| > $1.00 | Green |
| > $0.20 | Yellow |
| < $0.20 | Red |

#### Pokemon Name Recognition

- Names displayed in log + console via `Pokemon_Names.txt` database
- Cleans Discord markdown and emojis automatically
- Recognizes multi-part names (Tapu Koko, Mr. Mime, Iron Hands, etc.)

```
[23:49:57] UNCOMMON (Wingull) > Pokeball clicked! (Button 0) Caught
```

</details>

<details>
<summary><b>Bug Fixes & Stability</b></summary>

&nbsp;

- Various bug fixes and stability improvements
- Auto-Catch guard logging for better debugging
- Main loop now pauses during auto-release
- Multi-Account support: `--account` argument for separate configs

</details>

---

## Beta v2.1 &mdash; *18.02.2026*

> Catch limit detection, result tracking, session stats, and Discord webhooks.

<details>
<summary><b>New Features</b></summary>

&nbsp;

#### Daily Catch Limit Detection

- Detects `"you have reached the daily catch limit"` automatically
- Bot pauses completely + red warning + alarm + webhook alert
- Resume with `[P]` after limit resets

#### Catch Result Detection

Clean single-line log with result appended:

```
[23:49:57] UNCOMMON > Pokeball clicked! (Button 0) Caught
[23:50:12] RARE     > Greatball clicked! (Button 1) Fled
```

Waits up to 8 seconds for PokeMeow response.

#### Session Statistics & Tracking

| Feature | Detail |
|:--------|:-------|
| **Hotkey `[I]`** | Live session summary at any time |
| **Tracked** | Total caught/fled, catch rate %, rate per rarity, duration, best catches |
| **Counters** | Shiny + Legendary (session + all-time) |
| **Persistent** | Saved in `stats.json` |

#### Discord Webhook Notifications

- Send catch results to a Discord webhook
- Configurable which rarities are reported
- Catch limit warning via webhook
- Dedicated config window: Config `[W]`

</details>

<details>
<summary><b>Bug Fixes</b></summary>

&nbsp;

- AutoBuyer fix: 5s cooldown after purchase
- AutoBuyer also triggers on `on_message_edit`

</details>

---

## Beta v2.0 &mdash; *17.02.2026*

> AutoEgg, captcha auto-solve, temp-ban detection, colored messages, AutoBuyer.

<details>
<summary><b>New Features</b></summary>

&nbsp;

#### AutoEgg System

- Automatic egg hatch + hold on bot startup
- Detects `"egg is ready to hatch"` during hunting
- Config `[E]` to toggle

#### Captcha Auto-Solve

| Setting | Value |
|:--------|:------|
| **Services** | 2Captcha + CapSolver |
| **Optimized** | Numbers only (`numeric=1`), 3-8 digits |
| **Retries** | Configurable 1-5 attempts |

#### Temp-Ban Detection

- Bot pauses completely + warning + alarm
- Resume with `[P]` after ban expires

#### Colored Catch Messages

| Rarity | Color |
|:-------|:------|
| Common / Uncommon | Blue |
| Rare | Orange / Yellow |
| Super Rare | Light Yellow |
| Legendary | Purple |
| Shiny | Pink |

#### AutoBuyer System

- Monitors ball inventory after each catch
- Dedicated config window: Config `[B]`

</details>

<details>
<summary><b>Bug Fixes</b></summary>

&nbsp;

- Lootbox emoji false positives fixed
- Egg startup improved
- Captcha retry + numeric fix
- Deprecated asyncio fix, code cleanup

</details>

---

## Beta v1.9 &mdash; *16.02.2026*

> Captcha detection with auto-pause, alarm system, hotkeys.

<details>
<summary><b>Features</b></summary>

&nbsp;

- Captcha detection with auto-pause
- Captcha alarm: Desktop notification + sound
- Captcha auto-resume on edited messages
- Hotkeys: `[P]` Pause/Resume, `[Q/ESC]` Back

</details>

---

## Beta v1.8

> Core auto-catch system, ball rules, logging, daily tasks.

<details>
<summary><b>Features</b></summary>

&nbsp;

- Auto-Catch with rarity detection (button click)
- Configurable ball rules
- Live logging in `logs/` folder
- Daily tasks: `;daily`, `;h`, `;swap`, `;q`
- Hunting loop with random intervals (11-15s)
- Optional fishing (`;f`)

</details>

---

## Beta v1.3

> Initial release.

<details>
<summary><b>Features</b></summary>

&nbsp;

- Automatic daily tasks
- Automatic Pokemon hunting
- Auto-Catch based on rarity (text commands)
- Basic logging system

</details>

---

<div align="center">

<sub>CatchBot by MyNameIsKillua</sub>

</div>
