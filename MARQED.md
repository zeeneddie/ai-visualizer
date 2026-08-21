# MarQed fork — wat hier afwijkt van upstream

Fork van **[jaredrhod/ai-visualizer](https://github.com/jaredrhod/ai-visualizer)**
(AGPL-3.0-or-later). Alle eer voor het origineel gaat naar Jared
Rhodenizer. Fork-punt: tag `fork-point-a69dda8`.

## Code-wijzigingen: geen

Dit stuk werkte bij de eerste meting zonder één aanpassing, en dat is
zeldzaam genoeg om op te schrijven. De fork bestaat om P22 (fork-first
voor wat we zelf draaien) en om `zeeneddie/fullstack-agent` naar één
familie te laten wijzen — niet omdat er iets stuk was.

Upstream bijwerken: `git fetch upstream && git merge upstream/main`.

## Wat er gemeten is (2026-08-21)

Kale `python3 server.py`, geen installatie, geen afhankelijkheden:

| poort | uitkomst |
|---|---|
| `/config` | 200 — vier faces gevonden door `faces/` te scannen |
| `/` (galerij) | 200 |
| `/faces/board/index.html` | 200, 47.699 b |
| `/state` bij `.voice_state` = idle/listening/thinking/speaking | volgt alle vier |
| golfvorm ingespoten | `level=1.000`, 63/64 samples ≠ 0 |
| volledige keten met backtalk | gezicht volgt de echte stem, en zakt terug naar `idle` |

Eén waarneming die makkelijk voor een fout wordt aangezien: na
`set_state("idle")` meldt `/state` nog even `speaking`. Dat is de
zelfherstel-regel in backtalk's `feed_waveform` die "speaking" opnieuw
schrijft zolang er audio speelt. Binnen een seconde staat hij op `idle`.
Meet dus met geduld, niet met 300 ms.

## De bus

Drie bestandjes, en dat is de hele koppeling. Alles wat ze schrijft kan
de gezichten aansturen:

```
.voice_state        idle | listening | thinking | speaking
.voice_waveform     {"ts": ..., "samples": [64 floats]}
.voice_loading_pid  bestaat zolang de stem zijn denkgeluid speelt
```

## Licentie

Ongewijzigd: **AGPL-3.0-or-later**.
