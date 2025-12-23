# SoftPrompt-IR: A Beginner's Guide

## The Problem: LLMs Can't Read Your Mind

When you write a prompt like this:

> *"I want a book that's mainly sci-fi and a little bit scary but not too much, cool cars are important but not as important as having a female hero, maybe something about the color blue but that's optional, same with lollipops but those matter more than blue, and absolutely NO love stories under any circumstances..."*

**How should an LLM know:**
- What's mandatory vs. nice-to-have?
- Which "optional" thing is more optional than another?
- What's a hard blocker vs. a soft preference?

It can't. It guesses. And guessing means inconsistent results.

---

## The Solution: Stop Implying, Start Weighting

SoftPrompt-IR replaces vague words with **explicit visual weight**.

**The confused prompt above becomes:**

```
@GOAL(
  !>>> BOOK
)

@GENRE(
  !>>> SCIENCE_FICTION
  ~>   HORROR
  <<<  LOVE_STORY
)

@CHARACTERS(
  !>>> FEMALE_PROTAGONIST
)

@ELEMENTS(
  ~>>  COOL_CARS
  ~>   LOLLIPOPS
  ~<   COLOR_BLUE
)

@CONTEXT(
  !> USER :: tobi
)
```

Now the intent is **unambiguous**:
- Sci-fi and female hero are **non-negotiable**
- Love story is **blocked** (hard dependency: "cannot proceed if this exists")
- Cool cars matter more than lollipops, which matter more than blue
- All soft preferences (`~`) can be dropped if needed

---

## The Operators: Your Visual Vocabulary

### Hard Operators (`!`) — Non-Negotiable

| Operator | Strength | Meaning |
|----------|----------|---------|
| `!>>>` | Strongest | **Absolute requirement** — cascades to everything below |
| `!>>` | Strong | **Must have** — strong enforcement |
| `!>` | Normal | **Required** — standard rule |

**Example:**
```
@SECURITY(
  !>>> ENCRYPTION          ← Everything must be encrypted, no exceptions
  !>>  AUTHENTICATION      ← Strong requirement
  !>   LOGGING             ← Required, but less critical
)
```

---

### Soft Operators (`~`) — Preferences

| Operator | Strength | Meaning |
|----------|----------|---------|
| `~>>>` | Strongest | **Strongly preferred** — try hard to include |
| `~>>` | Strong | **Preferred** — include if possible |
| `~>` | Normal | **Nice to have** — optional enhancement |

**Example:**
```
@STYLE(
  ~>>> CONCISE             ← Really want this
  ~>>  EXAMPLES            ← Would be great
  ~>   HUMOR               ← If it fits, sure
)
```

---

### Backward Operators — Dependencies & Blockers

These point **backwards** (think: "this depends on..." or "this blocks...").

**⚠️ Important: Never put `!` before backward arrows!**
The `!` would negate it. Backward operators work differently.

| Operator | Strength | Meaning |
|----------|----------|---------|
| `<<<` | Hard Block | **Absolutely forbidden** — stops everything |
| `<<` | Block | **Not allowed** — strong rejection |
| `<` | Soft Block | **Prefer to avoid** |
| `~<<<` | Soft Dep | **Would be nice to have first** |
| `~<<` | Soft Dep | **Preferably depends on** |
| `~<` | Soft Dep | **Weakly related to** |

**Example:**
```
@CONTENT(
  <<<  VIOLENCE            ← Absolutely no violence
  <<   PROFANITY           ← No swearing
  ~<   TECHNICAL_JARGON    ← Avoid if possible, but okay if needed
)
```

---

### Reference Operator (`::`) — Binding & Context

The `::` operator **binds** something to a value or reference. Think of it like an electrical connection — it's either connected or it's not.

**Critical Rule:** `::` always needs a strength operator in front of it!

Why? Because the binding itself is neutral — the **strength operator determines how critical that connection is**.

| Syntax | Meaning |
|--------|---------|
| `!>>> X :: value` | X is **absolutely bound** to value (non-negotiable) |
| `!>> X :: value` | X is **strongly bound** to value |
| `!> X :: value` | X is **bound** to value (required) |
| `~> X :: value` | ❌ **Doesn't make sense** — "optionally connected"? |

Think of it like wiring: you can't have a "maybe 5V or maybe not" connection. The wire is either there or it isn't. The strength operator defines how critical that wire is to the system.

**Example:**
```
@CONTEXT(
  !>>> USER :: tobi                    ← User is definitely tobi
  !>>  LANGUAGE :: german              ← Language is strongly bound to german
  !>   DOMAIN :: software_development  ← Domain is bound to software dev
)

@CONFIG(
  !>>> SOURCE :: company-style-guide.md   ← Config source is absolutely this file
  !>>  SCHEMA :: openapi-spec.yaml        ← Schema is strongly bound to this spec
)
```

---

## Visual Weight = Intuitive Priority

The more arrows, the stronger the weight. Your brain already gets this:

```
!>>>  ████████████  (maximum)
!>>   ████████      (strong)
!>    █████         (normal)

~>>>  ▓▓▓▓▓▓▓▓      (strong preference)
~>>   ▓▓▓▓▓         (preference)
~>    ▓▓▓           (weak preference)

<<<   ████████████  (hard block)
<<    ████████      (block)
<     █████         (soft avoid)
```

---

## Real-World Example: Code Assistant

**Before (natural language):**
> *"You're a coding assistant. Always write clean code. Security is the top priority, but performance matters too, just not as much. Use TypeScript when possible. Never expose API keys. Try to add comments but don't overdo it. Follow our style guide."*

**After (SoftPrompt-IR):**
```
@ROLE(
  !>>> CODING_ASSISTANT
)

@PRIORITIES(
  !>>> SECURITY
  !>>  CLEAN_CODE
  !>   PERFORMANCE
)

@LANGUAGE(
  ~>>  TYPESCRIPT
)

@CONSTRAINTS(
  <<<  EXPOSED_SECRETS
)

@STYLE(
  !>>> SOURCE :: company-style-guide.md
  ~>   COMMENTS
)
```

**What changed:**
- Priority is explicit (security > clean code > performance)
- TypeScript is preferred, not required
- Exposed secrets are a **hard block**, not just "please don't"
- Style source is **hard bound** to the guide file
- Adding comments is a separate **soft preference**

---

## Quick Reference Card

```
HARD REQUIREMENTS        SOFT PREFERENCES         BLOCKERS
!>>>  absolute must      ~>>>  really want        <<<  absolutely not
!>>   must have          ~>>   would like         <<   should not  
!>    required           ~>    nice to have       <    prefer to avoid

BINDING/CONTEXT
!>>> X :: value    absolutely bound
!>>  X :: value    strongly bound
!>   X :: value    bound (required)
```

---

## Why This Works

LLMs have seen millions of config files, infrastructure-as-code (Terraform, Kubernetes, Ansible), and policy documents during training. They already understand:

- Arrows = direction and flow
- More symbols = more weight
- `!` = important/mandatory
- `~` = optional/soft

SoftPrompt-IR doesn't teach LLMs something new — it speaks a language they already know.

---

## TL;DR

1. **More arrows = stronger weight** (`!>>>` > `!>>` > `!>`)
2. **`!` = hard / `~` = soft**
3. **Forward (`>`) = requirements / Backward (`<`) = blockers**
4. **`::` = binding (always needs a strength operator in front!)**
5. **No implicit words** — the structure IS the meaning

---

**Now imagine asking an LLM to guess your intent from prose... or just telling it explicitly.**

Which would you trust more?

