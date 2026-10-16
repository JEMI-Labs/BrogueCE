# AGENTS.md — BrogueCE (Darkhold reference fork)

<!-- Last reviewed: 2026-10-17 -->

JEMI-Labs fork of [tmewett/BrogueCE](https://github.com/tmewett/BrogueCE), kept as a **design-research reference for [Darkhold](https://github.com/JEMI-Labs/Darkhold)**: John plays the release build, and agents read the real source when Darkhold's genre-canon-first rule (`Darkhold/Design-Docs/DESIGN_PRINCIPLES.md` §1) asks "what does Brogue do?". **Scope is gameplay mechanics and systems patterns only** — Brogue's ASCII, terminal-era UI is not a reference for Darkhold's presentation or UX. Upstream's own docs (`README.md`, `BUILD.md`, `CONTRIBUTING.md`) still apply to the game itself.

## Ground rules

- **Read, never copy.** BrogueCE is AGPL-3.0; Darkhold is proprietary (all rights reserved). Study mechanics, numbers and structure and re-express them in Darkhold's own code. Never paste or port Brogue source into Darkhold.
- **Findings land in Darkhold, not here.** Design conclusions go in Darkhold's audit docs, `WORKING_DECISIONS_LOG.md` or `BACKLOG.md`, citing the Brogue file and function. This repo holds only the reference.
- **Keep `master` mergeable with upstream.** Our only additions are this file and `CLAUDE.md`. Experiments (debug prints, tweaked tables, instrumentation) go on a local branch, never on `master`. No PRs to upstream.

## Remotes and sync

- `origin` → `JEMI-Labs/BrogueCE` (push via the `github-personal` SSH alias); `upstream` → `tmewett/BrogueCE`.
- Sync: `git fetch upstream && git merge upstream/master && git push`.

## Installed build

- `/Applications/Brogue CE.app` — **v1.15.1, macOS arm64**, from upstream's release asset `BrogueCE-1.15.1-macos-arm64.zip` (SHA-256 `e95b9543…c56fe889d`), installed 2026-10-17. Ad-hoc signed, not notarised; installed without a quarantine flag, so it opens directly.
- Upgrade: download the newer `macos-arm64` asset from [Releases](https://github.com/tmewett/BrogueCE/releases) and replace the app.
- Saves, recordings and high scores: `~/Library/Application Support/Brogue/Brogue CE/`. Recordings (`.broguerec`) replay a whole run — useful evidence to cite in a Darkhold playtest note.
- Building from source (only needed for instrumented experiments): see `BUILD.md`; needs SDL2 + SDL2_image from Homebrew.

## Where to look in the source

| Question | File (`src/brogue/`) |
|---|---|
| Level generation, rooms, lakes, machines/vaults | `Architect.c` |
| Turn order, speed, regeneration, hunger, light/stealth ticks | `Time.c` |
| Hit, damage, accuracy, sneak attacks | `Combat.c` |
| Monster AI, flee/follow, allies, spawning | `Monsters.c` |
| Items, enchanting, identification, wand/staff effects | `Items.c` |
| Stat-scaling formulas (enchant → effect) | `PowerTables.c` |
| Monster/item/terrain/dungeon-feature tables, depth spawn curves | `src/variants/GlobalsBrogue.c` |
| Pathing and distance maps | `Dijkstra.c`, `Movement.c` |
| Lighting and visibility | `Light.c` |

`src/variants/` also holds Rapid Brogue and Bullet Brogue — shorter dungeons tuned from the same tables, handy for pacing comparisons.
