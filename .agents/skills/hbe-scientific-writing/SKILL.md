---
name: hbe-scientific-writing
description: Draft and revise scholarly book chapters with evidence-aware scientific prose only in the Human-Bots-Ecology repository.
license: MIT
metadata:
  version: "2.3"
  last-reviewed: "2026-10-01"
  skill-author: K-Dense Inc.
---


## Human-Bots-Ecology repository scope

Use this skill only when the working directory is `/Users/rongzhi.nie/Human-Bots-Ecology` or its descendant and every target file resolves inside that repository. Do not apply it in another project, even if explicitly named there. A relocated repository needs an author-approved path update. Read the repository-root `AGENTS.md` and `docs/editorial-style.md` for manuscript work. Scripts, when needed, run from this repository with input and output paths inside it; inspect the script and resolve paths before invocation.

Use the free core workflow. Do not install global dependencies or sibling skills, enable paid APIs, upload manuscripts, or use proxy services. Public metadata queries use only necessary DOI, title, or keywords. No tool-attribution citation is automatically added to the book bibliography; provenance is recorded in `docs/skills.md`.

# Scientific writing for an academic book

Adapted from K-Dense Scientific Writing. The author controls the book's scientific and interpretive decisions. Use `docs/editorial-style.md` for Chinese prose; preserve source limitations and the author's voice.

## Work at the requested scale

For drafting, use the known chapter purpose, audience, author material and verified sources. Mark missing positioning or evidence explicitly and continue work that does not depend on it. A sentence-level edit does not require study registration, CRediT records, evidence IDs or claim hashes.

A monograph follows its argument and chapter roles. Use IMRaD only for an empirical chapter where it fits the author-selected structure. Do not impose clinical journal submission gates on theoretical or humanities chapters.

## Evidence-aware prose

1. Identify the paragraph or section's question, claim and available evidence. Distinguish definition, observation, empirical finding, interpretation and normative judgment.
2. Draft connected prose that makes the reasoning visible. Match claim strength to the design, sources, uncertainty and scope. Use only verified facts; leave unresolved matters clearly marked.
3. Check that methods, findings, figures and repeated numbers agree where they exist. Never convert association to causation or a non-significant result into equivalence.
4. Verify that citations support the precise nearby proposition. A metadata match is separate from checking the source text and locator. Do not invent quotations, pages, identifiers, statistical results, permissions or declarations.
5. Revise for terminology, paragraph movement and academic tone after substantive issues are settled. Summaries and conclusions must match what the chapter actually establishes.

Use [writing principles](references/writing_principles.md) when paragraph or sentence craft needs detail, and [citation integrity](references/citation_styles.md) when reference handling needs detail. Use [figures and tables](references/figures_tables.md) only for those deliverables. Sources should be opened and checked before calling a claim verified; search snippets alone are insufficient. Human author verification and AI checks should be identified accurately, never conflated.

## Optional empirical and publication audits

For an empirical chapter or an author-requested final audit, choose only the relevant checks from [evidence workflow](references/evidence_workflow.md), [reporting guidelines](references/reporting_guidelines.md) and [research integrity](references/research_integrity_open_science.md). Registries, claim hashes and full manuscript scaffolds are optional structured-audit tools, not a prerequisite for ordinary book editing. Use journal or clinical checklists only when the actual material and publication requirements call for them.

The bundled Python 3.11+ scripts are local and dependency-free. Their inputs follow the bundled schemas; do not run them on ordinary prose expecting a universal book-quality score. Read [CLI reference](references/cli_reference.md) before using them, resolve all input/output paths inside the repository and invoke from the repository root, for example:

```bash
python3 .agents/skills/hbe-scientific-writing/scripts/check_consistency.py path/to/consistency_manifest.json
```

Do not generate scaffold files or demand declarations unless the task needs them. Automated checks detect particular inconsistencies; they do not establish the truth of claims or publisher acceptance.

## Publication requirements

When the author requests publication preparation, check that publisher's actual formatting, quotation permissions, attribution and AI-use requirements. The author approves final scholarly decisions and submission. Do not assume medical-journal authorship rules apply to this book. Tool provenance is recorded in `docs/skills.md`; a tool paper is not automatically inserted among the book's scholarly sources.
