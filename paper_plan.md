# Tabular MOMO Paper Plan

## Working Title

Tabular MOMO: Multi-Agent Orchestration for Multi-Source Observation in Exploratory QA over Data Lakes

## Current Thesis

Text-centric Agentic RAG treats retrieval as the bottleneck: fetch a passage, read it, continue. Exploratory QA over tabular data lakes behaves differently — each useful table requires a multi-turn commitment (schema inspection, file-handle resolution, SQL or code execution, parsing repair, evidence validation, and an explicit stopping decision). The bottleneck is **execution discipline**, not just retrieval.

**Tabular MOMO** is a contract-based runtime-control framework for exploratory tabular QA. A planner orchestrates without touching raw data tools; bounded search and inspect subagents do the dirty work under explicit contracts and return compact evidence with status, missing outputs, and retry recommendations. The planner never sees raw schemas, SQL errors, or row dumps — this is the **context firewall**.

## Framing

The paper reads as an architecture paper with implementation grounding and an honest experimental section. It does *not* claim numeric improvements over baselines until the result CSVs are produced. The qualitative contribution is the failure-mode taxonomy and the mapping from each failure mode to a MOMO mechanism.

## Naming Caveat

The implementation package is currently named `sana_evaluation/` because it grew alongside the SANA ablation framework. The runtime-control features described in this paper (`delegation`, `sprint`, `cot`, `results`) are the MOMO contribution; the package rename is queued as a follow-on refactor and does not affect experimental results.

## Contributions

1. A failure analysis of agentic RAG on tabular data lakes that distinguishes retrieval failures from execution-discipline failures.
2. **Tabular MOMO**, a contract-based multi-agent architecture: a planner with restricted tool surface plus bounded search and inspect subagents with explicit contracts.
3. A **context firewall** mechanism implemented via bounded subagent return schemas (compact evidence + missing-output reports; raw artifacts isolated).
4. Runtime-control ablations exposed as separable feature flags (`results`, `cot`, `sprint`, `delegation`) on top of an existing `DataLakeAgent` harness.
5. An evaluation harness supporting multi-axis ablations over search mode, planning mode, computation mode, and MOMO features under shared tasks, budgets, traces, and logs.

## Current Evidence Snapshot

No committed result CSVs exist for MOMO variants yet. The Experiments section reports the harness and metric definitions; numeric cells use `--` placeholders pending the result run. The qualitative payoff (Table 3, failure-mode → MOMO-mechanism mapping) is independent of pending numbers.

## Paper Outline

1. **Introduction** — Stopping-rule illusion vignette; preview MOMO; contributions; relation to SANA.
2. **Problem Setting** — Exploratory QA over data lakes; discovery vs. commitment axis; related work.
3. **Why Tabular Data Breaks Text-Centric Agentic RAG** — Six failure mechanics: multi-turn source commitment, stopping-rule absence, context pollution, file-handle errors, source drift, premature submit.
4. **Tabular MOMO** — Overview; search contract; inspect contract; source-handle and source-family guards; context firewall; companion control primitives (`results`, `cot`, `sprint`).
5. **Implementation** — Five extension hooks in `SanaDataLakeAgent`; baseline never modified; `SanaBatchRunner`; CLI axes; naming caveat.
6. **Experiments** — RQ1/RQ2/RQ3; four tables; honest framing.
7. **Discussion** — Execution discipline as the transferable lever; partial-success protocol generalization; delegation vs. sprint tradeoff.
8. **Limitations and Future Work** — Latency cost, manual budget calibration, source-family selection dependence, package rename, single benchmark family.
9. **Conclusion** — Recap.
10. **Appendix** — Contract schemas, runtime pseudocode, prompt blocks, flag matrix.

## Decisions to Lock

- **Workshop venue:** VLDB 2026 ADS (https://vldb-ads.top/#cfp). Final submission date TBD.
- **Model lineup:** match the SANA paper — `gpt-5-mini` and `gpt-5.4-nano`.
- **Benchmark:** LakeQA-derived multi-source public-data tasks (same as SANA).
- **Experiment scope:** RQ1 single-agent vs. delegation; RQ2 per-mechanism failure-mode reduction; RQ3 subagent-budget sensitivity.
- **Future-work framing:** package rename to `momo_evaluation/`, adaptive budget reallocation, cross-contract evidence reuse, typed contract DSL.

## Next Writing Sprint

1. Produce Table 1 (main comparison) numbers from the harness.
2. Code-trace at least 20 runs to populate Table 3 (failure taxonomy) with concrete trace signals.
3. Draft Figure 1 (architecture diagram) in draw.io.
4. Decide whether to fold the SANA paper's preloaded-vs-ideal diagnostic into Table 2 or omit (SANA already covers it).
5. Confirm whether the `acks{}` block needs grant attributions before submission.
