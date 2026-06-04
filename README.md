# EA 12FEP Codebook — for LLM-Assisted Qualitative Analysis

[![License: CC BY-NC-ND 4.0](https://img.shields.io/badge/License-CC%20BY--NC--ND%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-nd/4.0/)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.20481464.svg)](https://doi.org/10.5281/zenodo.20481464)

A structured codebook for scoring participant testimony against Längle's **12 Fundamental Existential Prerequisites (12FEP)** framework, designed for use with Large Language Models (LLMs) as a research instrument.

Built by multi-LLM cross-validation against two primary Längle sources (2002, 2011b). Every code traces to a verbatim quote with citation. Designed to be downloaded, applied, extended, and forked.

## What this codebook is

A JSON artifact that constrains an LLM to score participant testimony on a 0–5 scale across the 12 Fundamental Existential Prerequisites, with required justification (testimony quote + Längle quote) for every score.

It is **not** a fine-tuned model. It is **not** an LLM prompt. It is a structured *constraint* — a reference that you provide to an LLM alongside testimony, so the LLM speaks Längle rather than generic prior knowledge.

For the methodology that produced this codebook, see the companion paper:
*Nelson-Zutter, G. (2026, June). "Do You Understand?!" Best Practices using Artificial Intelligence in Research (and Life). WCET4, Denver.* DOI: forthcoming.

## Repo layout

```
LICENSE                  CC BY-NC-ND 4.0
NOTICE.md                Längle source quotation clarification
README.md                this file
CITATION.cff             machine-readable citation metadata
.zenodo.json             Zenodo deposit metadata
codebook/
  ├── 12fep-master_GrahamNelsonZutter_CC-BY-NC-ND-4.0.json  the canonical MASTER (current)
  └── versions/          full development lineage (per-LLM, per-stage)
docs/
  └── lineage_GrahamNelsonZutter_CC-BY-NC-ND-4.0.md         how each version was produced (multi-LLM build story)
```

## How to use it

1. Download `codebook/12fep-master_GrahamNelsonZutter_CC-BY-NC-ND-4.0.json`.
2. Provide it to your LLM of choice alongside your participant testimony (CSV).
3. The codebook's `instructions_for_llm` block tells the LLM how to score (0–5 scale, quote-anchored justification required).
4. The LLM returns scores with justifications; you (the researcher) verify the quote provenance manually before publishing.

The methodology paper above describes the full workflow including multi-LLM cross-validation (Technique A) and the 5-pass consensus protocol (Technique C).

## How it was built

Three LLMs (ChatGPT 5 Thinking, Gemini 2.5 Turbo, Claude Sonnet 4.5) independently produced v1 codebooks from the same two Längle sources. Each LLM then merged all three v1s into its own merged-v1. The MASTER is the result of an LLM jury vote (2/3 chose Claude's version). See [`docs/lineage_GrahamNelsonZutter_CC-BY-NC-ND-4.0.md`](docs/lineage_GrahamNelsonZutter_CC-BY-NC-ND-4.0.md).

## How to cite

Use `CITATION.cff` (GitHub renders this automatically as a citation widget on the repo homepage) or:

> Nelson-Zutter, G. (2026). *EA 12FEP Codebook for LLM-Assisted Qualitative Analysis* [Data set]. Zenodo. https://doi.org/10.5281/zenodo.20481464

For a specific version, use the version DOI (e.g. v1.0.2: `10.5281/zenodo.20481519`); for the artifact in general, use the concept DOI (`10.5281/zenodo.20481464`).

## Versioning

Each substantive release is a tagged GitHub release. GitHub-→-Zenodo integration archives each release and mints a version DOI. The concept DOI represents the artifact across all versions.

**Important:** the codebook's version lineage is *not* a linear edit chain — it's a multi-LLM consensus process. See [`docs/lineage_GrahamNelsonZutter_CC-BY-NC-ND-4.0.md`](docs/lineage_GrahamNelsonZutter_CC-BY-NC-ND-4.0.md) for the rationale and methodology.

## Contributing

Issues, discussions, and PRs welcome. The intended development workflow:

- **Issues** track gaps, errors, edge cases, or proposals for new Längle sources to incorporate.
- **PR discussions** work through methodology proposals (e.g., adding a new FM sub-theme, extending to a different population's testimony).
- **Tagged releases** package consensus-validated updates and ship them with a fresh Zenodo DOI.

When proposing a new version: follow the multi-LLM consensus pattern documented in `docs/lineage_GrahamNelsonZutter_CC-BY-NC-ND-4.0.md`. Single-LLM unilateral edits should be flagged for review.

## Authorship and contribution

**Sole author:** Graham Nelson-Zutter (graham@aestra.ca), MSc student, Existential Analysis & Logotherapy, University of Salzburg.

The LLMs used during the methodology (ChatGPT 5 Thinking, Gemini 2.5 Turbo, Claude Sonnet 4.5, and others described in `docs/lineage_GrahamNelsonZutter_CC-BY-NC-ND-4.0.md`) are **research instruments**, not contributors or co-authors. They do not receive attribution as contributors in `CITATION.cff`, `.zenodo.json`, or git commit metadata. This follows the position taken by major academic journals (Nature, Science, JAMA, the World Association of Medical Editors) that AI systems cannot meet the criteria for authorship and must not be listed as such.

If a future contributor proposes a PR, they should be added to `CITATION.cff` as a co-author of that version (with their consent). The "LLMs as tools, not authors" rule applies to AI systems specifically and is not negotiable.

## License

This work is released under **Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)** — see `LICENSE`.

The Längle source quotations embedded in this codebook are used under academic fair use / quotation right and remain subject to their original publishers' copyright — see `NOTICE.md`.
