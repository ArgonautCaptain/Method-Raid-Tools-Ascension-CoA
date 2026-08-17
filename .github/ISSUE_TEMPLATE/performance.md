---
name: Performance issue
about: Report freezes, lag, FPS drops or memory crashes
title: "[PERF] "
labels: ''
assignees: ''

---

## Type of issue
- [ ] Freezes when opening the addon window
- [ ] Lag when moving / resizing windows
- [ ] FPS drop in combat
- [ ] Memory accumulation (OOM crash after several fights)
- [ ] Slow addon load on world enter
- [ ] Other: 

## When does it happen
Describe exactly when the issue occurs:
- Which module (Note, Reminder, Raid Cooldowns, BossWatcher, etc.)
- Which action (open, drag, reload)
- Which situation (solo / 5-man / 10-man / 25-man / arena / BG)

## Environment
- **WoW client version:** (e.g. 3.3.5a build 12340)
- **Server:** 
- **MRT version:** 
- **OS / FPS before the issue:** (Windows 10, 60 FPS)
- **Group size:** (1 / 5 / 10 / 25)

## Profiling (if available)
Enable the built-in MRT profiler and paste the output:
```
/rt profiler
-- wait 30 seconds under load
/rt profiler stats
```

Paste the result:
```
-- /rt profiler stats output here
```

## Memory usage
Run the command and paste the result:
```
/run print(format("MRT memory: %.2f MB", collectgarbage("count")/1024))
```

```
-- result here
```

## Additional context
- When did the issue first appear? (after an update / always)
- Does it reproduce with minimal addons (MRT only)?
- Does `/reload` help temporarily?
- How many bosses / fights before the crash?
