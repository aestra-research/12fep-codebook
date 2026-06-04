---
created_by: "Graham Nelson-Zutter"
copyright: "©2026 Aestra Inc. except when external source noted."
license: "CC BY-NC-ND-4.0"
---

# Version Lineage and Build Methodology

This document explains how each version of the 12FEP codebook was produced, and **why traditional file-level git versioning is not sufficient** as the canonical record.

## Why not just `git log`?

Traditional git history records *what changed in the repository* — useful for tracking deposits, edits, and PR merges.

But the codebook's version lineage is **not a linear single-author edit chain.** Each substantive version is the output of a structured multi-LLM process:

1. **Three LLMs** (ChatGPT 5 Thinking, Gemini 2.5 Turbo, Claude Sonnet 4.5) **independently** build their own v1 from the same Längle source set, using the same prompt.
2. **Each LLM then merges all three v1s** into its own merged-v1 — three parallel merges, not a single sequential one.
3. **An LLM jury vote** is conducted: each LLM is asked which of the three merged-v1s is best. The winner becomes the next MASTER.
4. **Manual researcher review** validates the LLM jury choice before promotion.

This is *parallel + consensus + selection*, not *single-author iteration*. `git log` would show "added v1.0 / v1.1 / v1.2 / v2.0 / MASTER" — a flat chain that loses the parallelism and the consensus structure.

`docs/lineage.md` (this file) records the methodology. `git log` records the *deposit* of each version into this repo. The two are complementary:

| What | Tracked by |
|---|---|
| When each file was uploaded to the repo | `git log` |
| Which LLM produced each version | `lineage.md` |
| Which prompt and source set produced each version | `lineage.md` |
| Which merge step produced each "merged-vN" | `lineage.md` |
| Why the MASTER was selected | `lineage.md` (LLM jury vote outcome + researcher review note) |
| What changed file-byte-wise between adjacent versions | `git diff` |

## Per-LLM strengths

The multi-LLM approach is intentional, not redundant. Each LLM brings different characteristics to codebook work, observed consistently across both this 12FEP build and the predecessor 4FMs build (see "Related artifact" below). Documenting them here so future researchers know what to expect from each track:

| LLM | Typical strengths in codebook work | Typical signatures |
|---|---|---|
| **Claude Sonnet 4.5** | Theoretical depth, phenomenological grounding, extensive version-history tracking, careful merge rationale | Largest output (~75 KB v2). Catches source duplications (e.g., Längle 2011 = 2002). Preserves nuance. |
| **ChatGPT 5 Thinking** | Practical coding structure, hierarchical code IDs (e.g., `FM1.ST1`), explicit inclusion/exclusion criteria, supporting-quote citation density | Most systematic sub-theme breakdowns. Strong for applied research coding. |
| **Gemini 2.5 Turbo** | Organizational clarity, conceptual framework tables, epistemological chains, accessibility | Cleanest structural overview. Compact outputs (often smaller — sometimes a signal of context-window loss; check for size drops). |

These signatures are not absolute — they shifted across runs. But they justify why "three LLMs in parallel" produces something stronger than any single LLM: their strengths are complementary, and their weaknesses are non-overlapping, so consensus tends to converge on the best contribution of each.

## Build methodology — current MASTER (v2)

**Source set:** Längle (2002), Längle (2011b) — see NOTICE.md for full citations.

**Prompts used:** P18 (codebook construction), P19 (merge step 1), P20 (merge step 2 with hallucination verification), P21 (LLM jury vote). Source: WCET4 paper *Catalog of LLM Prompts (from Thesis)* — Google Doc ID `1smLo49YXOyCat8UTZU54EfDW0MzIHL4Wq4QNImMzOqQ` and the methodology DOI [forthcoming].

### Step 1 — Independent build (parallel)

Each of three LLMs received the same prompt (P18) and the same two Längle source PDFs:

| LLM | v1 file |
|---|---|
| Claude Sonnet 4.5 | `codebook/versions/claude-12fep-v1.json` |
| Gemini 2.5 Turbo | `codebook/versions/gemini-12fep-v1.json` |
| ChatGPT 5 Thinking | `codebook/versions/chatgpt-12fep-v1.json` |

### Step 2 — Cross-LLM merge (parallel)

Each LLM received all three v1 files and ran prompt P19, producing its own merged-v1:

| Merger LLM | merged-v1 file |
|---|---|
| Claude Sonnet 4.5 | `codebook/versions/claude-12fep-merged-v1.json` |
| Gemini 2.5 Turbo | `codebook/versions/gemini-12fep-merged-v1.json` |
| ChatGPT 5 Thinking | `codebook/versions/chatgpt-12fep-merged-v1.json` |

### Step 3 — Verification pass: cross-LLM merge of the three merged-v1s

Each of the three LLMs was given **all three merged-v1 files** plus the original Längle source PDFs, and asked to produce a final cross-LLM merge (prompt P20) with verification that every quote traces back to the source. Each LLM produced its own merged-v2:

| Merger LLM | merged-v2 file |
|---|---|
| ChatGPT 5 Thinking | `codebook/versions/chatgpt-12fep-merged-v2.json` |
| Gemini 2.5 Turbo | `codebook/versions/gemini-12fep-merged-v2.json` |
| Claude Sonnet 4.5 | `codebook/versions/claude-12fep-merged-v2.json` *(byte-identical to `codebook/12fep-master.json` — see Step 4)* |

All three merged-v2s used the same inputs (the three merged-v1s + the Längle PDFs). They are *parallel candidates* for the final codebook, not sequential refinements. The MASTER's `codebook_metadata.merged_from` field lists the three merged-v1 inputs, confirming this structure.

### Step 4 — LLM jury vote

Each LLM was asked (prompt P21) to evaluate all three merged-v2 codebooks and select the most useful. Outcome:

- ChatGPT 5 Thinking chose: **Claude's merged-v2**
- Gemini 2.5 Turbo chose: **Claude's merged-v2**
- Claude Sonnet 4.5 chose: **Claude's merged-v2**

3/3 unanimous → Claude's merged-v2 was promoted to **MASTER**.

The MASTER is byte-identical to `codebook/versions/claude-12fep-merged-v2.json`. The MASTER lives at `codebook/12fep-master.json` as the canonical "use this one" pointer; the symmetric `versions/claude-12fep-merged-v2.json` copy makes the 3×3 parallel structure (3 LLMs × 3 stages) visible in the `versions/` directory for provenance auditing.

### Step 5 — Manual researcher review

The researcher (Graham Nelson-Zutter) manually verified:

- Every quote in the MASTER traces back to a verifiable line in the Längle source PDFs.
- Quotes that failed verification were moved to a `"Possible Hallucinations"` section (the **Quarantine** sub-pattern documented in the methodology paper).
- The `instructions_for_llm` block was reviewed for correctness vs. Längle's intent.

Date of MASTER promotion: \[fill in from `_MASTER_claude-12fep-merged-codebook-v2.json` mtime\].

## Future versions — the workflow contract

When the codebook is updated (new Längle source incorporated, FM sub-theme refined, etc.), the same five-step process should be followed:

1. Open a GitHub Issue describing the proposed change and rationale.
2. Open a draft PR running Steps 1–4 with the chosen LLM panel. Attach v1, merged-v1, merged-v2 outputs from each LLM in the PR.
3. Document the jury vote outcome in the PR.
4. Researcher review (Step 5) happens in the PR conversation.
5. Merge the PR and tag a new release (e.g., `v2.1`). GitHub → Zenodo auto-mints a version DOI.

**The PR discussion becomes the lineage record for that version.** It supplements (not replaces) the entry that should be added to this file when the release is tagged.

## Related artifact — the EA & LT 4FMs codebook (predecessor)

The 12FEP codebook builds on a prior multi-LLM consensus exercise: an **EA & LT 4 Fundamental Motivations codebook**, built from four Längle sources (1992, 2002, 2003, 2011) across the same three LLMs. That work has a fuller version history (v1.0 → v1.3 → v2.0 → v3.0–v3.3 → per-LLM v4.0 → cross-LLM v5.0) and produced extensive comparison + integration documentation:

- *EA_LT_4FMs_Codebook_v5_0_INTEGRATION_SUMMARY.md*
- *EA_LT_4FMs_Codebook_v5_0_COMPARISON_MATRIX.md*
- *EA_LT_4FMs_Codebook_v5_0_QUICK_REFERENCE.md*
- *Codebook_Merger_Summary.md*
- *codebook_comparison.md*
- *Quick_Start_Guide.md*

The 4FMs codebook is a **distinct artifact** from this 12FEP codebook (different scope — 4 motivations vs. 12 prerequisites; different source set) and may eventually get its own DOI deposit. For the WCET4 paper, the 12FEP codebook is the primary applied artifact and the focus of this repo.

The per-LLM signatures documented above were first characterized during the 4FMs build, and the same patterns held during the 12FEP build that produced this MASTER.

## On contributor attribution

Per the README: LLMs (Claude, ChatGPT, Gemini, etc.) are **tools used in the methodology**, not contributors or co-authors. They do not appear in `CITATION.cff`, `.zenodo.json`, git commit `--author`, or this lineage file's contributor lists.

Future human contributors who substantively shape a version (in the sense of ICMJE / CRediT author criteria) should be added to `CITATION.cff` with their explicit consent.

## Creation Timeline — original authoring dates

The codebook was authored Oct 31 – Nov 30, 2025, ~6 months before this repo was created (2026-05-31). The git commit dates here reflect when files were deposited into the public repo, not when they were authored. The authentic creation timestamps from the development workspace:

| File | Original creation (PT) | Stage |
|---|---|---|
| `versions/gemini-12fep-v1.json` | 2025-10-31  20:44 | Step 1 — Gemini's independent v1 |
| `versions/chatgpt-12fep-v1.json` | 2025-10-31  20:45 | Step 1 — ChatGPT's independent v1 |
| `versions/claude-12fep-v1.json` | 2025-10-31  21:13 | Step 1 — Claude's independent v1 |
| `versions/chatgpt-12fep-merged-v1.json` | 2025-10-31  21:16 | Step 2 — ChatGPT's merge of all 3 v1s |
| `versions/gemini-12fep-merged-v1.json` | 2025-10-31  21:17 | Step 2 — Gemini's merge of all 3 v1s |
| `versions/claude-12fep-merged-v1.json` | 2025-10-31  21:32 | Step 2 — Claude's merge of all 3 v1s |
| `versions/chatgpt-12fep-merged-v2.json` | 2025-11-01  00:27 | Step 3 — ChatGPT's merged-v2 candidate |
| `versions/gemini-12fep-merged-v2.json` | 2025-11-01  00:27 | Step 3 — Gemini's merged-v2 candidate |
| `versions/claude-12fep-merged-v2.json` (= `12fep-master.json`) | 2025-11-30  15:40 | Step 3 — Claude's merged-v2 (promoted to MASTER per LLM jury) |

Source: filesystem mtimes from the original codebook development workspace (iCloud, `~/Library/Mobile Documents/com~apple~CloudDocs/LibreOffice/*EA & LT Codebooks/LLMS/`). Mtimes are preserved at the source files and are verifiable by anyone with access to that workspace.

## Note on the renamed file

In the original development workspace, Claude's merged-v1 was named `claude-12fep-merged-codebook-v1.json` (with the extra word "codebook"). For symmetric naming across LLMs in this repo, it has been renamed to `claude-12fep-merged-v1.json`. The byte content of the codebook itself is unchanged; the `merged_from` reference inside `12fep-master.json` and `versions/claude-12fep-merged-v2.json` has been updated to reflect the in-repo filename (the only modification to the original artifact's JSON content).
