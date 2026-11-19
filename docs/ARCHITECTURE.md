# Architecture

Data map for the current implementation. Paths are repository-relative. The mod has no Java code and no mod plugin; everything it does comes from files the game merges at load.

| Reference | Scope |
|---|---|
| [CLAUDE.md](../CLAUDE.md) | Workflow, checks and document upkeep |
| [RELEASE.md](RELEASE.md) | Version numbers, release tooling, package contents and publishing |
| [CHANGELOG.md](CHANGELOG.md) | Change-note format and upkeep for every implementation task |

## Start here

| Change / symptom | Route |
|---|---|
| Add, remove or move a portrait | `graphics/portraits/`, then the faction file that lists it; [Faction files](#faction-files) |
| Portraits missing from the new game picker | `data/world/factions/player.faction`; [Player faction](#player-faction) |
| Mod name, id, version, description | `mod_info.json` (the version is stamped at packaging, see [RELEASE.md](RELEASE.md)) |
| Version Checker | `InterestingPortraitsPack.version`, registered in `data/config/version/version_files.csv`; owned by [RELEASE.md](RELEASE.md) |

## Data files

| File | What it defines |
|---|---|
| `mod_info.json` | Id `interestingportraitspack`, name `Interesting Portraits Pack`, game `0.9.1a`. No `jars`, no `modPlugin`, no dependencies, not marked `utility`. Uses `#` comments, which the game's JSON loader accepts |
| `graphics/portraits/interesting_portrait_<group><nn>.png` | 375 portraits, 128x128. `<group>` is the faction the art was made for (`hegemony`, `league`, `diktat`, `luddicpath`, `ai` and so on), not a game faction id |
| `data/world/factions/<faction id>.faction` | One file per vanilla faction, each holding only a `portraits` block |
| `data/config/version/version_files.csv` | Registers `InterestingPortraitsPack.version` for Version Checker |

## Faction files

- The game deep-merges a mod's `.faction` JSON into the vanilla file and appends arrays, so every path listed here is added to the vanilla pool. `"core_clearArray"` as a first element would empty the vanilla list first; the mod does not use it.
- Every faction file uses `standard_male` and `standard_female`. The game loads these into the faction's random person picker (`Faction.createRandomPerson`), used for officers, administrators, contacts and other generated people of that faction.
- Every listed path must exist under `graphics/portraits/`, and every portrait should be listed at least once. The release tool fails on a listed portrait that is not packaged.
- Check after an edit (both lists print nothing when the files agree):

```text
grep -oh 'graphics/[^"]*' data/world/factions/*.faction | sort -u > /tmp/refs
git ls-files graphics | sort > /tmp/files
comm -23 /tmp/refs /tmp/files; comm -13 /tmp/refs /tmp/files
```

- Gender is the list a portrait sits in; nothing in the file name says it.
- Files are CRLF with no newline at the end. Keep that, so diffs stay small.

| Faction file | Male | Female |
|---|---|---|
| `hegemony.faction` | 16 | 18 |
| `independent.faction` | 31 | 21 |
| `knights_of_ludd.faction` | 4 | 3 |
| `lions_guard.faction` | 2 | 2 |
| `luddic_church.faction` | 23 | 20 |
| `luddic_path.faction` | 15 | 13 |
| `persean_league.faction` | 22 | 20 |
| `pirates.faction` | 24 | 20 |
| `poor.faction` | 14 | 11 |
| `remnants.faction` | 3 | 3 |
| `scavengers.faction` | 14 | 11 |
| `sindrian_diktat.faction` | 21 | 20 |
| `sleeper.faction` | 4 | 7 |
| `tritachyon.faction` | 21 | 20 |

`remnants.faction` lists the three `ai` portraits under both genders.

## Player faction

`player.faction` lists nearly every portrait of the pack (197 male, 175 female) under `standard_male` and `standard_female`.

## Save data

The mod writes nothing to saves. People generated with one of its portraits store the path (`PersonAPI.setPortraitSprite`), so removing the mod or renaming a portrait leaves them pointing at a missing file in existing saves (what the game then shows is not checked).
