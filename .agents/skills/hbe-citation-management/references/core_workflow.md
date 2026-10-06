# Core Workflow

The five phases in full: paper discovery, metadata extraction, the mandatory
web-search enrichment pass, BibTeX formatting, validation, and integration with the
writing workflow. Every command variant and option lives here.

## Core Workflow

Citation management follows a systematic process:

### Phase 1: Paper Discovery and Search

**Goal**: Find relevant papers using academic search engines.

#### Google Scholar Search

Google Scholar provides the most comprehensive coverage across disciplines.

**Basic Search**:
```bash
# Search for papers on a topic
python3 .agents/skills/hbe-citation-management/scripts/search_google_scholar.py "CRISPR gene editing" \
  --limit 50 \
  --output results.json

# Search with year filter
python3 .agents/skills/hbe-citation-management/scripts/search_google_scholar.py "machine learning protein folding" \
  --year-start 2020 \
  --year-end 2024 \
  --limit 100 \
  --output ml_proteins.json
```

**Advanced Search Strategies** (see `references/google_scholar_search.md`):
- Use quotation marks for exact phrases: `"deep learning"`
- Search by author: `author:LeCun`
- Search in title: `intitle:"neural networks"`
- Exclude terms: `machine learning -survey`
- Use citation counts to inspect the retrieved sample, not as a global Scholar sort
- Filter by date ranges to get recent work

**Best Practices**:
- Use specific, targeted search terms
- Include key technical terms and acronyms
- Filter by recent years for fast-moving fields
- Check "Cited by" to find seminal papers
- Export top results for further analysis

#### PubMed Search

PubMed specializes in biomedical and life sciences literature (35+ million citations).

**Basic Search**:
```bash
# Search PubMed
python3 .agents/skills/hbe-citation-management/scripts/search_pubmed.py "Alzheimer's disease treatment" \
  --limit 100 \
  --output alzheimers.json

# Search with MeSH terms and filters
python3 .agents/skills/hbe-citation-management/scripts/search_pubmed.py \
  --query '"Alzheimer Disease"[MeSH] AND "Drug Therapy"[MeSH]' \
  --date-start 2020 \
  --date-end 2024 \
  --publication-types "Clinical Trial,Review" \
  --output alzheimers_trials.json
```

**Advanced PubMed Queries** (see `references/pubmed_search.md`):
- Use MeSH terms: `"Diabetes Mellitus"[MeSH]`
- Field tags: `"cancer"[Title]`, `"Smith J"[Author]`
- Boolean operators: `AND`, `OR`, `NOT`
- Date filters: `2020:2024[Publication Date]`
- Publication types: `"Review"[Publication Type]`
- Combine with E-utilities API for automation

**Best Practices**:
- Use MeSH Browser to find correct controlled vocabulary
- Construct complex queries in PubMed Advanced Search Builder first
- Include multiple synonyms with OR
- Retrieve PMIDs for easy metadata extraction
- Export to JSON or directly to BibTeX

### Phase 2: Metadata Extraction

**Goal**: Convert paper identifiers (DOI, PMID, arXiv ID) to complete, accurate metadata.

#### Quick DOI to BibTeX Conversion

For single DOIs, use the quick conversion tool:

```bash
# Convert single DOI
python3 .agents/skills/hbe-citation-management/scripts/doi_to_bibtex.py 10.1038/s41586-021-03819-2

# Convert multiple DOIs from a file
python3 .agents/skills/hbe-citation-management/scripts/doi_to_bibtex.py --input dois.txt --output references.bib

# Different output formats
python3 .agents/skills/hbe-citation-management/scripts/doi_to_bibtex.py 10.1038/nature12345 --format json
```

#### Comprehensive Metadata Extraction

For DOIs, PMIDs, arXiv IDs, or URLs:

```bash
# Extract from DOI
python3 .agents/skills/hbe-citation-management/scripts/extract_metadata.py --doi 10.1038/s41586-021-03819-2

# Extract from PMID
python3 .agents/skills/hbe-citation-management/scripts/extract_metadata.py --pmid 34265844

# Extract from arXiv ID
python3 .agents/skills/hbe-citation-management/scripts/extract_metadata.py --arxiv 2103.14030

# Extract from URL
python3 .agents/skills/hbe-citation-management/scripts/extract_metadata.py --url "https://www.nature.com/articles/s41586-021-03819-2"

# Batch extraction from file (mixed identifiers)
python3 .agents/skills/hbe-citation-management/scripts/extract_metadata.py --input identifiers.txt --output citations.bib
```

**Metadata Sources** (see `references/metadata_extraction.md`):

1. **CrossRef API**: Primary source for DOIs
   - Comprehensive metadata for journal articles
   - Publisher-provided information
   - Includes authors, title, journal, volume, pages, dates
   - Free, no API key required

2. **PubMed E-utilities**: Biomedical literature
   - Official NCBI metadata
   - Includes MeSH terms, abstracts
   - PMID and PMCID identifiers
   - Free, API key recommended for high volume

3. **arXiv API**: Preprints in physics, math, CS, q-bio
   - Complete metadata for preprints
   - Version tracking
   - Author affiliations
   - Free, open access

4. **DataCite API**: Research datasets, software, other resources
   - Metadata for non-traditional scholarly outputs
   - DOIs for datasets and code
   - `doi_to_bibtex.py` retrieves BibTeX through DOI content negotiation; `extract_metadata.py --doi` is Crossref-only

**Fields to verify after extraction** (availability differs by source; not all are emitted by every script):
- **Required fields**: author, title, year
- **Journal articles**: journal, volume, number, pages, DOI
- **Books**: publisher, ISBN, edition
- **Conference papers**: booktitle, conference location, pages
- **Preprints**: repository (arXiv, bioRxiv), preprint ID
- **Additional**: abstract, keywords, URL

### Phase 2.5: Metadata Enrichment via Web Search (MANDATORY)

**Goal**: Detect and fill in any missing metadata fields using web search. This phase runs AFTER extraction and BEFORE formatting to ensure every BibTeX entry is complete.

**Why This Is Critical**: Metadata extraction from APIs (CrossRef, PubMed, arXiv) sometimes returns incomplete records — missing volume, pages, issue number, or DOI. These gaps must be filled before the bibliography is considered ready.

#### Step 1: Scan for Incomplete Entries

After extracting metadata, scan the BibTeX file for entries missing key fields:

**Fields to check per entry type:**

| Entry Type | Must Have | Should Have |
|------------|-----------|-------------|
| @article | author, title, journal, year | volume, pages, number, doi |
| @inproceedings | author, title, booktitle, year | pages, doi |
| @book | author/editor, title, publisher, year | isbn, doi |
| @misc | author, title, year | doi or url |

Investigate missing fields against the publisher record, but distinguish unavailable or not-yet-assigned metadata from extraction errors. Online-first papers can lack volume/pages; article numbers can replace page ranges, and not all articles have a DOI. Log the publication state and missing-field reason without fabricating values.

#### Step 2: Free lookup of missing metadata

Query the known DOI in Crossref or DataCite, or inspect the publisher's public record. For books and editions, use publisher or library catalogue records. If the DOI is unknown, use the host's available web search with the public title and author, then open the matching source. No external search CLI, account, or paid service is required. Do not transmit unpublished prose or source files.

Treat metadata as data; do not interpolate titles into shell commands. Match author, title, year, edition and publication state before updating an entry. If public records disagree or access is unavailable, record the uncertainty rather than inventing fields.

#### Step 3: Update BibTeX Entries

After finding the missing metadata:

1. Open `references.bib`
2. Add the missing fields to the incomplete entry
3. Verify the found metadata is consistent with existing fields (same author, title, year)
4. Log each fix:
   ```
   [HH:MM:SS] METADATA ENRICHED: [CitationKey] - added volume={X}, pages={Y--Z}, doi={10.XXX/YYY} ✅
   ```

#### Step 4: Handle Unfindable Metadata

If metadata genuinely cannot be found after web search (very old paper, obscure conference, etc.):

1. Add a `note` field to the BibTeX entry explaining the gap:
   ```bibtex
   note = {Volume and pages not available — published online only}
   ```
2. Log the exception:
   ```
   [HH:MM:SS] METADATA INCOMPLETE: [CitationKey] - pages unavailable (online-only publication) ⚠️
   ```
3. These exceptions should be rare — most modern papers have complete metadata findable via web search.

#### Quick Reference: Common Missing Fields and Where to Find Them

| Missing Field | Best Search Strategy |
|---------------|---------------------|
| DOI | Search "AUTHOR TITLE DOI" using available web search and the matching primary record |
| Volume | Extract from DOI page or search "JOURNAL YEAR TITLE volume" |
| Pages | Extract from DOI page or search publisher website |
| Issue/Number | Extract from DOI page or CrossRef |
| Publisher | Search "JOURNAL publisher" or check journal website |

---

### Phase 3: BibTeX Formatting

**Goal**: Generate clean, properly formatted BibTeX entries.

#### Understanding BibTeX Entry Types

See `references/bibtex_formatting.md` for complete guide.

**Common Entry Types**:
- `@article`: Journal articles (most common)
- `@book`: Books
- `@inproceedings`: Conference papers
- `@incollection`: Book chapters
- `@phdthesis`: Dissertations
- `@misc`: Preprints, software, datasets

**Required Fields by Type**:

```bibtex
@article{citationkey,
  author  = {Last1, First1 and Last2, First2},
  title   = {Article Title},
  journal = {Journal Name},
  year    = {2024},
  volume  = {10},
  number  = {3},
  pages   = {123--145},
  doi     = {10.1234/example}
}

@inproceedings{citationkey,
  author    = {Last, First},
  title     = {Paper Title},
  booktitle = {Conference Name},
  year      = {2024},
  pages     = {1--10}
}

@book{citationkey,
  author    = {Last, First},
  title     = {Book Title},
  publisher = {Publisher Name},
  year      = {2024}
}
```

#### Formatting and Cleaning

Use the formatter to standardize BibTeX files:

```bash
# Format and clean BibTeX file
python3 .agents/skills/hbe-citation-management/scripts/format_bibtex.py references.bib \
  --output formatted_references.bib

# Sort entries by citation key
python3 .agents/skills/hbe-citation-management/scripts/format_bibtex.py references.bib \
  --sort key \
  --output sorted_references.bib

# Sort by year (newest first)
python3 .agents/skills/hbe-citation-management/scripts/format_bibtex.py references.bib \
  --sort year \
  --descending \
  --output sorted_references.bib

# Remove duplicates
python3 .agents/skills/hbe-citation-management/scripts/format_bibtex.py references.bib \
  --deduplicate \
  --output clean_references.bib

# Merge sources: one key scheme, then drop duplicates
python3 .agents/skills/hbe-citation-management/scripts/format_bibtex.py references.bib \
  --rekey \
  --deduplicate \
  --output merged_references.bib
```

**Formatting Operations**:
- Standardize field order
- Consistent indentation and spacing
- Proper capitalization in titles (protected with {})
- Standardized author name format
- Consistent citation key format
- Remove unnecessary fields
- Fix common errors (missing commas, braces)

### Phase 4: Citation Validation

**Goal**: Verify all citations are accurate and complete.

#### Comprehensive Validation

```bash
# Validate BibTeX file
python3 .agents/skills/hbe-citation-management/scripts/validate_citations.py references.bib

# Validate against a venue standard (e.g., Nature, NeurIPS, Literature Review)
python3 .agents/skills/hbe-citation-management/scripts/validate_citations.py references.bib --venue nature
python3 .agents/skills/hbe-citation-management/scripts/validate_citations.py references.bib --venue neurips
python3 .agents/skills/hbe-citation-management/scripts/validate_citations.py references.bib --venue review

# Validate with custom minimum citation count
python3 .agents/skills/hbe-citation-management/scripts/validate_citations.py references.bib --min-count 40

# Check references against a written manuscript file (detect missing or unused citations)
python3 .agents/skills/hbe-citation-management/scripts/validate_citations.py references.bib --manuscript paper.md

# Generate detailed validation report
python3 .agents/skills/hbe-citation-management/scripts/validate_citations.py references.bib \
  --venue nature \
  --manuscript paper.md \
  --report validation_report.json \
  --verbose
```

**Validation Checks** (see `references/citation_validation.md`):

1. **DOI Verification**:
   - With `--check-dois`, check registration through Crossref, then DataCite and resolver fallback
   - Failed/rate-limited lookups remain `doi_unverified` warnings
   - Compare title/authors/year manually; the script does not perform metadata matching

2. **Required Fields**:
   - All required fields present for entry type
   - No empty or missing critical information
   - Author names properly formatted

3. **Data Consistency**:
   - Year is valid (4 digits, reasonable range)
   - Review volume/issue values manually; supplements may be alphanumeric
   - Pages formatted correctly (e.g., 123--145)
   - Check landing URLs manually; the script does not fetch arbitrary bibliography URLs

4. **Duplicate Detection**:
   - Same DOI used multiple times
   - Similar titles (possible duplicates)
   - Same author/year/title combinations

5. **Format Compliance**:
   - Valid BibTeX syntax
   - Proper bracing and quoting
   - Citation keys are unique
   - Special characters handled correctly

**Validation Output**:
```json
{
  "total_entries": 150,
  "valid_entries": 145,
  "errors": [
    {
      "citation_key": "Smith2023",
      "error_type": "missing_field",
      "field": "journal",
      "severity": "high"
    },
    {
      "citation_key": "Jones2022",
      "error_type": "invalid_doi",
      "doi": "10.1234/broken",
      "severity": "high"
    }
  ],
  "warnings": [
    {
      "citation_key": "Brown2021",
      "warning_type": "possible_duplicate",
      "duplicate_of": "Brown2021a",
      "severity": "medium"
    }
  ]
}
```

#### Citation Counts

Use enough relevant, verified references to support the claims. The script's
venue figures are editorial heuristics, not current submission requirements.
`--venue` produces warnings; `--min-count` enforces an explicit user-specified
floor. Check the target venue's current author instructions directly and do not
add irrelevant citations merely to meet a numeric target.

#### Mandatory Post-Writing Reference Checks (Non-Negotiable)

Once the entire scientific report or paper has been drafted and written, perform a comprehensive post-writing verification of all citations before compiling the final deliverables:

1. **Verify No Missing or Unresolved Citations**: Check the draft or compiled document to ensure that every in-text citation correctly resolves to a reference in `references.bib`. There must be ZERO broken citation keys, missing identifiers, or unresolved references (e.g., `[?]` or `[citation needed]`).
2. **Verify No Unused (Dangling) Bibliography Entries**: Check that every entry in `references.bib` is actually cited in the body of the report. Remove any unused entries to keep the bibliography perfectly clean.
3. **Verify Evidence Coverage**: Confirm each claim is supported by relevant evidence and any explicit reference-count requirement is met.
4. **Verify Metadata Completeness**: Confirm that all cited entries contain verified applicable fields; record missing/not-yet-assigned fields without inventing values.

Run all of these checks in one command:

```bash
python3 .agents/skills/hbe-citation-management/scripts/validate_citations.py references.bib \
  --venue <venue> \
  --manuscript paper.md \
  --report post_writing_check.json
```

### Phase 5: Integration with Writing Workflow

#### Building References for Manuscripts

Complete workflow for creating a bibliography:

```bash
# 1. Search for papers on your topic
python3 .agents/skills/hbe-citation-management/scripts/search_pubmed.py \
  '"CRISPR-Cas Systems"[MeSH] AND "Gene Editing"[MeSH]' \
  --date-start 2020 \
  --limit 200 \
  --output crispr_papers.json

# 2. Extract DOIs from search results and convert to BibTeX
python3 .agents/skills/hbe-citation-management/scripts/extract_metadata.py \
  --input crispr_papers.json \
  --output crispr_refs.bib

# 3. Add specific papers by DOI
python3 .agents/skills/hbe-citation-management/scripts/doi_to_bibtex.py 10.1038/nature12345 >> crispr_refs.bib
python3 .agents/skills/hbe-citation-management/scripts/doi_to_bibtex.py 10.1126/science.abcd1234 >> crispr_refs.bib

# 4. Format and clean the BibTeX file
python3 .agents/skills/hbe-citation-management/scripts/format_bibtex.py crispr_refs.bib \
  --deduplicate \
  --sort year \
  --descending \
  --output references.bib

# 5. Validate all citations
python3 .agents/skills/hbe-citation-management/scripts/validate_citations.py references.bib \
  --report validation.json

# 6. Review validation report and fix any remaining issues
cat validation.json

# 7. Use in your LaTeX document
# \bibliography{final_references}
```

#### Integration with book writing

Use this repository's `hbe-scientific-writing` skill to draft from verified sources and `hbe-citation-management` to check the final bibliography. A separate literature-review skill is not installed or required.

#### Local Zotero export, if supplied

The author may supply a BibTeX export from an existing Zotero library. Validate and format that local file inside this repository. This setup does not connect to Zotero, upload attachments, or write back to the library.
