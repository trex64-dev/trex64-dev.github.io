# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static technical overview site for **TRX64**, a project-centered C64 engineering environment:

- **C64RE** analyzes C64 media and produces verified source, a persistent typed 6502 graph, semantic knowledge, impact analysis, and evidence (`github.com/Jondalar/C64ReverseEngineeringMCP`).
- **TRX64 Runtime** supplies reversible C64/1541/1581 software execution and runtime evidence through JSON-RPC (`github.com/Jondalar/TRX64`). Use “TRX64 Runtime” on the site so it is not confused with the umbrella.
- **TRX64 Ultimate** runs the native C64 runtime on physical C64 Ultimate / Ultimate 64 Elite II hardware.
- **TRX64 MultiCore** is the technical deployment path: persistent Ultimate host, partial and full-device machine-core loading, and a common core application. The repository remains `github.com/Jondalar/TRX64U-MultiCore`; use the product mark “TRX64 MultiCore” in public copy.
- **C64U / UE2 Emulator** is the software Ultimate target: unmodified firmware, UCI, C64U-specific facilities, Made for Ultimate development, automation, and CI (`github.com/Jondalar/UE2-C64U-Emulator`).

This repo contains no C64 media and is not a C64RE project — it only describes those tools.

## Layout and build

- No build step, no package manager, no tests, no JavaScript. `dist/` is the deployed site as-is.
- Five pages: `dist/index.html` (system map), `dist/reverse-engineering.html`, `dist/fpga-platform.html`, `dist/examples.html` (project evidence), and `dist/documentation.html`. All share `dist/assets/site.css`; each page carries its own header/nav and footer markup — keep them in sync.
- `dist/assets/` — screenshots (`*.png`) and the self-hosted `Sixtyfour` display font (OFL license alongside; keep it).
- Deployment: GitHub Pages, repo `trex64-dev/trex64-dev.github.io` → https://trex64-dev.github.io. `.github/workflows/pages.yml` publishes `dist/` on every push to `main`.
- `.openai/hosting.json` — earlier Codex/ChatGPT preview hosting of `dist/`. Don't change `project_id`.
- Preview locally: `python3 -m http.server -d dist 8000`.

## Page structure

- Design tokens on `:root` (`--bg`, `--surface*`, `--ink`, `--muted`, `--line`, accent `--violet`/`--cyan`/`--coral`/`--amber`, fonts `--sans`/`--mono`/`--display`). Dark-only (`color-scheme: dark`). Reuse tokens; don't hardcode new colors.
- Responsive breakpoints are defined near each layout group; `prefers-reduced-motion` is handled.
- `index.html` contains the concise hero, the project-centered `#ecosystem` map, and the four destination links.
- `reverse-engineering.html` documents C64RE, the LLM/MCP responsibility split, the 6502 graph, the five-stage workflow, and TRX64 Runtime validation.
- `fpga-platform.html` documents private-development status, the MultiCore loading architecture, TRX64 Ultimate, UE2, and unsupported example cores.
- `documentation.html` is the component and repository index.
- `examples.html`: one `.case` section per capability hook, each backed by one project (`#engine` Wasteland, `#easyflash` Accolade Comics, `#bdt` Last Ninja engine, `#graph` Neuromancer, `#port` Codename Droid). Screenshots in `dist/assets/examples/`. Figures and code excerpts are verified against the project files, not taken from session reports.
- The visual language assigns cyan to analysis/C64RE, violet to runtime/host infrastructure, and coral to FPGA/Ultimate target content.
- Section headings use numbered `section-index` labels (`01 / …`, `02 / …`); keep numbering in sync when adding/removing sections.

## Content rules

- Style: "Boeing Engineering Manual". Subject, function, interface, result. No sentence without a clear, checkable statement; no marketing language, no repeated claims (each technical statement appears once). All copy in English.
- Role boundaries: TRX64 Runtime runs the C64 and supplies runtime evidence but does not own the analysis pipeline. C64RE owns analysis and evidence and contains no emulator. MultiCore owns host/core FPGA integration. TRX64 Ultimate is the native C64 runtime on physical Ultimate hardware. UE2 is the software Ultimate target for firmware and Made for Ultimate development.
- MultiCore has three distinct start paths. A partial `.cor` replaces only `pb_l2` through ICAP while the installed host remains active and communicates through the versioned contract. A large `.cor` is a full-device bitstream started through IPROG; it carries the machine, a deliberate host copy, and the generic core application. A `.cfw` is external full firmware with its own application, not a MultiCore core. Never describe large cores as using the partial-core contract.
- Treat the current `TRX64U-MultiCore` README, `CORES.md`, and developer guides as the source of truth. The exported “TRX64 Ultimate MultiCore - Architecture manual” is a structural and visual reference dated 2026-11-02; its core list, status, counts, measurements, and build details may be stale.
- The umbrella name is **TRX64**, not TREX64. Keep the project at the center. Reverse engineering forms the inner environment; software execution and Ultimate hardware form the outer platform. MultiCore is a deployment mechanism, not a peer product domain.
- TRX64 Ultimate is the supported core. Other Commodore cores are experimental porting targets. Non-Commodore and console cores are unsupported board-capability showcases.
- C64RE workflow must match C64RE itself: 5 project stages containing a 7-phase per-artifact pipeline. Static first, runtime only confirms. The structural graph is built when the analysis report is imported (phase 3); semantic annotations form a separate human layer (phase 5).
- C64RE is LLM/API-first but embeds no LLM: Claude Code, Codex or another MCP harness drives it. Deterministic tools produce the disassembly; the LLM adds meaning and knowledge (annotations, evidence).
- Verify C64RE claims against its current source/doctrine, not against this page or a stale GitNexus index.
- Verify repository links and requirements against the upstream repositories before changing documentation.
- ROMs, firmware and third-party media are never distributed — keep that notice on the documentation page and in the footer.
