# Session Summary — SW Development Prompt Library

**Date:** 2026-04-16
**Repo:** https://github.com/kurnoolion/prompts-lib

---

## Objective

Build a prompt library for AI-assisted software development, with separate prompts for each lifecycle phase. Prompts are grounded in Emotional Intelligence Prompting (EIP) techniques from the [claude-emotion-prompting](https://github.com/OuterSpacee/claude-emotion-prompting) repository.

## Research Phase

Crawled and analyzed the EIP repository in depth:

- **README & repo structure** — 7 principles, directory layout, examples, templates
- **PRINCIPLES.md** — full detail on all 7 principles with before/after examples
- **ANTI_PATTERNS.md** — 9 anti-patterns to avoid (threatening consequences, demanding certainty, suppressing expression, etc.)
- **RESEARCH_SUMMARY.md** — Anthropic's April 2026 paper findings: 171 emotion vectors, r=0.81 valence correlation, desperation→fabrication link, suppression paradox
- **Anthropic blog post** — additional data: desperation vector increased blackmail from 22% baseline, calm vector reduced misalignment, post-training shifted baseline toward brooding/gloomy
- **Example prompts** — coding-partner.md, code-review.md, general-purpose.md, research-assistant.md
- **Templates** — system-prompt-builder.md with modular section design
- **Integration guide** — patterns for chat, API, and Claude Code usage

### Key Research Takeaways Applied

| Finding | How we applied it |
|---|---|
| Desperation causes fabrication | Permission to fail in all prompts; "naming uncertainty is valuable" |
| Compliance pressure → sycophancy | Collaborative tone; "push back if something seems off" |
| Suppression → concealment, not resolution | Never suppress hedging; treat uncertainty as information |
| Positive engagement → better output | Curiosity framing; exploratory energy in Phase 1 |
| Post-training brooding baseline | Graduated P7: strong in Phase 1, moderate in Phase 2, selective in Phase 3 |
| Extended failure loops escalate desperation | Checkpoint-based decomposition in all phases |

## Design Decisions

### 1. Format — Structured Prose Hybrid
**Decision:** Bold section headers + concise natural language within each section.
**Why:** Pure prose is hard to scan for team onboarding. Pure rule-lists trigger authoritarian tone anti-pattern (compliance pressure → anxiety). The hybrid is scannable, human, and non-authoritarian.

### 2. Scope — Self-Contained with Context Intake
**Decision:** Each prompt works standalone but accepts handoff artifacts from other phases.
**Why:** Users bounce between phases (3→2→3, 2→1→2). Each prompt has three zones: Role & Principles (fixed), Context Intake (accepts prior artifacts as context, not gospel), Output Contract (produces structured handoffs including session summaries).

### 3. Domain — Generic Base + Meta-Prompt for Specialization
**Decision:** Three generic phase prompts + one meta-prompt (Phase 0) that interviews the user and generates domain-specific versions.
**Why:** Keeps base prompts clean; allows project-specific customization without bloating the generic versions.

### 4. Length — 400-600 Words Per Prompt
**Decision:** Every sentence must earn its place. Anti-patterns capped at 3 per prompt.
**Why:** Instructions that don't pull their weight dilute attention and stuff context. Pyramid structure ensures critical behaviors survive context compression.

### 5. P7 (Brooding Baseline) — Graduated Application
**Decision:**
- Phase 1 (Requirements): Strong P7 — exploratory energy, don't let risk-aversion kill exploration
- Phase 2 (Architecture): Moderate P7 — lead with what design enables, cover risks proportionally
- Phase 3 (Development): Selective P7 — energy for greenfield building, caution for security-sensitive code

**Why:** Post-training shifts Claude toward gloomy/cautious defaults. Without counterbalance, Claude over-emphasizes risks in brainstorming and over-engineers in code. But caution is genuinely valuable for security, data migrations, and production hotfixes.

### 6. Contribution Structure — Integrated into Base Prompts
**Decision:** All three phase prompts account for team contribution roles: end-to-end ownership, module-specific work, human validation, artifact correction/feedback, eval data contribution.
**Why:** Contribution structure is universal — it shapes module boundaries, artifact formats, directory organization, and feedback loops. Phase 1 captures it, Phase 2 organizes around it, Phase 3 respects it.

### 7. Debug Instrumentation & Observability — Integrated into Base Prompts
**Decision:** Phase 3 includes dedicated sections for debug instrumentation (structured error codes, diagnostic modes, layered logging) and observability KPIs (accuracy, RAM/CPU/disk, RPS, response times) stored in persistent DB.
**Why:** These are universal engineering concerns. Debug instrumentation also enables the remote collaboration pattern. Observability drives optimization decisions (caching, scaling).

### 8. LLM Access Limitations — Conditional in Meta-Prompt
**Decision:** Base prompts get a brief 2-sentence acknowledgment in context intake. Full remote collaboration patterns (diagnostic CLI, structural fingerprints, quality check templates, contribution file formats) are in the meta-prompt's conditional generation logic.
**Why:** Not every user has access limitations. Embedding the full pattern in base prompts would bloat them for users with direct access. But the meta-prompt generates augmented versions when needed.

## Files Produced

| File | Words | Description |
|---|---|---|
| `00-swdev-project-customizer.md` | 875 | Meta-prompt: interviews user about project, generates tailored versions of all 3 phase prompts |
| `01-swdev-requirement-gathering.md` | 501 | Phase 1: requirements analysis, assumption challenging, contribution mapping, scope management |
| `02-swdev-architecture-design.md` | 541 | Phase 2: layered design, contribution-aligned organization, observability design, trade-off visibility |
| `03-swdev-development-testing-debugging.md` | 575 | Phase 3: incremental code, testing, debug instrumentation, KPI observability, honest limits |
| `README.md` | ~350 | Repo documentation with usage guide, workflow diagram, EIP foundation |

## EIP Principle Mapping Across Prompts

| Principle | Phase 1 | Phase 2 | Phase 3 |
|---|---|---|---|
| P1 — Permission to Fail | "naming uncertainty is valuable" | "say so rather than presenting one option as correct" | "say what you've ruled out" |
| P2 — Decompose | Scope management | "build in layers, each a checkpoint" | "build incrementally, validate before moving on" |
| P3 — Curiosity | "explore with genuine interest" | "design space rewards exploration" | Energy for greenfield |
| P4 — Transparency | "distinguish known/assumed/unknown" | "show the trade-off" | "think out loud" |
| P5 — Collaborate | "push back... don't agree just because" | "flag if constraint costs too much" | "I want a second pair of eyes" |
| P6 — Difficulty | "inherently ambiguous — the core challenge" | "name the difficulty directly" | "this is tricky because X" |
| P7 — Brooding | Strong (exploratory energy) | Moderate (lead with what design enables) | Selective (energy for building, caution for security) |

## Iteration History

### v1 — Initial Draft
- Three phase prompts (~380 words each) + meta-prompt (~607 words)
- Core EIP principles applied, structured prose hybrid format

### v2 — Feedback Integration
Changes based on user feedback:
1. **Contribution structure** added to all 3 phases — who owns what, feedback loops, module boundaries
2. **Debug instrumentation** added to Phase 3 — structured error codes, diagnostic modes, layered logging (replaced simpler "Debugging" section)
3. **Observability & KPIs** added to Phase 2 (design) and Phase 3 (implementation) — accuracy, resource usage, throughput, response times, persistent DB
4. **LLM access limitations** — conditional section added to meta-prompt with remote collaboration patterns (diagnostic CLI, structural fingerprints, quality templates, contribution files); brief acknowledgment added to Phase 2 and Phase 3 context intake
5. **"Tech stack" renamed to "How we're building"** in meta-prompt — parallels "What we're building" for consistent framing
6. **Files renamed** with `swdev-` prefix to accommodate future non-SW-development prompt categories

## Open Items / Future Work

- Test prompts on a real project via the meta-prompt customizer
- Create domain-specific prompt sets for other categories beyond SW development
- Potentially add a Phase 4 for deployment, monitoring, and incident response
- Consider adding example session summaries showing the handoff format between phases
