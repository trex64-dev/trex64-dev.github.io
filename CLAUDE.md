# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static marketing/overview site for **TREX64** — umbrella for three independent GPL-3.0-or-later projects:

- **TRX64** — reversible, cycle-accurate C64/1541/1581 runtime with JSON-RPC control (`github.com/Jondalar/TRX64`)
- **C64RE** — MCP server + workbench; persistent typed 6502 analysis graph, no emulator of its own, uses the TRX64 daemon only for runtime validation (`github.com/Jondalar/C64ReverseEngineeringMCP`)
- **C64U / UE2 Emulator** — Ultimate 64 Elite II / C64 Ultimate target environment: unmodified firmware on modeled board hardware backed by TRX64 (`github.com/Jondalar/UE2-C64U-Emulator`)

This repo contains no C64 media and is not a C64RE project — it only describes those tools.

## Layout and build

- No build step, no package manager, no tests, no JavaScript. `dist/` is the deployed site as-is.
- `dist/index.html` is the whole site: one file with all CSS inline in `<style>`. Edit it directly.
- `dist/assets/` — screenshots (`*.png`) and the self-hosted `Sixtyfour` display font (OFL license alongside; keep it).
- Deployment: GitHub Pages, repo `trex64-dev/trex64-dev.github.io` → https://trex64-dev.github.io. `.github/workflows/pages.yml` publishes `dist/` on every push to `main`.
- `.openai/hosting.json` — earlier Codex/ChatGPT preview hosting of `dist/`. Don't change `project_id`.
- Preview locally: `python3 -m http.server -d dist 8000`.

## Page structure (dist/index.html)

- Design tokens on `:root` (`--bg`, `--surface*`, `--ink`, `--muted`, `--line`, accent `--violet`/`--cyan`/`--coral`/`--amber`, fonts `--sans`/`--mono`/`--display`). Dark-only (`color-scheme: dark`). Reuse tokens; don't hardcode new colors.
- Responsive breakpoints at `max-width: 920px` and `640px`; `prefers-reduced-motion` handled.
- Sections, in order, anchored by id: hero + `#ecosystem` system map → `#api-first` → `#tools` (per-product cards + C64RE graph spec) → `#workflow` (5 project stages, 7 per-artifact analysis phases) → `#install` → footer.
- Each product has a consistent color/class suffix (`-trx`, `-c64re`, `-ue2`) reused across module, api-surface, and product blocks.
- Section headings use numbered `section-index` labels (`01 / …`, `02 / …`); keep numbering in sync when adding/removing sections.

## Content rules

- Style: "Boeing Engineering Manual". Subject, function, interface, result. No sentence without a clear, checkable statement; no marketing language, no repeated claims (each technical statement appears once). All copy in English.
- Role boundaries: TRX64 runs the C64 and supplies runtime evidence but does not decide hypotheses and does not own the analysis pipeline (it has disassembly features, not the C64RE process). C64RE owns analysis/evidence and contains no emulator. UE2 is the Ultimate target environment.
- C64RE workflow must match C64RE itself: 5 project stages containing a 7-phase per-artifact pipeline. Static first, runtime only confirms. The structural graph is built when the analysis report is imported (phase 3); semantic annotations form a separate human layer (phase 5).
- C64RE is LLM/API-first but embeds no LLM: Claude Code, Codex or another MCP harness drives it. Deterministic tools produce the disassembly; the LLM adds meaning and knowledge (annotations, evidence).
- Verify C64RE claims against its current source/doctrine, not against this page or a stale GitNexus index.
- Install commands in `#install` mirror the upstream READMEs; verify against those repos before changing them.
- ROMs, firmware and third-party media are never distributed — keep that notice in `#install` and the footer.
