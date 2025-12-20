# SoftPrompt-IR: Low-Level Intent & Direction Annotation for LLM Prompts

**SoftPrompt-IR** is not a DSL (Domain Specific Language), not a framework, and not a policy system. It is a **minimal symbolic layer** for explicitly representing intent strength, direction, and priority within prompts.

---

## Why SoftPrompt-IR Exists

LLMs already process implicit signals such as:

*   Priority
*   Intent
*   Hierarchy
*   Strength
*   Avoidance vs. Enforcement

Currently, however, these signals are:

*   Implicit
*   Buried in natural language flow
*   Easily obfuscated
*   Difficult to interpret (for both humans and models)

**SoftPrompt-IR makes these signals explicit.**

This is not done to gain more control, but to **reduce ambiguity, attack surface, and unintended behavior.**

---

## Core Idea

SoftPrompt-IR uses small symbolic operators to annotate:

*   How strong something is
*   The direction it pushes toward
*   Whether it enforces or avoids something

It defines **no execution logic**, but **only the intent structure.**

### Example

```
@TASK(
  "Write a short story about a failed mission."
)

~<<<purple_prose
~<<cliches
~<over_explaining
```

**Interpretation:**

*   Avoid excessive purple prose (strong)
*   Avoid clichés (moderate)
*   Avoid over-explaining (gentle)

There are **no prohibitions** and **no hard rules**, only **probability shaping.**

---

## Intuition for Direction & Strength

SoftPrompt-IR is based on visual and positional intuition:

1.  **More symbols = More weight**
2.  **Direction** is crucial
3.  **Prefix position** implies dominance

**Examples:**

*   `!>>`: Mandatory, non-negotiable
*   `~<`: Gentle avoidance / de-escalation
*   `~<<<`: Strong stylistic avoidance
*   `>>!`: Sentence or local constraint

This works because:

*   Humans grasp these symbols instantly.
*   LLMs already model similar latent concepts.

---

## What SoftPrompt-IR Is Not

SoftPrompt-IR is **not**:

*   A jailbreaking technique
*   A prompt hacking tool
*   A policy bypass
*   A replacement for classifiers or filters
*   A new programming language

In fact, the goal is often the opposite: **It aims to make unsafe or contradictory intentions harder to conceal.**

---

## Why Low-Level Matters

Because SoftPrompt-IR operates **below** existing prompt engineering conventions:

*   It can be combined with any prompt
*   It does not fight existing safety systems
*   It can be safely ignored
*   It degrades gracefully

Low-level primitives tend to:

*   Last longer
*   Integrate more easily
*   Fail more predictably

---

## Potential Applications

*   Structuring system prompts
*   Safety and governance research
*   Prompt evaluation and testing
*   Control in creative writing
*   Prompting in high-risk domains (theoretically)
*   Debugging prompt behavior

---

## Design Philosophy

The philosophy of SoftPrompt-IR prioritizes:

*   **Explicit** over Implicit
*   **De-escalation** over Prohibition
*   **Structure** over Cleverness
*   **Readability** over Compression
*   **Compatibility** over Control

---

## Status

This is an **experimental specification**. There are **no guarantees**, **no enforcement**, and **no hidden magic.**

It is simply a small idea that seems to work surprisingly well.

**Final Note:**

If you think, "This is obvious—why didn't this exist already?" then that is the point. 🙂
