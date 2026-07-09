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
├── AGENTS.md                          # Agent operating manual for the wiki
├── raw-sources/                       # Raw source material — INGEST THESE during the demo
│   ├── bcg-harvard-jagged-technological-frontier.pdf   # BCG/HBS study (3 MB)
│   ├── mit-cognitive-debt.pdf                           # MIT Media Lab study (36 MB)
│   ├── wharton-cognitive-surrender.pdf                  # Wharton cognitive surrender (1.8 MB)
│   └── devlin-meter-picks-the-winners.md                # Devlin Liles blog post (15 KB)
├── slides/
│   ├── generate_slides.py             # Python script that builds the deck (python-pptx)
│   └── Living-Context.pptx            # Pre-built presentation
└── sample-wiki/                       # BLANK wiki — populate during live demo
    ├── index.md                       # Empty content catalog (will grow on ingest)
    ├── log.md                         # Action log (starts with initialization entry)
    ├── sources/                       # Source summaries will be created here
    ├── entities/                      # Entity pages will be created here
    └── concepts/                      # Concept pages will be created here
```

## Reference Documents

Two foundational documents live at the repository root for easy reference:

- **[OKF-SPEC.md](./OKF-SPEC.md)** — The complete Open Knowledge Format (OKF) v0.1 specification. OKF is an open, human- and agent-friendly format for representing knowledge as a directory of markdown files with YAML frontmatter. No schema registry, no central authority, no required tooling.

- **[karpathy-llm-wiki-gist.md](./karpathy-llm-wiki-gist.md)** — A verbatim copy of Andrej Karpathy's [LLM Wiki gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), which describes the core pattern of building persistent, compounding knowledge bases with LLMs. This is the intellectual foundation for the talk.

## Raw Sources

The `raw-sources/` directory contains the four foundational AI research documents that will be ingested into the wiki during the live demo. These are the immutable inputs — the agent reads them but never modifies them.

1. **bcg-harvard-jagged-technological-frontier.pdf** — "Navigating the Jagged Technological Frontier" (Dell'Acqua et al., BCG/HBS). Landmark field study of 758 consultants showing AI amplifies in-frontier work by 40% but degrades out-of-frontier accuracy by 19 percentage points.
2. **mit-cognitive-debt.pdf** — "Your Brain on ChatGPT: Accumulation of Cognitive Debt" (Kosmyna et al., MIT Media Lab). EEG study showing reduced brain activity and 83% recall failure among ChatGPT users.
3. **wharton-cognitive-surrender.pdf** — "Thinking—Fast, Slow, and Artificial" (Shaw & Nave, Wharton). 1,372-participant study showing 80% follow-rate on faulty AI outputs.
4. **devlin-meter-picks-the-winners.md** — "The Meter Picks the Winners" (Devlin Liles). Token economics, silent tokenizer repricing, and building a local owned floor.

## Blank Wiki (Live Demo Starting Point)

The `sample-wiki/` directory is a **blank OKF-compliant wiki skeleton** — it starts empty with just the navigational structure (`index.md`, `log.md`, and empty `sources/`, `entities/`, `concepts/` directories). During the live demo, the agent ingests each raw source one at a time, creating source summaries, entity pages, concept pages, and cross-references. The wiki grows live in front of the audience.

To run the demo:
1. Open `sample-wiki/` as an Obsidian vault
2. Point an AI agent at `AGENTS.md` and the raw sources
3. Ingest each source following the workflow in AGENTS.md
4. Watch the Obsidian graph view populate in real time

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

## Getting Started for Participants

```bash
git clone https://github.com/trayburn/presentation-wiki.git
```

Then in Obsidian:

1. **Open as vault**: Open Obsidian -> Open folder as vault -> select the `presentation-wiki` folder
2. **Pre-configured**: The repo includes shared Obsidian settings (graph view colors, enabled core plugins, link behavior) so it works out of the box with no manual setup
3. **Graph view**: Click the graph icon (or Ctrl+G / Cmd+G) — it starts empty because the wiki is blank
4. **Start ingesting**: Follow the instructions in [AGENTS.md](./AGENTS.md) with your AI agent. Point it at `raw-sources/` and the ingest workflow. As the agent creates wiki pages, the graph view will populate in real time.

No additional Obsidian plugins or configuration required. The `.obsidian/` folder in the repo handles:
- Core plugins enabled (graph, backlinks, search, file explorer, properties)
- Graph view color-coded by directory (sources = blue, entities = amber, concepts = purple)
- Link format set to shortest-path wikilinks
- Auto-update of links when files move

Machine-specific files (`workspace.json`, cache) are gitignored, so your local window layout won't clutter commits.

## Talk Outline

1. **Surviving the Daily Feed (5 min)** — The developer's daily flood of AI announcements
2. **The Architecture: OKF + Obsidian (10 min)** — Diffable markdown + YAML frontmatter
3. **Live Demo: Ingesting the Canon (20 min)** — Running the pipeline on four foundational studies
4. **Your Monday Morning Roadmap (5 min)** — Setting up a shared Git repo for team memory

## License

MIT for original work by Tim Rayburn — see [LICENSE](./LICENSE). Source articles, the OKF specification, Karpathy's gist, and the presentation deck are the property of their respective authors and are explicitly excluded from the MIT license. See the "Files Excluded from This License" section in LICENSE for the complete list.
