---
type: Reference
title: Wharton Cognitive Surrender Research
description: Wharton research showing that high trust in AI leads users to stop constructing answers entirely, resulting in high follow rates for incorrect outputs.
resource: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=xxxxxx
tags: [cognitive-surrender, judgment, research]
timestamp: 2026-06-29T11:52:00Z
---

# Summary

Conducted by researchers Shaw and Nave at the Wharton School, this study analyzed "cognitive surrender"—a phenomenon where individuals delegate the entire reasoning process to an AI, skipping the mental construction of the answer altogether. 

# Key Findings

- **Wholesale Transfer of Reasoning**: Cognitive surrender is distinct from automation bias. Instead of checking the AI's work with bias, the human stops formulating their own thoughts and immediately adopts the AI's output.
- **High Follow Rates on Errors**: In experiments with 1,372 participants:
  - **93%** followed the AI when its output was accurate.
  - **80%** continued to follow and accept the AI's output even when it was *faulty or incorrect*.
- **The Trust Multiplier**: Participants with high trust in AI were **nearly 3x more likely** to surrender their judgment to incorrect AI suggestions.
- **Incentive Insufficiency**: Introducing performance incentives (paying for correct answers) and direct corrective feedback reduced the rate of cognitive surrender, but *failed to eliminate it*, even among experienced users.

# Implications for Developers

1. **Vibe Coding Risk**: When developers experience tight deadlines or fatigue, they enter a state of cognitive surrender, accepting AI-generated pull requests and bug fixes without active validation.
2. **Structural Scaffolding**: Systems must force active developer judgment through tool constraints (e.g., prompt harnesses that require developers to choose between options or explain the code changes before execution).

# Citations

[1] Shaw, J., & Nave, G. (2025). "Cognitive Surrender: The Wholesale Delegation of Human Reason to AI Systems." Wharton School Working Paper.
