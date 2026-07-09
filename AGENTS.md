# Agent Operating Manual: OKF Knowledge Wiki

This document is the complete operating manual for any AI agent working with the knowledge bundle in this repository. It is self-contained: an agent needs only standard file tools (read, write, edit, search) plus the Obsidian CLI (for link health, graph visualization, and search) to perform all operations described here. No wiki-tool, no database, no SDK.

---

## 1. Purpose

This repository demonstrates the **Living Context** pattern: using AI agents to incrementally build and maintain a persistent, compounding knowledge base as interlinked markdown files. Instead of RAG — which rediscovers knowledge from scratch on every query — the wiki compiles knowledge once and keeps it current. Cross-references are already there. Contradictions have already been flagged. Synthesis reflects everything ingested.

The wiki is a **persistent, compounding artifact.** Every new source makes it richer. The agent does all the grunt work — summarizing, cross-referencing, filing, and bookkeeping. The human curates sources, directs analysis, and asks good questions.

---

## 2. Architecture: Three Layers

```
repository root/
├── raw-sources/                # Layer 1: IMMUTABLE raw source documents
│   ├── bcg-harvard-jagged-technological-frontier.pdf
│   ├── mit-cognitive-debt.pdf
│   ├── wharton-cognitive-surrender.pdf
│   └── devlin-meter-picks-the-winners.md
├── sample-wiki/                # Layer 2: THE WIKI (agent-owned markdown)
│   ├── index.md                # Content catalog — one-line summaries per page
│   ├── log.md                  # Chronological action log (append-only)
│   ├── sources/                # Source summaries (created during ingest)
│   ├── entities/               # Entity pages (created during ingest)
│   └── concepts/               # Concept pages (created during ingest)
├── OKF-SPEC.md                 # Full OKF specification (reference)
├── karpathy-llm-wiki-gist.md   # Original Karpathy wiki pattern (reference)
├── AGENTS.md                   # This file — the schema and operating manual
└── Abstract.md                 # Talk abstract and outline
```

**Layer 1 — Raw Sources:** Immutable source documents in `raw-sources/`. The agent reads from them but never modifies them. These are the inputs for the ingest workflow.

**Layer 2 — The Wiki:** Agent-owned markdown files inside `sample-wiki/`. The agent creates, updates, and maintains these entirely. The human reads them; the agent writes them. The wiki starts blank and grows as sources are ingested.

**Layer 3 — This File (AGENTS.md):** The schema. It defines structure, conventions, and workflows. This is what makes the agent a disciplined wiki maintainer rather than a generic chatbot.

---

## 3. OKF Conformance Rules

This wiki follows the **Open Knowledge Format (OKF) v0.1**. The full specification lives at `OKF-SPEC.md`. The essential rules are reproduced here so an agent never needs to leave this file to produce conformant output.

### 3.1 File Structure

Every `.md` file in the wiki is either a **reserved file** or a **concept document**.

Reserved filenames (MUST NOT be used for concept documents):
- `index.md` — Directory listing for progressive disclosure. No frontmatter (except optionally at the bundle root). Lists pages with one-line descriptions.
- `log.md` — Append-only chronological history. No frontmatter. Date-grouped entries, newest first.

All other `.md` files are concept documents.

### 3.2 Frontmatter (REQUIRED on every concept document)

```yaml
---
type: Reference                 # REQUIRED — short string identifying the kind of concept
title: Display Title            # Recommended — human-readable name
description: One-line summary   # Recommended — used by index generators and previews
resource: https://example.com   # Optional — canonical URI for the underlying asset
tags: [tag1, tag2]              # Optional — cross-cutting categorization
timestamp: 2026-06-29T11:50:00Z # Optional — ISO 8601 last-modified time
---
```

**Required:** `type` must be present and non-empty on every concept document. Any string is valid — consumers tolerate unknown types. This wiki uses:
- `Reference` — Source summaries and research references
- `Entity` — Proper nouns (people, organizations, products)
- `Concept` — Techniques, patterns, ideas, categories

**Extensions:** Producers MAY include any additional keys. Consumers SHOULD preserve unknown keys when round-tripping.

### 3.3 Body

Standard markdown. Producers SHOULD favor structural markdown (headings, lists, tables, fenced code blocks) over freeform prose — structure aids both human reading and agent retrieval.

Conventional section headings:

| Heading        | Purpose                                              |
|----------------|------------------------------------------------------|
| `# Summary`    | Overview of the source/topic                         |
| `# Key Findings` | Bulleted findings from the source                  |
| `# Implications for Developers` | Practical takeaways                 |
| `# Citations`  | External sources backing claims. See section 3.5    |

### 3.4 Cross-Linking

Concepts link to other concepts using **standard markdown links**. This wiki uses OKF absolute (bundle-relative) links:

```markdown
See the [BCG Study](/sources/bcg-harvard-study.md) for the jagged frontier findings.
```

Links begin with `/` and are interpreted relative to the `sample-wiki/` root. This is the recommended form because it is stable when documents are moved within their subdirectory.

Relative links are also valid:
```markdown
See the [neighboring study](./mit-cognitive-debt.md).
```

A link from concept A to concept B asserts a relationship. The specific kind of relationship is conveyed by surrounding prose, not by the link itself. Consumers treat all links as directed edges.

Consumers MUST tolerate broken links — a link whose target does not exist is not malformed; it may represent not-yet-written knowledge.

### 3.5 Citations

When a concept's body makes claims sourced from external material, list those sources under a `# Citations` heading:

```markdown
# Citations

[1] Dell'Acqua, F., et al. (2023). "Navigating the Jagged Technological Frontier." HBS Working Paper.
[2] https://example.com/supporting-evidence
```

### 3.6 Index Files

`index.md` enumerates the directory's contents to support **progressive disclosure** — letting a human or agent see what is available before opening individual documents. The body uses sections grouping concepts under headings:

```markdown
# Research Sources Index

## Primary Research Studies

* [BCG & Harvard AI Field Study](/sources/bcg-harvard-study.md) - Landmark randomized field study of 758 consultants.
* [MIT Media Lab Cognitive Debt Study](/sources/mit-cognitive-debt.md) - Research showing reduced brain activity with AI.

## Industry Perspectives

* [The Meter Picks the Winners](/sources/devlin-meter-picks-winners.md) - Token economics and routing discipline.
```

### 3.7 Log Files

`log.md` is a flat list of date-grouped entries, newest first:

```markdown
# Wiki Update Log

## 2026-06-29
* **Ingest**: Added BCG & Harvard AI Field Study → sources/bcg-harvard-study.md
* **Ingest**: Added MIT Media Lab Cognitive Debt Study → sources/mit-cognitive-debt.md
```

Date headings MUST use ISO 8601 `YYYY-MM-DD` form.

### 3.8 Conformance Summary

A bundle is conformant with OKF v0.1 if:
1. Every non-reserved `.md` file contains a parseable YAML frontmatter block.
2. Every frontmatter block contains a non-empty `type` field.
3. Every reserved filename follows the structure in sections 3.6 and 3.7.

---

## 4. Operations

### 4.1 Orient (CRITICAL — do this every session)

Before any operation, always orient:

1. **Read this file** (AGENTS.md) — understand structure and conventions.
2. **Read `sample-wiki/index.md`** — learn what pages exist and their summaries.
3. **Scan recent `sample-wiki/log.md`** — read the last 20-30 entries to understand recent activity.

This prevents duplicate pages, missed cross-references, and contradictions with the schema.

### 4.2 Ingest

When a new source (article, paper, blog post, transcript) is provided, integrate it into the wiki:

**Step 1 — Check for duplicates first.**
Search existing pages for the topic using `obsidian search query="topic keywords" path="sample-wiki"` or `search_files` across all `.md` files in `sample-wiki/`. If already ingested, inform the user and ask if supplementary information should be added.

**Step 2 — Read the source.**
Read the raw source material carefully. Raw sources live in `raw-sources/` relative to the repository root. Extract key findings, entities mentioned, and concepts introduced.

**Step 3 — Discuss takeaways with the user** (skip in automated contexts).
Summarize what you found. Ask which entities and concepts to extract. Get confirmation before creating pages.

**Step 4 — Create a source summary.**
- File: `sample-wiki/sources/[descriptive-slug].md`
- Frontmatter: `type: Reference`, `title`, `description`, `resource`, `tags`, `timestamp`
- Body sections: `# Summary`, `# Key Findings`, `# Implications for Developers`, `# Citations`
- This is the 1:1 summary of the source — the primary artifact of ingest

**Step 5 — Create or update entity pages.**
For each proper noun (person, organization, product, place):
- File: `sample-wiki/entities/[entity-slug].md`
- If exists: add new information, bump `timestamp`
- If new: write a complete entity page
- Minimum 2 cross-references to other wiki pages

**Step 6 — Create or update concept pages.**
For each common noun (technique, pattern, idea):
- File: `sample-wiki/concepts/[concept-slug].md`
- Same rules as entities

**Step 7 — Add cross-references.**
- Link the source summary to all entities and concepts it mentions
- Link entities to related concepts and vice versa
- Link to other source summaries where connections exist
- Ensure bidirectional links where appropriate
- Every new or updated page MUST have at least 2 outbound cross-references

**Step 8 — Update index.**
Regenerate or manually update `sample-wiki/index.md`:
- Add new pages under the correct section, alphabetically
- Include the one-line `description` from each page's frontmatter
- If Obsidian is available, use `obsidian search` to verify no pages were missed from the index

**Step 9 — Log the operation.**
Append to `sample-wiki/log.md`:
```markdown
## 2026-06-29
* **Ingest**: Added [Source Title] → sources/[slug].md
  - Created N entity pages
  - Created M concept pages
  - Added X cross-references
```

**Step 10 — Report what changed.**
List every file created or updated for the user.

A single source can trigger updates across 5-15 wiki pages. This is normal and desired — it is the compounding effect.

### 4.3 Query

When the user asks a question that requires synthesizing wiki knowledge:

1. **Read `sample-wiki/index.md`** to identify relevant pages.
2. For larger wikis (50+ pages), also search using `obsidian search query="key terms" path="sample-wiki"` or `search_files` across all `.md` files.
3. **Read the relevant pages.**
4. **Synthesize an answer** with citations using standard markdown links: "Based on [BCG Study](/sources/bcg-harvard-study.md) and [MIT Study](/sources/mit-cognitive-debt.md)..."
5. **File valuable answers back** — if the answer is a substantial comparison, deep dive, or novel synthesis, create a page in `sample-wiki/queries/[descriptive-slug].md`. Don't file trivial lookups.
6. **Log the operation.**

### 4.4 Lint

When the user asks to health-check or audit the wiki:

Prefer the Obsidian CLI for link and structure checks — it uses Obsidian's own resolver and is authoritative. Fall back to file scanning (`search_files`, `read_file`) when Obsidian is unavailable.

**Checks (by priority):**

1. **Broken links:** `obsidian unresolved verbose` — broken links across the wiki. Fall back to scanning all markdown link targets with `search_files` and verifying each exists.
2. **Missing frontmatter:** Verify every `.md` file (except `index.md` and `log.md`) starts with `---` and contains a `type:` field. Use `read_file` or `obsidian properties`.
3. **Orphan pages:** `obsidian orphans` — pages with no incoming links. Fall back to building an inbound link map by scanning all files.
4. **Dead-end pages:** `obsidian deadends` — pages with no outgoing links. Fall back to scanning for pages with fewer than 2 outbound links.
5. **Index completeness:** Every wiki page should appear in `index.md`. Compare the filesystem against index entries.
6. **Contradictions:** Pages on the same topic with conflicting claims. Look for pages that share tags/entities but state different facts.
7. **Stale content:** Pages whose `timestamp` is much older than other pages covering the same entities.
8. **Page size:** Flag pages over 200 lines — candidates for splitting.

**Report findings** grouped by severity (broken links > missing frontmatter > orphans > stale content > style issues). Propose specific fixes with file paths.

**Append to log.md:**
```markdown
## 2026-06-29
* **Lint**: N issues found
  - Fixed X broken links
  - Added Y cross-references to fix orphans
```

---

## 5. File Naming Conventions

- **Lowercase with hyphens**: `bcg-harvard-study.md`, `andrej-karpathy.md`, `cognitive-debt.md`
- **No date prefixes**: Wiki pages are timeless
- **Descriptive stems**: Filename should match the primary subject
- **No spaces, no special characters**: Use hyphens only

---

## 6. Decision Trees

### Entity vs Concept

**Entity (proper noun):**
- Specific person, organization, product, place
- Examples: Andrej Karpathy, OpenAI, GPT-4, MIT Media Lab
- File location: `entities/`

**Concept (common noun):**
- Technique, pattern, idea, category
- Examples: cognitive debt, cognitive surrender, token economics, jagged frontier
- File location: `concepts/`

**When in doubt:** If you can say "the" or "a" before it, it's a concept. If it's a unique name, it's an entity.

### New Page vs Update Existing

**Create new page when:**
- No existing page covers this entity/concept
- This would be a major subtopic deserving its own page

**Update existing when:**
- Page already exists for this entity/concept
- New source adds details, context, or corrections
- Information fits naturally into existing structure

**When in doubt:** Check `index.md` first, then search all files.

### Handling Contradictions

If sources disagree:
1. Document both perspectives in the relevant page
2. Cite sources for each claim
3. Note in the page body: "According to [Source A](/sources/source-a.md), X. However, [Source B](/sources/source-b.md) claims Y."
4. Flag for user review during lint

---

## 7. Quality Standards

### Good Wiki Page Criteria

**Must have:**
- Valid OKF frontmatter with non-empty `type`
- At least 2 cross-references to other wiki pages (standard markdown links)
- Structured headings (`#`, `##` sections)

**Should have:**
- Opening paragraph summarizing the topic
- Multiple perspectives (if applicable)
- Related concepts/entities section
- Tags that aid discovery

### What to Avoid

- **Orphans:** Pages with no incoming links (fix during lint)
- **Unsourced claims:** Always cite sources via `resource:` frontmatter or `# Citations`
- **Duplicates:** Check before creating new pages
- **Stale info:** Update pages when new sources arrive
- **Broken links:** Verify targets exist
- **Pages without cross-references:** Isolated pages are invisible

---

## 8. How to Extend This Wiki

To ingest a new source right now:

1. Read the source material
2. Follow the Ingest workflow (section 4.2) step by step
3. Use file tools (`read_file`, `write_file`, `patch`, `search_files`) and Obsidian CLI where available
4. Update `index.md` and `log.md` when done
5. Report every file created or updated

The wiki grows richer with every source. The maintenance cost is near zero because the agent handles the bookkeeping that humans abandon.

---

## 9. Tools

### File Tools (always available)

These are the baseline tools for reading, writing, and searching the wiki. They work everywhere with no dependencies.

- **read_file** — Read any wiki page, source file, or index
- **write_file** — Create new wiki pages
- **patch** — Edit existing wiki pages (targeted find-and-replace)
- **search_files** — Search content across files (regex) or find files by name (glob)

### Obsidian CLI (recommended when available)

Obsidian is the visualization layer for this wiki — graph view, backlinks, and the IDE experience described in the talk. The Obsidian CLI provides agent-accessible commands that use Obsidian's own link resolver, making them authoritative for link health checks.

If Obsidian is installed, prefer it for the operations below. If it is not available, the file-tool equivalents listed alongside each command work as fallbacks.

**Search:**
```bash
# Search wiki content with full Obsidian query syntax
obsidian search query="cognitive debt" path="sample-wiki"
obsidian search:context query="token economics" path="sample-wiki"
```

**Link health (authoritative — uses Obsidian's own resolver):**
```bash
obsidian unresolved verbose          # broken links across the wiki
obsidian orphans                     # pages with no incoming links
obsidian deadends                    # pages with no outgoing links
```

**Backlinks — see what links to a given page:**
```bash
obsidian backlinks file="bcg-harvard-study" counts
```

**Properties / frontmatter:**
```bash
obsidian properties path="sample-wiki/sources/bcg-harvard-study.md"
obsidian property:read name=tags file="bcg-harvard-study"
obsidian property:set name=timestamp value="2026-07-09T12:00:00Z" file="bcg-harvard-study"
```

**Tags across the wiki:**
```bash
obsidian tags folder="sample-wiki" counts sort=count
```

**Open a wiki page in Obsidian UI for live review:**
```bash
obsidian open path="sample-wiki/sources/bcg-harvard-study.md"
```

**Graph view:** The primary reason to use Obsidian with this wiki. Opening the `sample-wiki/` directory as an Obsidian vault gives you:
- **Graph View** — visualizes the knowledge network, showing what connects to what, which pages are hubs, which are orphans
- **Backlinks** — every page shows what links to it, bidirectional navigation
- **YAML frontmatter** — powers Dataview queries for dynamic tables and lists

This is the "Obsidian as the IDE" experience from the talk: the agent writes markdown; Obsidian renders the graph and connections in real time.

### Tool Selection Guide

| Task | Best Tool |
|------|-----------|
| Find broken links | `obsidian unresolved` (authoritative) |
| Find orphaned pages | `obsidian orphans` |
| Find isolated pages | `obsidian deadends` |
| Check what links to a page | `obsidian backlinks` |
| Search wiki content | `obsidian search` or `search_files` |
| Read a page | `read_file` |
| Create/edit a page | `write_file` / `patch` |
| Set frontmatter | `obsidian property:set` or `patch` |
| Visualize the graph | Open `sample-wiki/` in Obsidian |

---

## References

- **OKF-SPEC.md** — Full Open Knowledge Format v0.1 specification (at repo root)
- **karpathy-llm-wiki-gist.md** — Andrej Karpathy's original LLM Wiki pattern (at repo root)
