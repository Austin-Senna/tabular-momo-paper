# Tabular MOMO Figures Plan

## Figure 1 — MOMO Architecture

**Purpose.** One-glance summary of the planner + search-subagent + inspect-subagent structure and the context firewall.

**Content.**
- Top: Planner box. Tool surface listed: `plan`, `final_answer`, `search_subagent(contract)`, `inspect_subagent(contract)`. Crossed out below: `peek_file`, `query_file`, `execute_code`, `download_file` (illustrating restriction).
- Middle: two parallel boxes, "Search Subagent" and "Inspect Subagent". Each shows its contract input fields (compressed list) and its return fields (status + compact payload).
- Inside each worker box: dashed cloud labeled "raw schemas / tracebacks / SQL errors / row dumps — discarded on return."
- Boundary line labeled **Context Firewall** between planner and workers.
- Arrows: planner → contract → worker; worker → compact return → planner.

**Format.** draw.io source committed alongside PNG export at `figures/fig1_momo_architecture.{drawio,png}`.

## Figure 2 — Contract Lifecycle

**Purpose.** Sequence-style view of a typical task: planner issues a search contract, receives candidates, issues one or more inspect contracts, receives answer fragments and missing outputs, decides whether to retry or finalize.

**Content.**
- Swim-lane diagram with three lanes: Planner / Search Subagent / Inspect Subagent.
- Calls: `search_subagent(SearchContract)` → return `{status: success, candidates: [...]}`.
- Then `inspect_subagent(InspectContract{source_family_ids=[s1]})` → return `{status: partial, answer_fragments: [...], missing_outputs: [2021]}`.
- Then `inspect_subagent(InspectContract{retry_of_contract_id=..., source_family_ids=[s2]})` → return `{status: success, answer_fragments: [...]}`.
- Finally `final_answer(...)`.

**Format.** draw.io source at `figures/fig2_contract_lifecycle.{drawio,png}`.

## Figure 3 — Failure-Mode → Mechanism Mapping

**Purpose.** Compact qualitative payoff figure tying each failure mode from §3 to the MOMO mechanism that addresses it.

**Content.** Two-column block diagram. Left column: eight failure-mode chips (over-searching, schema thrashing, file-handle errors, source drift, zero-row loop, context bloat, redundant verification, premature submit). Right column: five mechanism chips (bounded search subagent, bounded inspect subagent, `_FileReferenceGuard`, `_InspectSourceGuard`, context firewall via compact returns, partial-success protocol). Arrows from each failure mode to its addressing mechanism.

**Format.** draw.io source at `figures/fig3_failure_mode_mapping.{drawio,png}`.

## Optional Figure 4 — Subagent Budget Trade-off Plot

**Purpose.** If Table 4's numbers are produced before submission, render as a small line plot showing partial-rate, budget-exhausted-rate, and cost as a function of `max_inspect_subagent_calls`.

**Format.** Generated from `subagent_budget_results.csv` via a one-shot matplotlib script; commit the script alongside the PNG.

## Asset Conventions

- Source files in draw.io XML format committed alongside PNG/PDF exports.
- Embed in `main.tex` via `\includegraphics[width=\linewidth]{figures/<name>}`.
- Caption convention matches the SANA paper.
