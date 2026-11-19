# Interesting-Portraits-Pack

Starsector mod (game `0.98.5a-RC6`), data only: it adds 375 portraits to the vanilla factions and the player through `data/world/factions/*.faction`. There is no Java code, no mod plugin and no save data.

## Required workflow

These instructions apply to all coding agents. App-specific tools are scoped below.

Reviews, explanations, audits, and proposals are read-only unless the user requests changes. For implementation, finish the authorized edit, commit, validation, pull request, and merge steps. Use reasonable assumptions for routine details; ask when a missing decision would materially change the scope or behavior. A request to stop or propose only overrides the implementation workflow.

### Read the current documentation

1. Read the current remote `CLAUDE.md` in full at the start of each new task. Within the same task, reuse the version already read unless the instructions change. Read new or changed instruction files before following them.
2. Use the `starsector-knowledge` skill before answering or changing anything related to Starsector. It covers one exact game version. Do not rely on memory.
   - Load the `house-style` skill for every task. Its personal rules (answers, vocabulary, comments, commits, punctuation) take precedence over older wording in this repository.
3. The mod has no `lib/` and needs none.
4. Select the required guides using [Which guide to read](#which-guide-to-read). Apply every matching row, including for reviews and fixes to existing behavior.

### Which guide to read

| Task | Required reading / order |
|---|---|
| Add, remove or move portraits, or trace where a portrait shows up | [ARCHITECTURE.md](docs/ARCHITECTURE.md), then the vanilla `.faction` file in the skill's `data/world/factions/`. |
| Update for a new game version | [ARCHITECTURE.md](docs/ARCHITECTURE.md#player-faction), then the skill's vanilla `player.faction` and the `portraits` loader in `obf/loading/SpecStore.java`. |
| Write or revise player-facing text (mod description, changelog) | [CHANGELOG.md](docs/CHANGELOG.md#wording) |
| Edit a guide or add documentation | [Documentation upkeep](#documentation-upkeep), the owning guide and its inbound links. |
| Change the release tooling or prepare a release | [RELEASE.md](docs/RELEASE.md) |
| Any implementation task | [CHANGELOG.md](docs/CHANGELOG.md): add or amend the task's entries in the current unreleased section before merge. |

### Technical references and conflicts

[ARCHITECTURE.md](docs/ARCHITECTURE.md) is the data map and holds the engine facts the mod depends on; [RELEASE.md](docs/RELEASE.md) owns versions, the release tooling and publishing. Resolve a technical conflict against the exact source (repository data or the `starsector-knowledge` skill) and record the evidence.

### Make and record changes

- Work on `dev` for now: branch from `dev` and open pull requests into `dev`. `master` holds released states and the online version file; it is updated from `dev` only for a release.
- Use one task branch for the user's message.
- Make one commit per requested change. If one message contains several changes, commit them separately in the requested order.
- Commit messages are as short as possible: all lowercase, at most 8 words, programmer speak (`add hegemony portraits`). One line only. No body, no conventional-commit prefix, no testing notes, agent or model names, co-author trailers, session metadata, links or trailing full stop.
- Update the affected documentation in the same commit, following [Documentation upkeep](#documentation-upkeep). Do this without a separate user request.
- Update `Changelog` in the same commit, using [CHANGELOG.md](docs/CHANGELOG.md).
- Bump versions and publish a release only when the user explicitly asks; follow [RELEASE.md](docs/RELEASE.md).
- Work only in the current task checkout. Live game installations and unrelated checkouts are read-only unless the user explicitly asks you to change them.
- Do not deploy or synchronize after merge unless the user explicitly requests it.
- The mod stores nothing in saves. Keep it that way; say in the PR if a change would put anything in sector memory.
- Do not rename portraits or change the mod id unless asked: generated people in existing saves keep the old path.

### Validate and merge

- There is no compile gate: the mod has no source.
- A change to anything under `data/` or `graphics/`, `mod_info.json` or the version file must pass the release checks and a package preview on the exact final task-branch revision (commands in [RELEASE.md](docs/RELEASE.md#package)). After a faction file change, check that every listed portrait exists and every portrait is listed (one-liner in [ARCHITECTURE.md](docs/ARCHITECTURE.md#faction-files)).
- Documentation-only changes need no checks.
- No CI runs on pushes or pull requests. The release workflow runs only when started by hand on request; do not add triggers or check workflows.
- Open a pull request into `dev` and merge it when the work is complete. Do not leave finished work on an unmerged branch.
- Use the connected GitHub app to create, inspect, and merge pull requests.
- Fetch current remote `dev` before branching and again before merging. Merge intervening changes, push the exact final commit, and verify the merged remote revision.

### Branch cleanup

The repository does not delete merged branches automatically. Delete the task branch after merge. If deletion returns HTTP 403, report the branch name and leave it in place.

After merge, fast-forward the local task branch to the new `dev`.

## Documentation upkeep

Update documents automatically as part of each relevant change. Before committing, check which entries the diff makes stale and update those entries in that commit.

| Document | Owns | Update when / how |
|---|---|---|
| `AGENTS.md` | Discovery for other agents | Keep it a short pointer to this file. Do not copy workflow policy into it. |
| `CLAUDE.md` | Task scope, workflow, commits, PRs, checks, document maintenance | Replace changed policies here and update links. |
| `docs/ARCHITECTURE.md` | Data map: faction files, portrait counts, engine facts | Update when a faction file, portrait count or engine fact changes. No release history or duplicate policy. |
| `docs/RELEASE.md` | Version numbers, release tooling, package contents and publishing | Keep the workflow, the tool constants and the version file aligned. |
| `Changelog` and `docs/CHANGELOG.md` | Shipped change notes and how agents write them | Add or amend unreleased entries with each implementation task; preserve published entries. |

Give each fact one home. Move useful detail to its proper owner and link to it.

Write documentation in ordinary English. Use direct verbs and concrete nouns. Keep identifiers exact. Describe the current state, not history.

## Answers and wording

- Keep answers short and succinct: lead with the result, no preamble or recap. Detailed evidence goes in the pull request.
- Programming-related text (commits, PRs, comments, identifiers, technical docs) uses the vocabulary programmers have used for the last 20 years. Prefer the old word: use, fix, add, show; not leverage, address, introduce, surface.
- No em dashes and no hyphen used as a dash. No emojis unless topical.

## Comments

- Default to no comments. Add one only where it explains what is not apparent: engine quirks, required ordering, units, cross-file coupling.
- Comments are minimal, all lowercase, terse programmer speak, one line where possible. Identifiers keep their exact spelling.
- Leave existing comments you are not touching alone, including the `#` comments in `mod_info.json`.

## Agent behavior

- Keep the user's selected model. The main agent owns planning, merging and shipped changes. Delegate bounded research only when authorized, using the `researcher` agent (`.claude/agents/researcher.md`).
- Give brief progress updates and concise final results. Put detailed evidence in the pull request.

## Version names

Versions use major.minor.letter. See [RELEASE.md](docs/RELEASE.md#versions); do not bump versions or publish tags as part of ordinary task work.
