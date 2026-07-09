---
type: Reference
title: BCG & Harvard AI Field Study
description: Landmark randomized field study of 758 consultants showing that AI amplifies good work but makes out-of-boundary work worse.
resource: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4570074
tags: [ai-adoption, judgment, research]
timestamp: 2026-06-29T11:50:00Z
---

# Summary

Conducted by researchers from Harvard Business School, MIT, Wharton, and Boston Consulting Group (BCG), this study examined the real-world performance effects of OpenAI's GPT-4 on 758 consultants performing realistic management tasks. 

# Key Findings

- **Productivity Boost**: Consultants using AI completed 12.2% more tasks on average and completed them 25.1% faster.
- **Quality Amplification**: For tasks *within* the AI's capability frontier, the quality of results increased by 40% compared to a control group working without AI.
- **The Jagged Frontier Trap**: For tasks *outside* the AI's capability frontier (requiring qualitative reasoning or contextual judgment), consultants using AI were **19 percentage points less likely** to reach the correct answer compared to the control group.
- **Automation Bias & Falling Asleep at the Wheel**: Consultants who trusted the AI too much failed to verify its output, resulting in the propagation of confident-sounding but incorrect results.

# Implications for Developers

1. **Frontier Judgment**: Developers must understand where the boundaries of their AI model's capabilities lie (e.g., writing boilerplate vs. resolving architectural bottlenecks).
2. **Quality Gates**: Multi-step workflows require explicit, deterministic verification points to catch errors before they propagate.

# Citations

[1] Dell'Acqua, F., McFowland, E., Mollick, E., Lifshitz-Assaf, H., Choudhury, K., & Lakhani, K. R. (2023). "Navigating the Jagged Technological Frontier: Field Experimental Evidence of the Effects of AI on Knowledge Worker Productivity and Quality." HBS Working Paper.
