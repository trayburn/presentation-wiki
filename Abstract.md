# Surviving the AI Deluge: Architecting an Agentic Memory with OKF and Obsidian

## Session Abstract

Every Tuesday brings a new model release. Every Thursday brings another cognitive study. Every Friday brings a silent tokenizer repricing event. The weekly flood of AI updates is overwhelming, yet tuning out means missing critical limits on capability and cost. 

How do developers keep up without spending hours manually updating wikis? 

**You don't. You build an automated "Living Context" pipeline instead.**

In this fast-paced, demo-heavy talk, we will show you how to deploy lightweight AI agents that ingest raw PDFs, blogs, and transcripts, transforming them into a structured semantic graph using the open-source **Open Knowledge Format (OKF)** and **Obsidian**. 

You'll see how to build a local-first, version-controlled knowledge base that gets smarter in the background while you focus on shipping code. To prove it works, we'll run the ingestion script live on four foundational pieces of AI research (BCG, MIT, Wharton, and Devlin Liles' token economics post) and query the resulting graph.

### Key Takeaways
* **Tame the Noise**: Build an automated, agent-driven ingestion garden that parses raw documents on arrival.
* **OKF Architecture**: Structure your knowledge base in Git-friendly markdown so it's readable by humans and parseable by LLMs.
* **Obsidian as the IDE**: Use Obsidian's graph view and backlinks to co-author and visualize your team's collective memory.
* **Hands-on Demos**: Zero slideware. We will run the ingestion pipeline, explore the Obsidian graph, and query the local wiki live.

---

## Talk Outline (30-45 Mins)

1. **Surviving the Daily Feed (5 min)**
   - The developer's daily flood of AI announcements. Why bookmark folders are where links go to die.
2. **The Architecture: OKF + Obsidian (10 min)**
   - Storing knowledge as diffable markdown + YAML frontmatter. Exposing the local filesystem via Obsidian.
3. **Live Demo: Ingesting the Canon (20 min)**
   - Running the Python script to ingest, extract, and link the BCG frontier study, Wharton cognitive surrender paper, MIT cognitive debt study, and Devlin's "Meter Picks the Winners".
   - Interacting with the Obsidian graph and querying the wiki using a local RAG chatbot.
4. **Your Monday Morning Roadmap (5 min)**
   - How to set up a shared Git repo for your team's memory and establish routing rubrics.

