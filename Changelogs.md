# Changelog

## Beta v2.4 (19.02.2026)

### Neue Features
- **Start-Menue ueberarbeitet**
  - `[1] Start` — Startet den Bot direkt (nur Hunting + Auto-Catch)
  - `[2] Start + Daily Tasks` — Fuehrt zuerst alle Dailys aus (;daily, ;h, ;swap, ;q), dann Hunting
  - Daily Tasks sind nicht mehr einzeln in der Config an/ausschaltbar — stattdessen entscheidet man direkt beim Start
  - Weniger Klicks, schnellerer Workflow

- **Config-Menue vereinfacht**
  - Daily Task Toggles ([1]-[4]) entfernt — werden jetzt ueber Startmenue gesteuert
  - Auto-Catch ist jetzt `[1]`, Ball-Regeln `[2]`, Fish `[3]`
  - Uebersichtlicher und weniger Optionen

### Proxy-Warnung (wichtig!)
- **Proxy nur nutzen wenn Browser + Bot denselben Proxy verwenden!**
  - Wenn der Bot ueber einen Proxy in Tokyo laeuft, aber der Browser (wo du den Account manuell nutzt) ueber deine echte IP in Deutschland, sieht Discord IP-Spruenge zwischen Kontinenten — das ist ein Ban-Risiko!
  - Jedes Geraet und jeder Browser, der denselben Account nutzt, muss denselben Proxy verwenden
  - Ausnahme: Deutsche Residential Proxies sind weniger auffaellig wenn man selbst in DE ist
  - Ohne Proxy ist man sicherer als mit falsch konfiguriertem Proxy

### Aenderungen
- Hauptmenue: Optionen verschoben (Config=[3], Logs=[4], Update-Checker=[5], Beenden=[6])
- Version auf 2.4 erhoeht

---

## Beta v2.3 (19.02.2026)

### Neue Features
- **Proxy-Support** 🔒
  - HTTP/HTTPS und SOCKS5 Proxy-Support pro Account
  - Config → `[Y]` Proxy setzen (URL-Format: `http://host:port` oder `socks5://user:pass@host:port`)
  - Proxy-Authentifizierung mit User:Pass unterstützt
  - Proxy-Status wird im Launcher bei jedem Account angezeigt (`[HTTP: host:port]` / `[Kein Proxy]`)
  - SOCKS5 Support via optionale `aiohttp_socks` Dependency
  - Proxy-Status beim Bot-Start in der Konsole + Warnung bei Multi-Acc ohne Proxy
  - **Empfohlen für Multi-Acc:** Jeder Account eigene IP → deutlich weniger Ban-Gefahr

- **IP-Check** 🌐
  - Config → `[Z]` zeigt echte IP und Proxy-IP an
  - Vergleicht beide IPs automatisch: Unterschiedlich = Proxy funktioniert ✓
  - Warnung wenn IPs identisch sind (Proxy leitet nicht weiter)
  - Nutzt Fallback-Kette (ipify, icanhazip, checkip.amazonaws) für Zuverlässigkeit
  - Funktioniert mit HTTP und SOCKS5 Proxies

### Änderungen
- `requirements.txt` erweitert um `aiohttp_socks` (optional, nur für SOCKS-Proxies)
- Config-Menü erweitert: `[Y]` Proxy, `[Z]` IP-Check im Einstellungen-Bereich
- Proxy-Info wird in der Config-Übersicht unter Token/Channel angezeigt (Typ + maskierte URL)

---

## Beta v2.2 (18.02.2026)

### Neue Features
- **Multi-Account Launcher** 🚀
  - Neuer zentraler Launcher (`launcher.py` / `start_multi-acc.bat`)
  - `[A]` Account hinzufügen (z.B. "main", "alt1", "alt2")
  - `[K]` Account konfigurieren (öffnet das normale CatchBot Config-Menü)
  - `[S]` Alle bereiten Accounts gleichzeitig starten (jeder in eigenem Konsolenfenster)
  - `[1-9]` Einzelnen Account starten oder konfigurieren
  - `[P]` Laufende Prozesse anzeigen und optional alle beenden
  - `[R]` Account aus der Liste entfernen
  - Prüft automatisch ob Token und Channel ID gesetzt sind
  - Jeder Account bekommt eigene Dateien: `config_<name>.json`, `stats_<name>.json`, `logs/<name>/`
  - Workflow: `start_multi-acc.bat` → Account hinzufügen → `[K]` Token & Channel setzen → `[S]` alle starten

- **Auto-Release Duplikate** ♻️
  - Automatisch doppelte Pokemon releasen mit `;release duplicates`
  - Behält Legendary und Shiny automatisch
  - Konfigurierbares Intervall (Standard: alle 50 Catches)
  - Config → [X] an/aus, [V] Intervall einstellen

- **Captcha-Guthaben abfragen** 💰
  - Direkt im Config-Menü das 2Captcha/CapSolver Guthaben prüfen
  - Config → [G] zeigt aktuelles Guthaben beider Services
  - Farbige Anzeige: Grün (>$1), Gelb (>$0.20), Rot (<$0.20)

- **Pokemon-Namens-Erkennung** 📖
  - Pokemon-Namen werden jetzt im Log und in der Konsole angezeigt
  - `Pokemon_Names.txt` als Datenbank mit allen Pokemon-Namen
  - Bereinigt Discord-Markdown und Emojis automatisch
  - Erkennt auch mehrteilige Namen (Tapu Koko, Mr. Mime, Iron Hands, etc.)
  - Beispiel: `[23:49:57] 🎯 UNCOMMON (Wingull) → Pokeball geklickt! (Button 0) ✅ Gefangen`

### Bug Fixes & Stabilität
- Diverse Bug Fixes und Stabilitäts-Verbesserungen
- Auto-Catch Guard-Logging für besseres Debugging
- Main Loop pausiert jetzt auch während Auto-Release
- Multi-Account Support: `--account` Argument für separate Configs pro Account

---

## Beta v2.1 (18.02.2026)

### Neue Features
- **Daily Catch-Limit Erkennung** ⛔
  - Erkennt "you have reached the daily catch limit" automatisch
  - Bot pausiert komplett + große rote Warnung + Alarm + Webhook-Alert
  - Mit [P] fortsetzen nachdem das Limit zurückgesetzt wurde

- **Catch-Result Erkennung (Gefangen/Geflohen)** ✅❌
  - Saubere einzeilige Log-Ausgabe mit Ergebnis direkt angehängt:
    ```
    [23:49:57] 🎯 UNCOMMON → Pokeball geklickt! (Button 0) ✅ Gefangen
    [23:50:12] 🎯 RARE → Greatball geklickt! (Button 1) ❌ Geflohen
    ```
  - Wartet bis zu 8 Sekunden auf PokéMeow-Antwort

- **Session-Statistiken & Tracking** 📊
  - **[I] Hotkey** zeigt jederzeit die aktuelle Session-Zusammenfassung
  - Gesamt gefangen/geflohen, Catch-Rate %, Fangquote pro Rarity, Session-Dauer, beste Fänge
  - **Shiny/Legendary Counter** — Session + All-Time
  - **Persistent Stats** in `stats.json`

- **Discord Webhook Benachrichtigungen** 🔔
  - Fang-Ergebnisse an einen Discord Webhook senden
  - Konfigurierbar welche Rarities gemeldet werden
  - Catch-Limit Warnung über Webhook
  - **Eigenes Konfigurations-Fenster** über Config → [W]

### Bug Fixes
- AutoBuyer Fix: 5s Cooldown nach dem Kauf
- AutoBuyer auch bei on_message_edit

---

## Beta v2.0 (17.02.2026)

### Neue Features
- **AutoEgg System** 🥚
  - Automatisches Egg Hatch + Hold beim Bot-Start
  - Erkennt "egg is ready to hatch" während dem Hunting automatisch
  - In Config unter [E] an/ausschaltbar

- **Captcha Auto-Solve (2Captcha + CapSolver)** 🤖
  - Optimiert für PokéMeow: Nur Zahlen (numeric=1), 3-8 Stellen
  - Konfigurierbares Retry-System (1-5 Versuche)

- **Temp-Ban Erkennung** ⛔
  - Bot pausiert komplett + Warnung + Alarm
  - Nach Ablauf des Bans mit [P] fortsetzen

- **Farbige Catch-Nachrichten** 🎨
  - Common/Uncommon = 🔵 Blau | Rare = 🟡 Orange/Gelb | Super Rare = 🟡 Hellgelb
  - Legendary = 🟣 Lila | Shiny = 🩷 Pink/Rosa

- **AutoBuyer System** 🛒
  - Überwacht Ball-Bestände automatisch nach jedem Catch
  - Eigenes Konfigurations-Fenster über Config → [B]

### Bug Fixes
- Lootbox-Emoji False Positives behoben
- Egg Startup verbessert
- Captcha Retry Fix, Captcha Numeric Fix
- Deprecated asyncio Fix, Code Cleanup

---

## Beta v1.9 (16.02.2026)

### Neue Features
- Captcha-Erkennung mit Auto-Pause
- Captcha-Alarm: Desktop-Notification + Sound
- Captcha-Auto-Resume bei bearbeiteten Nachrichten
- Pause/Resume Hotkey [P], Zurück-Hotkey [Q/ESC]

---

## Beta v1.8

### Features
- Auto-Catch System mit Rarity-Erkennung (Button-Klick)
- Ball-Regeln konfigurierbar
- Live-Logging in `logs/` Ordner
- Daily-Tasks: ;daily, ;h, ;swap, ;q
- Hunting-Loop mit zufälligen Intervallen (11-15s)
- Optionales Fischen (;f)

---

## Beta v1.3

### Initiale Features
- Automatische Daily-Tasks
- Automatisches Pokemon Hunting
- Auto-Catch basierend auf Rarity (Text-Commands)
- Basis Logging-System
