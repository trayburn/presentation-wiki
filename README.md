# Surviving the AI Deluge: Architecting an Agentic Memory with OKF and Obsidian

Presentation assets for the talk by **Tim Rayburn** — Vice President of Consulting, Improving.

Every Tuesday brings a new model release. Every Thursday brings another cognitive study. Every Friday brings a silent tokenizer repricing event. This talk shows how to deploy lightweight AI agents that ingest raw PDFs, blogs, and transcripts, transforming them into a structured semantic graph using the **Open Knowledge Format (OKF)** and **Obsidian**.

## Repository Contents

```
presentation-wiki/
├── README.md                          # You are here
├── Abstract.md                        # Session abstract and talk outline
├── OKF-SPEC.md                        # Full Open Knowledge Format v0.1 specification
├── karpathy-llm-wiki-gist.md          # Verbatim copy of Karpathy's original LLM Wiki gist
├── slides/
│   ├── generate_slides.py             # Python script that builds the deck (python-pptx)
│   └── Living-Context.pptx            # Pre-built presentation
└── sample-wiki/
    ├── index.md                       # Wiki index with progressive disclosure
    └── sources/
        ├── bcg-harvard-study.md       # BCG & Harvard AI field study (758 consultants)
        ├── mit-cognitive-debt.md      # MIT Media Lab cognitive debt study (EEG/recall)
        ├── wharton-cognitive-surrender.md  # Wharton cognitive surrender research
        └── devlin-meter-picks-winners.md   # Token economics and routing discipline
```

## Reference Documents

Two foundational documents live at the repository root for easy reference:

- **[OKF-SPEC.md](./OKF-SPEC.md)** — The complete Open Knowledge Format (OKF) v0.1 specification. OKF is an open, human- and agent-friendly format for representing knowledge as a directory of markdown files with YAML frontmatter. No schema registry, no central authority, no required tooling.

- **[karpathy-llm-wiki-gist.md](./karpathy-llm-wiki-gist.md)** — A verbatim copy of Andrej Karpathy's [LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), which describes the core pattern of building persistent, compounding knowledge bases with LLMs. This is the intellectual foundation for the talk.

## Sample Wiki

The `sample-wiki/` directory contains a working OKF-compliant knowledge bundle with four foundational AI research sources ingested as concept documents:

1. **BCG & Harvard AI Field Study** — 758 consultants, the jagged technological frontier
2. **MIT Media Lab Cognitive Debt Study** — EEG evidence of reduced brain activity with AI assistance
3. **Wharton Cognitive Surrender Research** — 80% follow-rate on faulty AI outputs
4. **The Meter Picks the Winners** — Token economics, repricing, and local owned floors

Each source follows OKF conventions: YAML frontmatter with `type`, `title`, `description`, `resource`, `tags`, and `timestamp`, followed by structured markdown with Summary, Key Findings, Implications, and Citations sections.

## Slides

The presentation deck is built programmatically using `python-pptx`:

```bash
cd slides
pip install python-pptx
python generate_slides.py
```

This regenerates `Living-Context.pptx` with 7 slides covering the problem, the Living Context pattern, OKF architecture, the ingestion canon, a live demo outline, and the Monday Morning Roadmap.

## Key Takeaways

- **Tame the Noise** — Build an automated, agent-driven ingestion pipeline that parses raw documents on arrival
- **OKF Architecture** — Structure knowledge in Git-friendly markdown that's readable by humans and parseable by LLMs
- **Obsidian as the IDE** — Use graph view and backlinks to visualize your team's collective memory
- **Hands-on** — Zero slideware. The ingestion pipeline, Obsidian graph, and local wiki queries run live

## Talk Outline

1. **Surviving the Daily Feed (5 min)** — The developer's daily flood of AI announcements
2. **The Architecture: OKF + Obsidian (10 min)** — Diffable markdown + YAML frontmatter
3. **Live Demo: Ingesting the Canon (20 min)** — Running the pipeline on four foundational studies
4. **Your Monday Morning Roadmap (5 min)** — Setting up a shared Git repo for team memory

## License

Presentation materials by Tim Rayburn. The OKF specification and Karpathy's gist are the property of their respective authors.
