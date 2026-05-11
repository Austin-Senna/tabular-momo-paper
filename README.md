# Tabular MOMO Paper

VLDB ADS workshop paper workspace for **Tabular MOMO** (Multi-Agent Orchestration for Multi-Source Observation), a runtime-control framework for LLM agents performing exploratory question answering over tabular data lakes.

## Layout

- `main.tex` — VLDB workshop LaTeX entrypoint.
- `subsections/` — paper body, one file per section.
- `references.bib` — bibliography (seeded from the companion SANA paper).
- `figures/` — image assets used by the paper (currently placeholders).
- `paper_plan.md` — working paper plan, argument, figures, and next writing tasks.
- `figures.md` — figure plan.

## Build

From this directory:

```bash
tectonic main.tex
```

## Related work

This paper is one of two papers describing the implementation under `sana_evaluation/` from different angles. The companion paper, SANA, describes the same code as an *ablation framework*; this paper describes its *delegation architecture* as a runtime-control intervention. See `paper_plan.md` for the naming caveat.
