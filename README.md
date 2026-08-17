<p align="center">
  <picture>
    <source srcset="https://cdn.jsdelivr.net/gh/ExoJdi/Method-Raid-Tools-3.3.5a/.github/logo.png" media="(prefers-color-scheme: dark)">
    <img src="https://cdn.jsdelivr.net/gh/ExoJdi/Method-Raid-Tools-3.3.5a/.github/logo.png" alt="Method Raid Tools" width="120">
  </picture>
</p>

<p align="center">
  <b>Method Raid Tools — Full backport for Project Ascension Conquest of Azeroth</b><br>
  <i>Modern raid tools on the 3.3.5a client</i>
</p>

<br>

<h1 align="center">Method Raid Tools — Full backport for Project Ascension Conquest of Azeroth</h1>

<p align="center">
  <a href="#english">English</a> ·
  <a href="#russian">Русский</a>
</p>

---

<a name="english"></a>
## English

<p align="center">
  <a href="#modules">Modules</a> ·
  <a href="#installation">Installation</a> ·
  <a href="#advanced-features">Advanced Features</a> ·
  <a href="#slash-commands">Slash Commands</a> ·
  <a href="#credits">Credits</a>
</p>

### About

**Method Raid Tools (MRT)** is a comprehensive raid utility addon: notes, timers, reminders, readiness checks, combat analysis, and more. This repository is a **full backport of MRT** (based on versions 4840 + 5240) to the **WoW 3.3.5a (WotLK)** client.

Method Raid Tools for Project Ascension: Conquest of Azeroth

Version: 0.1.0-CoA Beta
Status: Beta
Client: Project Ascension / 3.3.5a

<a name="modules"></a>
### Modules

#### Raid coordination

- **Note** — shared raid notes with icons, timers (`{time:...}`), personal assignments, and multiple boss pages. Syncs with raid leader.
- **VisNote (Visual Note)** — graphical note builder: drop boss icons, player tokens, markers and arrows on a canvas, then broadcast the picture alongside the text note.
- **Reminder** — the built-in reminder engine (a "WeakAuras-lite" inside MRT). Triggers on spells, auras, casts, distance, raid composition, BigWigs/DBM events. **Boss timings for WotLK, TBC and Vanilla raids are bundled** (imported from DBM-Warmane).
- **Timers** — pull timer, break timer, custom raid timers, dynamic pull timer (spec‑optimal window).
- **Raid Cooldowns (ExCD2)** — external CD bars: defensives, raid CDs (Divine Hymn, Tranquility, AMZ, etc.), Innervate, brez. Highlights on nameplates when a watched CD is ready.
- **Interrupts** — kick rotation tracker: interrupt log, per‑target assignments, nameplate highlights, "next kicker" announcer.
- **Raid Check** — food/flask/runes/Vantus/raid buffs/durability checker. CTRA/oRA3 interop, autonomous inspect‑based durability scan, one‑click ready check.
- **Raid Groups** — saved raid layouts (profiles) you can apply with one command.
- **Invite Tool** — auto‑invite, disband, reinvite, keyword‑based invite lists.
- **Auto Logging** — automatically toggles `/combatlog` on pull and off on wipe/kill.
- **Encounter** — server‑agnostic encounter start/end detection (shim for `ENCOUNTER_START/END`).
- **Pets** — pet‑owner mapping for correct damage/cd attribution.
- **WhoPulled** — "who pulled the boss?" detector with chat callout.

#### Inspect & Loot

- **Inspect / Inspect Viewer** — async inspection of gear, gems, enchants, talents (main tree), set bonuses, and item levels across the raid.
- **Loot History** — local loot log filtered by player/boss/instance.
- **Loot Link** — posts the entire loot window to chat with `/rt loot`.

#### Combat analysis — Fight Log (BossWatcher)

- **BossWatcher (Fight Log)** — in‑game combat‑log analyzer. Tabs: Damage, Healing, Buffs/Debuffs, Enemy Casts, Player Spells, Power, Interrupts/Dispels, Tracking, Death Log, Positions. Full phase support, fight persistence across reloads, **memory cap protection for 32‑bit clients** (see below).

#### Other

- **WA Checker** — checks WeakAuras versions across the raid.
- **Profiles** — export/import addon profiles between characters.

<a name="installation"></a>
### Installation

1. Download the latest release (or clone this repository).
2. Extract the archive.
3. **Important:** the addon folder must be named exactly **`MRT`** (GitHub ZIPs extract with a `-master` suffix — rename it).
4. Move the `MRT` folder into: \Interface\AddOns\
5. **No need** to install `!!!ClassicAPI` — all missing APIs (Mixin, CreateColor, SetGradient, etc.) are shimmed in `Compat335.lua`.
6. Enable the addon on the character‑select screen and launch the game. Type `/mrt` (or `/rt`, `/exrt`, `/ert`, `/raidtools`, `/methodraidtools`) to open.

### Compatibility

This fork is developed and tested specifically for
Project Ascension: Conquest of Azeroth.

Other 3.3.5a private servers are not supported by this fork.

<a name="advanced-features"></a>
### Advanced Features

#### Fight Log memory cap fix (32‑bit WoW)

Retail MRT records every CLEU event into Lua tables. On a 64‑bit client this is fine; on a 32‑bit 3.3.5a client a long progress night will OOM the game. This backport adds a **per‑fight memory budget** (configurable in `MRT/BossWatcher.lua`):

| Cap                          | Default | What it limits                              |
| ---------------------------- | ------- | ------------------------------------------- |
| `BW_AURA_CAP`                | 30 000  | Total aura applies/removes per fight        |
| `BW_CAST_CAP_PER_SOURCE`     | 5 000   | Cast events per source per fight            |
| `BW_GRAPH_CAP`               | 1 500   | DPS/HPS graph data points per fight         |
| `BW_TRACKED_GUID_CAP`        | 1 000   | Distinct tracked GUIDs per fight            |
| `BW_TOTAL_BUDGET_KB`         | 150 MB  | Cumulative budget across saved fights       |

When a cap is hit, MRT prints a warning and truncates only the granular log (totals remain accurate). Two extra toggles in `/mrt` → BossWatcher options:

- **Disable buffs & debuffs** — skips aura logging entirely (massive memory saving).
- **Reduced detail (5s segments)** — increases segment interval from 1s to 5s (∼5× memory reduction).

<a name="slash-commands"></a>
### Slash Commands

All commands accept any of these prefixes: `/rt`, `/exrt`, `/ert`, `/mrt`, `/raidtools`, `/methodraidtools`.

#### Core

| Command | Description |
|---------|-------------|
| `/rt` | Open MRT options window |
| `/rt set` | Open options (alias) |
| `/rt icon` | Toggle minimap icon |
| `/rt getver` | Ask raid for MRT version |
| `/rt getverg` | Ask guild for MRT version |
| `/rt quit` | Disable MRT until reload |
| `/rt profiler` | Open CPU profiler window |
| `/rl` | Reload UI (registered by MRT) |

#### Note

| Command | Description |
|---------|-------------|
| `/rt note` (`/rt n`) | Toggle on‑screen note window |
| `/rt editnote` | Open Notes tab in options |
| `/rt note set <name>` | Switch to and broadcast saved note `<name>` |
| `/rt note show <name>` | Open extra window for note `<name>` |
| `/rt note timer` | Fire ENCOUNTER_START/END for testing timers |
| `/rt note starttimer <name>` | Start custom note timer |
| `/rt note synctimer <name>` | Sync custom note timer with raid |
| `/rt note phase <N>` | Advance note phase counter to N |

#### Timers

| Command | Description |
|---------|-------------|
| `/rt pull` | 10‑second pull timer |
| `/rt pull <X>` | X‑second pull timer |
| `/rt afk <X>` | X‑minute break timer (0 = cancel) |
| `/rt timer <NAME> <X>` | Custom raid timer |
| `/rt mytimer <X>` | Local countdown to X seconds |
| `/rt dpt` | Dynamic Pull Timer (spec‑optimal time) |

#### Marks & World Markers

| Command | Description |
|---------|-------------|
| `/rt mark 1..4` | Apply mark groups 1‑4 (configured in MarksSimple) |
| `/rt mark 5` | Clear all marks |
| `/rt mb` / `/rt mm` | Toggle floating Marks Bar |

#### Raid Groups

| Command | Description |
|---------|-------------|
| `/rt raidgroup <name>` | Apply saved raid layout `<name>` |
| `/rt inv` | Auto‑invite |
| `/rt dis` | Disband raid |
| `/rt reinv` | Disband + reinvite raid |
| `/rt invlist <N>` | Invite from preset list N |

#### Raid Check

| Command | Description |
|---------|-------------|
| `/rt check` | Open raid buffs/consumables window |
| `/rt food` | Print missing food (local) |
| `/rt foodchat` | Announce missing food to chat |
| `/rt flask` | Print missing flasks (local) |
| `/rt flaskchat` | Announce missing flasks to chat |
| `/rt potion` | Print missing potion users |
| `/rt potionchat` | Announce missing potion users |
| `/rt check r` / `rc` | Check runes (DK runeforges/runescrolls) |
| `/rt check v` / `vc` | Check Vantus runes |
| `/rt check b` / `bc` | Check raid buffs |

#### Attendance

| Command | Description |
|---------|-------------|
| `/rt roster` | Save current raid roster snapshot |

#### Raid Cooldowns (ExCD2)

| Command | Description |
|---------|-------------|
| `/rt cd` | Toggle Raid CD bars on/off |
| `/rt runcd <id> <name>` | Force‑trigger a CD (debug) |
| `/rt resetcd <id> <name>` | Reset a CD to ready (debug) |
| `/cddebug` | Toggle CD debug logging |

#### Inspect / Loot

| Command | Description |
|---------|-------------|
| `/rt raid` | Open Inspect Viewer (gear/talents) |
| `/rt loot` | Link loot window contents to chat |

#### Fight Log (BossWatcher)

| Command | Description |
|---------|-------------|
| `/rt fl` (`/rt bw`) | Open Fight Log window |
| `/rt fl s` / `start` | Manually start logging current fight |
| `/rt fl e` / `end` | Manually stop logging |
| `/rt fl c` / `clear` | Clear in‑memory fight buffer |
| `/rt fl reset` | Wipe saved‑fights buffer |
| `/rt fl save` | Save current fights to SavedVariables |
| `/rt fl phase` | Advance active phase counter |
| `/rt fl allenergy` | Toggle "log all energy" debug mode |

#### Reminder

| Command | Description |
|---------|-------------|
| `/rt rem ver` | Query raid for Reminder DB version |

#### Debug

| Command | Description |
|---------|-------------|
| `/mrtcleu on` | Enable CLEU sniffer (prints every spell event) |
| `/mrtcleu off` | Disable sniffer, print summary |
| `/mrtcleu status` | Show CLEU dispatcher wiring status |
| `/rt profiler` | CPU profiler |
| `/cdtalread` | Talent broadcast debug |

<a name="credits"></a>
### Credits

- Original addon: **Method Raid Tools Team**
- WotLK 3.3.5a backport: **ExoJdi**
- Boss timings database: **Zidras / DBM-Warmane**
- Testing & community support

---

<a name="russian"></a>
## Русский

<p align="center">
<a href="#модули-1">Модули</a> ·
<a href="#установка-1">Установка</a> ·
<a href="#расширенные-возможности">Расширенные возможности</a> ·
<a href="#команды-слэша">Команды слэша</a> ·
<a href="#благодарности-1">Благодарности</a>
</p>

### Об аддоне

**Method Raid Tools (MRT)** — набор инструментов для рейдовой игры. Этот репозиторий содержит **полный бэкпорт MRT** (версии 4840 + 5240) для клиента **WoW 3.3.5a (WotLK)**.

<a name="модули-1"></a>
### Модули

#### Рейд-координация

- **Note** — рейдовые заметки с иконками, таймерами и синхронизацией.
- **VisNote** — визуальный редактор заметок (игровое поле с иконками).
- **Reminder** — движок напоминаний (триггеры на заклинания, ауры, касты, фазы боссов). Встроены тайминги для всех боссов WotLK/TBC/Vanilla.
- **Timers** — пулл-таймер, таймер перерыва, пользовательские таймеры.
- **Raid Cooldowns (ExCD2)** — панель внешних кулдаунов рейда.
- **Interrupts** — трекер кик-ротации.
- **Raid Check** — проверка расходников и баффов.
- **Raid Groups** — сохранённые раскладки групп.
- **Invite Tool** — авто-приглашение, реинвайт.
- **Auto Logging** — автоматический `/combatlog`.

#### Инспект и лут

- **Inspect Viewer** — просмотр экипировки, талантов, камней и чарок всего рейда.
- **Loot History** — лог выпавшего лута.
- **Loot Link** — отправка содержимого лута в чат.

#### Анализ боя — Fight Log

- **BossWatcher** — встроенный анализатор логов с вкладками урона, исцеления, кастов, прерываний и т.д. Защита от переполнения памяти.

#### Другое

- **WA Checker** — проверка версий WeakAuras.
- **Profiles** — экспорт/импорт профилей.

<a name="установка-1"></a>
### Установка

1. Скачайте релиз или клонируйте репозиторий.
2. Распакуйте архив.
3. **Важно:** папка должна называться ровно **`MRT`**.
4. Переместите `MRT` в `\Interface\AddOns\`.
5. **`!!!ClassicAPI` не требуется** — все шимы уже внутри.
6. Включите аддон в игре и введите `/mrt`.

### Совместимость

- **WoW 3.3.5a** (Interface `30300`). Работает на любом WotLK-сервере.

<a name="расширенные-возможности"></a>
### Расширенные возможности

#### Защита памяти Fight Log (32-битный клиент)

В `MRT/BossWatcher.lua` заданы лимиты памяти на бой. При достижении лимита старое сохранение удаляется, а не вызывает вылет клиента. В настройках BossWatcher можно отключить запись ауэр или уменьшить детализацию графиков.

<a name="команды-слэша"></a>
### Команды слэша

Основные префиксы: `/rt`, `/mrt`, `/exrt`, `/ert`, `/raidtools`, `/methodraidtools`.

| Команда | Описание |
|---------|----------|
| `/rt` | Открыть настройки |
| `/rt pull [X]` | Пулл-таймер |
| `/rt check` | Проверка расходников |
| `/rt note` | Показать заметки |
| `/rt fl` | Открыть боевой лог |
| `/rt cd` | Панель кулдаунов |
| `/rt raid` | Инспектор экипировки |
| `/rt loot` | Отправить лут в чат |
| `/rt roster` | Сохранить состав рейда |
| `/rt inv` / `/rt dis` | Пригласить / расформировать |

Полный список команд см. в [английской версии](#slash-commands).

<a name="благодарности-1"></a>
### Благодарности

- Оригинальный аддон: **Команда Method Raid Tools**
- Бэкпорт: **ExoJdi**
- Тайминги боссов: **Zidras / DBM-Warmane**

---

## Сообщение об ошибках

Если вы нашли баг, пожалуйста, откройте **issue** на GitHub:  
[https://github.com/ExoJdi/Method-Raid-Tools-3.3.5a/issues](https://github.com/ExoJdi/Method-Raid-Tools-3.3.5a/issues)

Приложите:
- Описание что произошло и что ожидалось.
- Шаги воспроизведения.
- Скриншоты, ошибку Lua (если есть).
- Результат `/mrtcleu status` (если уместно).

---

## Donate / Поддержать

Если бэкпорт спас ваш рейд, вы можете поблагодарить автора:  
**[boosty.to/exojdi](https://boosty.to/exojdi)**

Удачи на пуллах! — *exojdi*