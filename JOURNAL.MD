# PathReview Contribution Journal

## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/149

**Issue title:** Structural chunker silently drops documents that contain no headings

**Tier:** [x] Tier 1 [ ] Tier 2 [ ] Tier 3

**Problem summary:**

The structural chunker currently returns an empty list when a document does not contain Markdown headings. This causes valid plain-text documents to be silently excluded from the RAG index. The issue affects `StructuralChunker.chunk()` in the ingestion module. A successful fix should preserve documents without headings by creating a valid chunk or using an appropriate fallback strategy.

**Selection reasoning — “Is this right for me?” checklist:**

This issue is a Tier 1 bug with a clearly identified function and failing unit test. The scope appears limited to the structural chunker and its related tests, so it should not require understanding the entire application. I also have experience with document chunking, embeddings, and RAG projects, which makes the problem relevant to my background. I expect the issue to be realistic to complete within the Week 8–9 timeline.

**Branch name:** `fix/149-handle-documents-without-headings`

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [ ] Issue added to cohort ledger

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** [d5a98ef — document issue 149 reproduction](https://github.com/DTran228/pathreview/commit/d5a98ef985cd0a16a07cec8312df6e4fd5cfab5b)

**Reproduction summary:** I reproduced issue #149 with a 1,000-character plain-text input that
contains no Markdown headings. `StructuralChunker.chunk()` returned `0` chunks, even though the
input is non-empty; `_extract_sections()` only collects lines after it has seen a heading, so it
silently drops every line in this document type.

**PLAN.md link:** [PLAN.md](PLAN.md)

**Walkthrough video (recommended):** Not recorded.

**Blockers or open questions:** The repository's `.venv` points to a Python executable that is
no longer installed, so the standard `pytest` command cannot start yet. This is a local setup
blocker separate from #149. The reproduction above was executed against the chunker's current
logic with a minimal local tokenizer stub; after repairing the environment, I will run the
existing focused unit test and the complete structural-chunker suite before implementing the fix.
