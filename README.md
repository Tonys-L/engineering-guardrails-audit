# Engineering Guardrails Audit

**[中文文档](README_zh.md)** | English

A meta-auditor skill for AI coding agents: it audits **the engineering defense net itself**, not your business code. It statically scans a target project's infrastructure, configs, and build scripts to determine whether the **6 physical defense tripwires** are present, then outputs a maturity level report with hardening guidance.

Stack-agnostic. Business-agnostic. Never runs your tests.

## Why

AI agents iterating on a codebase tend to take shortcuts — silently swallowed exceptions, unhandled async errors, cross-layer imports, fake-green tests. Documentation and conventions don't stop them. Only **programmatically enforced gates (Exit Code ≠ 0)** do.

This skill audits whether those gates actually exist — and whether they actually fire.

## Install

```bash
npx skills add Tonys-L/engineering-guardrails-audit
```

Works with Claude Code, Cursor, Codex, TRAE, and 75+ agents supported by the [skills CLI](https://skills.sh).

## The 6 Tripwires

| # | Tripwire | Guards against |
|---|---|---|
| 1 | Semantics & types | Strict-check erosion, unhandled async failures, type-assertion escape hatches |
| 2 | Architecture topology | Cross-layer imports, circular dependencies |
| 3 | Contract sync | Version SSOT, manifest ↔ implementation drift, i18n asymmetry |
| 4 | Quality anti-gaming | Empty catch/except, duplication threshold, coverage floor |
| 5 | Environment purity | Conflict markers, private absolute paths, artifact size budget |
| 6 | Anti fake-green | Roundtrip fidelity tests, direct-source imports, single aggregate gate |

## Maturity Levels

```
Level 0  Bare            → no programmatic protection
Level 1  Syntax          → typing tripwire in place
Level 2  Architecture    → + dependency topology
Level 3  Contract sync   → + SSOT & manifest checks
Level 4  Industrial      → all 6 tripwires, CI-enforced
```

Level is computed by a deterministic formula (longest qualifying segment, at most one degraded dimension) — same input, same result, every run.

## Three Distortion Modes It Detects

| Mode | Meaning |
|---|---|
| False safety | Docs claim a protection that config doesn't have |
| False accusation | Auditor reports a gap that was already fixed (stale evidence) |
| Vacuous gate | A gate that looks CI-enforced but its failure path is unreachable in CI |

## Evidence Grades

Every "armed" verdict carries a grade: `E1 CI merge-blocking > E2 local aggregate / git hooks > E3 manual > E4 config-only > E5 paper-only`.

## Usage

Ask your agent:

- "Audit this repo's guardrails"
- "Will this project survive AI-driven iteration?"
- "What guardrail level is this project?"

## License

[MIT](LICENSE)
