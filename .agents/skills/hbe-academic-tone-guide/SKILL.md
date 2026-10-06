---
name: hbe-academic-tone-guide
description: Edit Chinese or English scholarly tone and claim strength only for book manuscripts in the Human-Bots-Ecology repository.
metadata:
  openclaw:
    emoji: "🎓"
    category: "writing"
    subcategory: "polish"
    keywords: ["academic tone", "register", "formal writing", "scholarly voice", "hedging", "disciplinary conventions"]
    source: "wentor-research-plugins"
---

## Human-Bots-Ecology repository scope

Use this skill only when the working directory is `/Users/rongzhi.nie/Human-Bots-Ecology` or its descendant and every target file resolves inside that repository. Do not apply it in another project, even if explicitly named there. A relocated repository needs an author-approved path update. Read the repository-root `AGENTS.md` and `docs/editorial-style.md` for manuscript work. Scripts, when needed, run from this repository with input and output paths inside it; inspect the script and resolve paths before invocation.

Use the free core workflow. Do not install global dependencies or sibling skills, enable paid APIs, upload manuscripts, or use proxy services. Public metadata queries use only necessary DOI, title, or keywords. No tool-attribution citation is automatically added to the book bibliography; provenance is recorded in `docs/skills.md`.

# Academic Tone Guide

A skill for adjusting the tone, register, and voice of academic writing to match disciplinary expectations and target venue conventions. Covers the spectrum from informal drafts to publication-ready prose, including hedging language, impersonal constructions, discipline-specific stylistic norms, and common tone pitfalls that undermine credibility.

## Chinese book adaptation

Apply `docs/editorial-style.md` to Chinese manuscripts. Do not impose English contraction rules, mandatory passive voice, a ban on all questions, or a journal-only voice on a monograph. Preserve the author's voice and disciplinary distinctions. Editing examples are illustrations: never add statistics, citations, methods, or precision absent from the manuscript.

## Understanding Academic Register

### The Register Spectrum

Academic writing occupies a specific position on the formality spectrum. The appropriate register depends on the genre (journal article, conference paper, thesis, grant proposal, blog post) and the discipline (humanities tend toward more elaborate prose; STEM fields favor concise, direct statements).

```
Register Levels in Academic Writing:

Level 1 - Informal (lab notebooks, internal emails):
  "We tried the new method and it worked way better."

Level 2 - Semi-formal (conference talks, blog posts):
  "We tested the proposed method and observed significant improvement."

Level 3 - Formal (journal articles, theses):
  "The proposed method was evaluated against the baseline, yielding
   an improvement in accuracy, with magnitude and uncertainty reported from verified results."

Level 4 - Highly formal (legal briefs, policy documents):
  "The aforementioned methodology was subjected to rigorous
   evaluation, the results of which demonstrate an enhancement in predictive accuracy within the scope of the verified evaluation."
```

Most academic papers should aim for Level 3. Level 4 often sounds stilted and impedes readability. Level 2 is acceptable for workshop papers or invited commentaries but too casual for top-tier journals.

### Discipline-Specific Norms

```
STEM Fields:
  - Prefer active voice for methods: "We collected samples..."
  - Use passive voice for established processes: "Samples were centrifuged..."
  - Short sentences, minimal adjectives
  - Precision over elegance
  - Numbered equations and defined variables

Social Sciences:
  - Mix of active and passive voice
  - Moderate hedging: "The results suggest..." rather than "The results prove..."
  - Theory-heavy introductions with clear operational definitions
  - APA style typically required

Humanities:
  - Complex sentence structures are acceptable
  - Argumentative, first-person voice common in many fields
  - Extensive engagement with prior scholarship expected
  - Discipline-specific jargon must be deployed precisely
  - Chicago or MLA style common
```

## Hedging and Boosting

### Hedging Language

Hedging is the use of cautious language to qualify claims. It signals intellectual honesty and awareness of limitations. Over-hedging weakens arguments; under-hedging invites criticism.

```
Hedging Devices:

Modal verbs (context-dependent examples, not a universal strength order):
  might, could, may, can, should, would, will, must
  "This approach might explain..."  (weak claim)
  "This approach may explain..."    (moderate claim)
  "This approach can explain..."    (confident claim)

Hedging phrases:
  "It appears that..."
  "The evidence suggests..."
  "One possible interpretation is..."
  "To some extent..."
  "Under certain conditions..."

Hedging quantifiers:
  "Most participants..." (not "all")
  "In many cases..." (not "always")
  "A substantial proportion..." (not "everyone")
```

### When to Hedge and When Not To

```
HEDGE when:
  - Reporting your own results (findings suggest, data indicate)
  - Describing an association from correlational data without converting it into a causal claim
  - Generalizing beyond your sample
  - Citing published findings whose original scope or uncertainty requires qualification
  - Interpreting ambiguous results
  - Discussing implications and future directions

DO NOT HEDGE when:
  - Stating established facts ("DNA is a double helix")
  - Describing your own methodology ("We recruited 200 participants")
  - Reporting objective measurements ("The temperature was 37 degrees C")
```

## Common Tone Problems and Fixes

### Informal Language

```
Problem: Colloquialisms and contractions

Before: "We didn't find any significant results, which was kind of
         surprising given that lots of previous studies had found
         strong effects."

After:  "No statistically significant effects were observed; interpret
         this result with its uncertainty and the verified prior literature."

Rules:
  - No contractions (didn't -> did not, it's -> it is)
  - No colloquialisms (kind of, a lot, pretty much)
  - No rhetorical questions in formal papers
  - Avoid first-person opinion statements without evidence
  - Use specific numbers only if verified in the source; otherwise retain appropriate qualitative wording
```

### Overconfident Claims

```
Problem: Making claims stronger than the evidence supports

Before: "Our results prove that social media causes depression in
         teenagers."

After:  "Our results indicate an association
         between social media usage and depressive symptoms among
         adolescent participants, consistent with the hypothesis
         that excessive social media engagement may contribute to
         negative mental health outcomes."

Note: "Prove" is almost never appropriate in empirical research.
      Use "demonstrate," "indicate," "suggest," or "support."
```

### Emotional Language

```
Problem: Using emotionally charged words that bias the reader

Before: "The appalling lack of research on this devastating condition
         is shocking and must be remedied immediately."

After:  "Despite the substantial disease burden, the existing literature
         on this condition remains limited. Further research is needed
         to address critical knowledge gaps."
```

## Authorial Voice and Person

### First Person Usage

The question of whether to use "I" or "we" in academic writing varies by discipline and journal.

```
Active first person (increasingly accepted in STEM):
  "We designed the experiment to test..."
  "We propose a novel framework for..."
  "In this paper, we argue that..."

Passive alternative:
  "The experiment was designed to test..."
  "A novel framework is proposed for..."
  "This paper argues that..."

Guidance:
  - Check your target journal's style guide
  - "We" is standard in multi-author papers, even in passive-heavy fields
  - Single-author "I" is common in humanities, less so in STEM
  - Avoid "the authors" to refer to yourself (awkward in most contexts)
  - Never use "you" to address the reader in formal papers
```

## Practical Revision Checklist

```
Tone Audit Checklist:

1. Search for contractions -> expand all
2. Search for colloquialisms -> replace with formal equivalents
3. Check hedging on all claims -> calibrate to evidence strength
4. Verify consistent voice (active/passive) -> match journal norms
5. Remove emotional/judgmental adjectives -> use neutral descriptors
6. Check whether questions serve the scholarly argument; revise empty rhetorical questions
7. Ensure technical terms are defined on first use
8. Distinguish statistical significance from substantive importance; do not add a statistical claim
9. Remove filler phrases ("it is important to note that") -> state directly
10. Read abstract aloud -> does it sound like published papers in the venue?
```

This checklist can be applied during the final revision pass, after substantive content editing is complete. Tone adjustments should be among the last changes made, as earlier revisions frequently introduce new informal language that needs to be caught.
