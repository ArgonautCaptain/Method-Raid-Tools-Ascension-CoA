# Method Raid Tools (MRT) — WotLK 3.3.5a Backport

A full backport of the **Method Raid Tools** addon from retail to **WoW 3.3.5a**
(Wrath of the Lich King, build `12340`, interface `30300`). It also runs on
TBC and Vanilla 3.3.5a-based private servers.

The codebase is a merge of two upstream sources:

- **MRT 4840** — the last WotLK-era release of MRT.
- **MRT 5240** — the current retail (modern) release of MRT.

Retail features are layered on top of the WotLK base. Everything that still
needs retail APIs is shimmed through `Compat335.lua`, so the same source tree
loads cleanly on a 3.3.5a client without `LoadOutOfDate`.

> **Tested target:** WotLK 3.3.5a (12340) clients, including the
> **Awesome WotLK** patched client (where extra retail APIs and events such
> as `NAME_PLATE_UNIT_ADDED`, `C_NamePlate.*`, `C_VoiceChat.*` work natively).
> The addon detects native APIs and skips its shims when they are present.


![Uploading image.png…]()


---

## WeakAura — Kaze MRT Timer (backported)

The companion WeakAura **Kaze MRT Timer**
([wago.io/bcVeRwoUj](https://wago.io/bcVeRwoUj)) — used as a clean,
modern timer overlay on top of MRT — has been backported to 3.3.5a
and is fully working with this version of MRT. Import it into
WeakAuras-3.3.5a and it will pick up MRT's note timers,
`{time:...}` markup and Reminder events out of the box.

---

## Installation

1. Download / clone this repo.
2. Copy the inner `MRT/` folder into your client's
   `Interface\AddOns\` directory.
3. (Re)launch the game. At the character select screen you should see
   **Method Raid Tools** enabled in the AddOns list.
4. In-game, type `/mrt` (or `/rt`, `/exrt`, `/ert`, `/raidtools`,
   `/methodraidtools`) to open the options window.

No external dependencies are required — `LibStub`, `LibDeflate`, `LibCustomGlow`
and the rest of Ace3-style libs ship vendored in `MRT/libs/`. The old
`!!!ClassicAPI` requirement is **not** needed: `Mixin`, `CreateColor`,
`SetGradient`, etc. are shimmed in `Compat335.lua`.

---

## Modules

Every major retail MRT module has been ported and is enabled by default
(toggleable in `/mrt` → Options).

### Raid coordination

- **Note** — the classic shared raid note (text + icon markup).
  Sync with the raid leader / assists, edit live, multiple per-boss
  pages, dynamic timer markup (e.g. `{time:p1, 0:30}`), per-line color
  highlighting, and per-character "personal" assignments.
- **VisNote (Visual Note)** — graphical note builder: drop boss icons,
  player tokens, marker icons and arrows on a canvas, then broadcast
  the resulting picture to the raid alongside the text note.
- **Reminder** — the personal callout / reminder engine (the retail
  "WeakAuras-lite" inside MRT). Pre-configured trigger types for
  spells, auras, casts, units in range, raid composition, BigWigs/DBM
  events, etc. **Boss timings and phase scripts for WotLK, TBC and
  Vanilla raids are bundled** — they were imported and adapted from
  [Zidras/DBM-Warmane](https://github.com/Zidras/DBM-Warmane), so
  phase-aware reminders (`{phase:N}`, `Switch to phase`, etc.)
  fire on the correct pulls without any extra setup.
- **Timers** — pull timer / break timer / custom raid timer, with
  Dynamic Pull Timer support (per-spec optimal pull window).
- **Raid Cooldowns (ExCD2)** — external CD bars: shows the raid's
  defensives, raid CDs (Divine Hymn, Aura Mastery, Tranquility,
  Anti-Magic Zone, etc.), Innervate, brez CDs, and any custom spell
  per-class. Includes Reminder-style highlighting on the player's
  nameplate when a watched CD is up.
- **Battle Rezz (BattleRes)** — combat-rez tracker. WotLK has no
  shared brez pool, but the panel still tracks per-class cooldown
  state across the raid (Rebirth, Soulstone, Raise Ally) so the
  raid leader knows who is up.
- **Marks (Skull/Cross/etc. assignments)**, **MarksSimple**,
  **MarksBar** — marker assignment lists, the floating mark bar
  with `/rt mark N` macro support, and the world-marker (flare /
  beacon) pinger.
- **Interrupts** — kick rotation tracker. CLEU-driven kick log,
  per-target kick assignments, nameplate highlights and a
  "next kicker" announcer.
- **Raid Check (RaidCheck)** — food / flask / runes / Vantus / raid
  buffs / consumables / durability checker. Supports CTRA / oRA3
  durability interop, autonomous Inspect-based durability scanning,
  and a one-click ready-check window.
- **Raid Groups (RaidGroups)** — saved raid layouts (named profiles)
  that you can re-apply with one click or via slash command.
- **Invite Tool (InviteTool)** — auto-invite, disband, reinvite,
  pre-saved invite lists. Reads keywords from whisper / chat.
- **Raid Attendance (RaidAttendance)** — automatic roster snapshots
  for attendance tracking; can be saved manually with `/rt roster`.
- **Encounter** — server-agnostic encounter detection (`ENCOUNTER_START`
  / `ENCOUNTER_END` shim) used by every other module.
- **Pets** — extra pet info / pet-owner mapping for all CLEU
  consumers (so a Ghoul/Imp's damage credits the right hunter / lock
  / DK in fight log and CD bars).
- **WhoPulled** — first-pull / "who pulled" detector with chat
  callout.
- **Auto Logging (AutoLogging)** — auto-toggles the WoW combat log
  (`/combatlog`) on raid pulls and turns it off on wipes / kills, so
  you do not lose Warcraft Logs uploads.

### Inspect & Loot

- **Inspect / Inspect Viewer (InspectViewer)** — async `NotifyInspect`
  driver. The viewer shows raid-wide gear, gems, enchants, talents
  (primary tree), set bonuses, and an item-level overview.
- **Loot History (LootHistory)** — local loot drop log with filters
  by player / boss / instance.
- **Loot Link (LootLink)** — links the entire loot window to chat
  with one command (`/rt loot`).

### Combat analysis — Fight Log (BossWatcher)

- **BossWatcher (Fight Log)** — the in-game combat-log analyzer.
  Per-fight tabs for Damage, Healing, Buffs/Debuffs, Enemy Casts,
  Player Spells, Power, Interrupts/Dispels, Tracking, Death Log,
  and Positions. Full segment / phase support, "Save fights"
  persistence across `/reload`, and per-fight memory budget guard
  (see below).

### Other

- **WA Checker (WAChecker)** — version-check WeakAuras across the
  raid, see who has outdated / missing auras.
- **Bindings** — keybind support for the modules above
  (`/rt mark N`, world markers, mark bar toggle, etc.).
- **Profiles** — export / import addon profiles between characters.
- **Changelog** — in-game changelog window.
- **BlizzFix / BossIconsPack / DiagonalGlyphAtlas / Options** —
  shared infrastructure (Blizzard UI fixes, the boss-icon spritesheet
  used by Note / VisNote / Reminder, the diagonal glyph atlas, the
  options framework).

---


## Fight Log memory cap fix (32-bit WoW)

Retail MRT records every CLEU event of the pull into per-fight Lua
tables. On a 64-bit retail client this is fine; on a 3.3.5a 32-bit
client a 30-minute progress wipe-fest will OOM the game and either
crash the client or evict you to the character screen.

This backport adds a per-fight memory budget on top of the
BossWatcher recorder. The relevant caps live near the top of
`MRT/BossWatcher.lua`:

| Cap                          | Default | What it limits                              |
| ---------------------------- | ------- | ------------------------------------------- |
| `BW_AURA_CAP`                | 30 000  | Total aura applies/removes per fight        |
| `BW_CAST_CAP_PER_SOURCE`     | 5 000   | Cast events per source per fight            |
| `BW_GRAPH_CAP`               | 1 500   | DPS/HPS graph data points per fight         |
| `BW_TRACKED_GUID_CAP`        | 1 000   | Distinct tracked GUIDs per fight            |
| `BW_TOTAL_BUDGET_KB`         | 150 MB  | Cumulative budget across saved fights       |

When a per-fight cap is hit, MRT stops appending to that bucket
(damage / healing totals are still accurate; only the granular
event log is truncated) and prints a yellow warning to the chat:

> `[MRT/BossWatcher]` recorded fight log truncated (X cap reached).
> Consider enabling 'Disable buffs & debuffs' or 'Reduced detail
> (5s segments)' on long fights to avoid the cap.

When the cumulative `BW_TOTAL_BUDGET_KB` is exceeded across saved
fights, the oldest saved fight is evicted (and a chat warning is
printed) instead of the client OOMing.

Two extra toggles are exposed in `/mrt` → BossWatcher options:

- **Disable buffs & debuffs** — skips aura logging entirely. Buffs
  and debuffs are by far the largest source of memory use; on a long
  H-LK or H-Sindragosa progress night this single toggle drops fight
  RAM by ~5–10×.
- **Reduced detail (5s segments)** — bumps the segment interval
  from 1 s to 5 s, cutting graph/segment memory ~5×.

Both toggles are off by default. Turn them on for long progress
fights or low-memory clients; turn them off for normal tactics
review.

---

## Slash commands

The addon registers six aliases — they all do the same thing.
Examples below use `/rt` for brevity.

```
/mrt   /rt   /exrt   /ert   /raidtools   /methodraidtools
```

### Core

| Command         | Effect                                              |
| --------------- | --------------------------------------------------- |
| `/rt`           | Open the MRT options window.                        |
| `/rt set`       | Open the options window (alias).                    |
| `/rt icon`      | Toggle the minimap icon.                            |
| `/rt getver`    | Ask the raid for everyone's MRT version.            |
| `/rt getverg`   | Same, but query the guild instead of the raid.      |
| `/rt quit`      | Disable MRT for this session (until reload).        |
| `/rt profiler`  | Open the CPU profiling window.                      |
| `/rl`           | Reload UI (registered by MRT for convenience).      |

### Note

| Command                          | Effect                                              |
| -------------------------------- | --------------------------------------------------- |
| `/rt note` *(or `/rt n`)*        | Toggle the on-screen note window.                   |
| `/rt editnote`                   | Open the Notes tab in options.                      |
| `/rt note set <name>`            | Switch to & broadcast the note saved as `<name>`.   |
| `/rt note show <name>`           | Add an extra window for note `<name>`.              |
| `/rt note timer`                 | Manually fire `ENCOUNTER_START` / `ENCOUNTER_END`   |
|                                  | (useful for testing dynamic timer markup).          |
| `/rt note starttimer <name>`     | Start a custom note timer.                          |
| `/rt note synctimer <name>`      | Sync a custom note timer with the raid.             |
| `/rt note phase <N>`             | Manually advance the note phase counter to N.       |

### Timers

| Command                          | Effect                                              |
| -------------------------------- | --------------------------------------------------- |
| `/rt pull` *(no arg)*            | 10-second pull timer.                               |
| `/rt pull <X>`                   | X-second pull timer.                                |
| `/rt afk <X>`                    | X-minute AFK / break timer (0 = cancel).            |
| `/rt timer <NAME> <X>`           | Custom raid timer.                                  |
| `/rt mytimer <X>`                | Set the local countdown frame to X seconds.         |
| `/rt dpt`                        | Dynamic Pull Timer (uses your spec's optimal time). |
| `/rt cleutimer <event> <id> <t>` | CLEU-driven timer (advanced).                       |

### Marks / world markers

| Command          | Effect                                                       |
| ---------------- | ------------------------------------------------------------ |
| `/rt mark 1..4`  | Apply marks group 1..4 (configured in MarksSimple).          |
| `/rt mark 5`     | Clear all marks.                                             |
| `/rt mb` / `/rt mm` | Toggle the floating Marks Bar.                            |

### Raid groups / invite tool

| Command                       | Effect                                          |
| ----------------------------- | ----------------------------------------------- |
| `/rt raidgroup <name>`        | Apply saved raid layout `<name>`.               |
| `/rt inv`                     | Run autoinvite.                                 |
| `/rt dis`                     | Disband the raid.                               |
| `/rt reinv`                   | Disband + reinvite raid.                        |
| `/rt invlist <N>`             | Invite from predefined list N.                  |

### Raid check / consumables

| Command                | Effect                                               |
| ---------------------- | ---------------------------------------------------- |
| `/rt check`            | Open the raid buffs / consumables window.            |
| `/rt food`             | Print missing food locally.                          |
| `/rt foodchat`         | Announce missing food to chat.                       |
| `/rt flask`            | Print missing flasks locally.                        |
| `/rt flaskchat`        | Announce missing flasks to chat.                     |
| `/rt potion`           | Print missing potion users (if PotionCheck is on).   |
| `/rt potionchat`       | Announce missing potion users.                       |
| `/rt check r` / `rc`   | Runes (DK runeforges / runescrolls).                 |
| `/rt check v` / `vc`   | Vantus runes.                                        |
| `/rt check b` / `bc`   | Raid buffs.                                          |

### Raid attendance

| Command         | Effect                                            |
| --------------- | ------------------------------------------------- |
| `/rt roster`    | Save the current raid roster snapshot.            |

### Raid Cooldowns (ExCD2)

| Command                    | Effect                                        |
| -------------------------- | --------------------------------------------- |
| `/rt cd`                   | Toggle the Raid CD bars on/off.               |
| `/rt runcd <id> <name>`    | Force-trigger a CD line (debug).              |
| `/rt resetcd <id> <name>`  | Reset a CD line to ready (debug).             |

### Inspect Viewer

| Command         | Effect                                            |
| --------------- | ------------------------------------------------- |
| `/rt raid`      | Open the Inspect Viewer (gear / talents).         |

### Loot Link

| Command         | Effect                                            |
| --------------- | ------------------------------------------------- |
| `/rt loot`      | Link the contents of the loot window to chat.     |

### Fight Log (BossWatcher)

| Command                                     | Effect                                  |
| ------------------------------------------- | --------------------------------------- |
| `/rt fl` *(or `/rt bw`)*                    | Open the Fight Log window.              |
| `/rt fl s` / `/rt fl start`                 | Manually start logging the current fight. |
| `/rt fl e` / `/rt fl end`                   | Manually stop logging.                  |
| `/rt fl c` / `/rt fl clear`                 | Clear the in-memory fight buffer.       |
| `/rt fl reset`                              | Wipe the saved-fights buffer (`SAVED_DATA`). |
| `/rt fl save`                               | Save current fights into SavedVariables. |
| `/rt fl phase`                              | Manually advance the active phase counter. |
| `/rt fl allenergy`                          | Toggle "log all energy" mode (debug).    |

### Reminder

| Command         | Effect                                              |
| --------------- | --------------------------------------------------- |
| `/rt rem ver`   | Query the raid for everyone's Reminder DB version.  |

---

## Debug commands

Useful only when reproducing a bug or wiring a new module. They add
spam to the chat frame, so leave them off in normal play.

| Command                  | Effect                                                                       |
| ------------------------ | ---------------------------------------------------------------------------- |
| `/mrtcleu on`            | Enable the **CLEU sniffer** — prints every `SPELL_CAST_SUCCESS` /           |
|                          | `SPELL_AURA_APPLIED` / `SPELL_CAST_START` event MRT receives. Useful when   |
|                          | a Reminder / ExCD2 trigger is not firing.                                    |
| `/mrtcleu off`           | Disable the sniffer and print a summary of received events.                 |
| `/mrtcleu status`        | Show whether MRT's CLEU dispatcher and CLEU modules are correctly wired.    |
| `/rt profiler`           | Open the CPU profiler window — sortable by module / function CPU + memory. |

There is also a Reminder-side debug toggle (Options →
Reminder → "Debug mode") that enables verbose logging of trigger
evaluation. Pair it with `MRT_VERBOSE_DEBUG = true` in chat
(`/run MRT_VERBOSE_DEBUG = true`) to also surface `DebugPrint(...)`
output from the rest of the addon.

If MRT looks dead after a `/reload`, the first thing to check is:

```
/run print(MRT and "ok" or "no MRT", MRT and MRT.A and "modules: "..tostring((function() local n=0 for _ in pairs(MRT.A) do n=n+1 end return n end)()) or "?")
```

That confirms whether the addon loaded at all and how many modules
registered.

---

## Reporting bugs

When something goes wrong, **please open a GitHub issue** at
<https://github.com/ExoJdi/MRT/issues> with:

1. **A clear description** of what happened and what you expected
   to happen.
2. **Reproduction steps** — exact slash commands, the boss / phase
   you were in, any other addons that were involved, whether you
   were on the Awesome WotLK patched client or vanilla 3.3.5a.
3. **Screenshots** of the bug, the chat error (if any), and the
   relevant settings tab in MRT options.
4. **The Lua error stack**, if you got one — copy the full
   `BugSack` / `!ImprovedErrorFrame` text.
5. (optional but very helpful) The output of `/mrtcleu status`
   captured during the bug.

The smaller and more reproducible the report, the faster the fix.

---

## Donate / Support

If this backport saved your raid night, you can drop a thank-you /
buy the backporter a coffee here:

**[boosty.to/exojdi](https://boosty.to/exojdi)**

Cheers, and good luck on your pulls. — *exojdi*
