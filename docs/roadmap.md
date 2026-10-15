# OGF Roadmap

This is a 12-month plan from the JS-first pivot (~2026-05) to a public release with broad genre support. Estimates are rough and the order of milestones inside a phase isn't strict — find what works.

If you're reading this and OGF doesn't look like this map, the map is wrong; update it.

> **Drift declared 2026-10-15 (owner):** the Phase 0 docs below drifted from the project board. This section is kept as written history; the board is the source of truth until Phase 0 is re-synced.

## Owner decisions 2026-10-15

Decisions taken by the owner (Rylee) on 2026-10-15, after review of the docs against the project board. Rows below carry the markers; nothing here deletes roadmap work.

- **Item 5 — Phase 0 docs drifted from the board.** Outcome: (b) drift declared, roadmap updated to match the board. Phase 0 rows marked "drift declared 2026-10-15 (owner)".
- **Item 6 — GitHub issues are disabled for this repo.** Outcome: (a) gated on Phase 5. Note added: "issues disabled; enable at Phase 5 (owner decision 2026-10-15)". Nothing was filed or opened as part of this decision.
- **Item 7 — Phases 2-4 and Tier C.** Outcome: (a) parked behind Phase 1. Rows marked "parked behind Phase 1 (owner decision 2026-10-15)". Rows are kept, not deleted.

## Vision

> **Bolt.new for game dev.** Open browser, chat with an agent, ship a 2D game. No engine install, no boilerplate setup, drag-edit anything you don't want to describe in words. Export to web, itch.io, or Steam (via wrapper).

The two questions every roadmap milestone has to answer:
1. Does this expand the set of **games the agent can build**?
2. Does this expand the set of **things the user can edit visually**?

Answer at least one, ideally both.

## Phase 0 — Web-first reposition (now, ~1 week)

**Status: in progress on `feature/js-first`** — drift declared 2026-10-15 (owner).

- README + docs declare web as primary, Godot as second-class — drift declared 2026-10-15 (owner)
- Default engine flips from Godot to Web in the New Project flow — drift declared 2026-10-15 (owner)
- `docs/architecture.md` written so newcomers understand the agent-first paradigm — drift declared 2026-10-15 (owner)
- `docs/genre-support.md` started as a living matrix — drift declared 2026-10-15 (owner)
- Convention / spec template default audience = web games — drift declared 2026-10-15 (owner)

This phase is mostly text: positioning, defaults, direction. No SceneModel changes yet.

## Phase 1 — Phaser as default runtime + OGF Schema v1 (~3-5 weeks)

**The largest single phase.** Lock in the runtime, write the schema, prove it works on three sample genres.

> GitHub issues are disabled for this repo — issues disabled; enable at Phase 5 (owner decision 2026-10-15). Until then, track Phase 1 work in this doc.

Deliverables:

1. **Pick + lock framework: Phaser 3** (TBD: re-validate with a 30-min spike if you want — see `docs/architecture.md` Design Principle #3 for why we go schema-first not framework-first)
2. **OGF Game Schema v1** — JSON shape the agent must produce, with these top-level entities:
   - `level` — viewport, camera mode, layers
   - `layer` — bg / parallax / midground / foreground / props / colliders / hud
   - `actor` — sprite + animation refs + spawn position
   - `prop` — single sprite + transform + collider (optional)
   - `tilemap-layer` — tileset ref + cell grid
   - `path` — point sequence (paths for TD enemies, Bezier curves later)
   - `zone` — area + behavior trigger
   - `audio-cue` — sfx hook + music loop
3. **Phaser loader** (`packages/web-runtime/loaders/phaser.ts`) — `OgfLevelJson → Phaser.Scene`
4. **Schema-aware SceneModel** in `packages/contracts/src/scene.ts` — extend with layers, tilemap, animation refs
5. **3 sample projects**, one per Tier A genre (see `genre-support.md`)
   - `samples/td-mvp/` — proves schema works for tower defense (regression test for current OGF behavior)
   - `samples/platformer-mvp/` — proves parallax layers work
   - `samples/arena-mvp/` — proves top-down survivors-like works
6. **Convention rewrite**: `WEB` template in `apps/daemon/src/templates/conventions.ts` switches from "vanilla JS + Canvas" to "Phaser + OGF schema". Old vanilla JS conventions kept as a comment block for reference but not active.

## Phase 2 — Tier A editor parity (~3-5 weeks)

**Parked behind Phase 1 (owner decision 2026-10-15).** Rows kept, not deleted; not scheduled until Phase 1 lands.

Run each Tier A genre end-to-end. For each genre, fill out a "demo gauntlet" table:

```
✅ Spec ran clean
✅ Assets generated
[?] Editor render — ?
[?] Drag editing — ?
[?] Play runs — ?
[?] Export works — ?
```

Whichever rows show ❌ are the work for this phase. Likely items:

- Layer toggle in scene editor (show/hide each parallax layer)
- Tilemap brush v1 (paint single-tile changes; not a full tile editor yet)
- Animation preview in canvas (cycle through sprite frames at FPS)
- Per-genre prop schema validation (e.g. TD needs path slots, platformer needs platform colliders)

End state: all three Tier A genres have a green row across spec / assets / render / drag / play / export.

## Phase 3 — Tier B genres (~6-8 weeks)

**Parked behind Phase 1 (owner decision 2026-10-15).** Rows kept, not deleted; not scheduled until Phase 1 lands.

RPG, roguelike, puzzle. Each one will reveal new SceneModel gaps. Repeat the gauntlet pattern.

Likely structural extensions:

- Scene-to-scene navigation (roguelike room graph)
- NPC dialog editor (RPG)
- Grid / board state model (puzzle)
- More tilemap features (autotile, layered tilemaps)

This phase is the riskiest in scope — it's where OGF's "fit all genres" claim will either prove out or hit hard limits. Be willing to declare some Tier B genres "agent-only, editor-trust-mode" if the editor work is too big.

## Phase 4 — Distribution (~3-4 weeks)

**Parked behind Phase 1 (owner decision 2026-10-15).** Rows kept, not deleted; not scheduled until Phase 1 lands.

Make OGF games shareable.

- "Play in browser" tab polish (already partial in `PlayPane.tsx`)
- "Export standalone HTML" — single self-contained HTML file with embedded assets, drop on itch.io
- "Export to Steam" — Tauri wrapper that produces a Steamworks-compatible binary (~10MB shell over the web app)
- "Export PWA" — install-to-home-screen for mobile sharing
- Sample projects packaged as one-click templates in the New Project flow

End state: a non-developer user can make a game, click Export, and ship to itch.io / Steam / web in 5 minutes.

## Phase 5 — Public release + community (~6-8 weeks)

- Public landing page (replace this docs/ with proper marketing)
- Tutorial videos / GIFs
- Open the GitHub repo for issues — issues disabled; enable at Phase 5 (owner decision 2026-10-15)
- Asset library / template gallery
- Genre community: invite contributions for Tier C genres

## Tier C — Long tail (whenever)

**Parked behind Phase 1 (owner decision 2026-10-15).** Rows kept, not deleted.

- Fighting games (frame data editor)
- Tactics / grid games (cover, range, threat overlay)
- Idle / clicker (UI-heavy, mostly DOM)
- Rhythm / music games (probably "agent-only" — timing is too precise to drag-edit)

Some of these may never get full editor support. That's fine — agent-only is a valid product level (see Architecture Design Principle #4).

## What we won't do

- 3D / VR / AR — out of scope, use Unity / Godot
- Real-time multiplayer netcode at engine level — possibly Phase 5+ as a Colyseus / Geckos.io plugin, not core
- Asset marketplace (paid asset store)
- Mobile-native authoring (OGF stays browser-based)
- Owned game engine (use Phaser; build schema + adapters, not engine itself)

## Out of phase: ongoing maintenance

Work that's always happening regardless of phase:

- Bug fixes on existing OGF features (regen flow, image_gen watcher, file tree refresh, etc.)
- Convention tightening as we observe agent failure modes
- Performance / DX polish on the editor
- Daemon protocol improvements (tracking new Codex CLI versions)

## How to track progress

This doc is the high-level map. Specific tasks live in:
- `docs/genre-support.md` — what works per-genre
- GitHub issues (when public)
- Per-phase commit messages on `feature/js-first`

When a phase completes, update this doc + cut a tag (`phase-1-done`, etc.).
