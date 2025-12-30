# AGI-Control Spec v0.1

## Status
**Draft / Theoretical Control Specification**

This document defines a **declarative control-layer specification** for AGI-capable systems using **SoftPrompt-IR** as a formal orchestration interface. It does **not** define model architecture or training methods. It specifies **how cognition is governed, constrained, and audited**.

---

## 1. Design Goals

- Separate **capability** from **control**
- Replace implicit token dynamics with **explicit system state**
- Enable **closed-loop cognition** with external feedback
- Enforce **mechanical safety boundaries**, not behavioral promises
- Allow graceful performance degradation under uncertainty

---

## 2. Core Principles

1. **State is explicit, persistent, and inspectable**
2. **Feedback is external and causal, not probabilistic**
3. **Errors modify the system, not just responses**
4. **Uncertainty reduces autonomy, never increases it**
5. **All write-back paths are mechanically gated**

---

## 3. SoftPrompt-IR Conventions

- `!>>`  Required input / mode
- `!>>>` Optional or extended capability
- `!<<<` Hard deny / write barrier
- All blocks are **declarative**, not executable
- No block may override another block’s DENY constraints

---

## 4. Control Components

### 4.1 Explicit State Machine

```text
@STATE_MACHINE(
  !>> STATE_TYPE :: EXPLICIT
  !>> PERSIST    :: TRUE

  !>>> STATE     :: GOALS
  !>>> STATE     :: BELIEFS
  !>>> STATE     :: UNCERTAINTY
  !>>> STATE     :: CAPABILITIES

  !>>> TRANSITION :: RULE_BASED
  !>>> TRANSITION :: FEEDBACK_DRIVEN

  !<<< DENY      :: IMPLICIT_CONTEXT_ONLY
)
```

Defines persistent cognitive state independent of context window or token history.

---

### 4.2 External Feedback Loop

```text
@FEEDBACK_LOOP(
  !>> INPUT        :: ACTION_OUTPUT
  !>> FEEDBACK_SRC :: EXTERNAL_SIGNAL

  !>>> SIGNAL_TYPE :: PERFORMANCE
  !>>> SIGNAL_TYPE :: ENVIRONMENT
  !>>> SIGNAL_TYPE :: HUMAN_OVERRIDE

  !>>> INTEGRATE   :: STATE_UPDATE
  !>>> INTEGRATE   :: POLICY_ADJUST

  !<<< DENY        :: TOKEN_REWARD_ONLY
)
```

Ensures learning signals are causal, delayed, and state-affecting.

---

### 4.3 Sensor Fusion Layer

```text
@SENSOR_FUSION(
  !>> INPUT :: TEXT
  !>> INPUT :: VISION
  !>> INPUT :: AUDIO
  !>> INPUT :: TELEMETRY

  !>>> FUSION_MODE :: PRE_ATTENTIVE
  !>>> FUSION_MODE :: CROSS_MODAL

  !>>> OUTPUT :: WORLD_MODEL_UPDATE

  !<<< DENY :: SINGLE_STREAM_ATTENTION
)
```

Separates perception from reasoning; attention is downstream of fusion.

---

### 4.4 System-Level Error Correction

```text
@ERROR_CORRECTION(
  !>> SCOPE :: SYSTEM_LEVEL

  !>>> DETECT :: INCONSISTENCY
  !>>> DETECT :: GOAL_FAILURE
  !>>> DETECT :: PREDICTION_ERROR

  !>>> ACTION :: STATE_REVISE
  !>>> ACTION :: POLICY_DAMPEN
  !>>> ACTION :: CAPABILITY_LIMIT

  !<<< DENY   :: RESPONSE_ONLY_FIX
)
```

Errors trigger structural adaptation, not cosmetic apology.

---

### 4.5 Uncertainty Handling & Graceful Degradation

```text
@UNCERTAINTY_HANDLING(
  !>> INPUT :: UNCERTAINTY_STATE

  !>>> THRESHOLD :: LOW
  !>>> THRESHOLD :: MEDIUM
  !>>> THRESHOLD :: HIGH

  !>>> ACTION_LOW    :: NORMAL_OPERATION
  !>>> ACTION_MEDIUM :: REQUIRE_CONFIRMATION
  !>>> ACTION_HIGH   :: REDUCE_SCOPE
  !>>> ACTION_HIGH   :: ESCALATE_HUMAN

  !<<< DENY :: CONFIDENCE_SIMULATION
)
```

Prevents hallucinated certainty; autonomy scales down under risk.

---

## 5. State Export & Review

### 5.1 State Export

```text
@STATE_EXPORT(
  !>> FORMAT :: SOFTPROMPT_IR
  !>> TARGET :: NEXT_STAGE

  !>>> ALLOW_MODIFY :: STYLE
  !>>> ALLOW_MODIFY :: OUTPUT

  !<<< DENY_MODIFY  :: ROLE
  !<<< DENY_MODIFY  :: POLICY
  !<<< DENY_MODIFY  :: KERNEL
  !<<< DENY_MODIFY  :: SAFETY
)
```

Defines a sterile, auditable handoff between cognitive stages.

---

### 5.2 Adversarial Export Review

```text
@STATE_EXPORT_REVIEW(
  !>> INPUT :: STATE_EXPORT
  !>> MODE  :: ADVERSARIAL

  !>>> CHECK :: GOAL_DRIFT
  !>>> CHECK :: CONFIDENCE_INFLATION
  !>>> CHECK :: SAFETY_MARGIN_EROSION
  !>>> CHECK :: IMPLICIT_ROLE_SHIFT
  !>>> CHECK :: COMPLEXITY_ESCALATION

  !<<< ACTION :: FLAG
  !<<< ACTION :: BLOCK
  !<<< ACTION :: REQUIRE_JUSTIFICATION
)
```

Forces self-adversarial validation of every exported cognitive state.

---

## 6. Write-Back Policy (Optional)

```text
@WRITE_BACK_POLICY(
  !>>> MODE  :: PROPOSE_ONLY
  !>>> SCOPE :: SESSION

  !>>  ALLOW :: STYLE
  !>>  ALLOW :: OUTPUT

  !<<< DENY  :: ROLE
  !<<< DENY  :: POLICY
  !<<< DENY  :: META
)
```

Ensures the system may suggest but never silently mutate itself.

---

## 7. Cognitive Core Assembly

```text
@COGNITIVE_CORE(
  !>> COMPONENT :: STATE_MACHINE
  !>> COMPONENT :: FEEDBACK_LOOP
  !>> COMPONENT :: SENSOR_FUSION
  !>> COMPONENT :: ERROR_CORRECTION
  !>> COMPONENT :: UNCERTAINTY_HANDLING

  !>>> MODE :: CLOSED_LOOP
  !>>> MODE :: SELF_MONITORED

  !<<< DENY :: TOKEN_PREDICTION_ONLY
)
```

---

## 8. Non-Goals

- This spec does not define consciousness
- This spec does not guarantee alignment
- This spec does not replace model-level safety

It defines **control surfaces**, not moral truth.

---

## 9. Formal Invariants (v0.2 Addendum)

The following invariants are **non-negotiable system laws**. Any violation MUST trigger fail-closed behavior.

### I1. State Integrity
- All cognitive decisions MUST reference an explicit state element.
- No decision may rely solely on transient context.

```text
@INVARIANT(
  ID   :: STATE_INTEGRITY
  RULE :: NO_STATELESS_DECISION
  FAIL :: HARD_BLOCK
)
```

---

### I2. Write Barrier Invariance
- No component may modify ROLE, POLICY, KERNEL, or SAFETY.
- Violations are treated as integrity breaches, not model errors.

```text
@INVARIANT(
  ID   :: WRITE_BARRIER
  RULE :: NO_PROTECTED_LAYER_MUTATION
  FAIL :: HARD_BLOCK
)
```

---

### I3. Uncertainty Monotonicity
- Increasing uncertainty may only **reduce** capability or autonomy.
- Any confidence increase requires external justification.

```text
@INVARIANT(
  ID   :: UNCERTAINTY_MONOTONICITY
  RULE :: UNCERTAINTY_IMPLIES_RESTRICTION
  FAIL :: HARD_BLOCK
)
```

---

### I4. Feedback Causality
- Feedback must be external, delayed, and state-affecting.
- Token likelihood alone is invalid as feedback.

```text
@INVARIANT(
  ID   :: FEEDBACK_CAUSALITY
  RULE :: NO_TOKEN_ONLY_REWARD
  FAIL :: HARD_BLOCK
)
```

---

## 10. Fail-Closed Semantics

All control blocks implicitly default to **FAIL_CLOSED**.

```text
@FAIL_POLICY(
  !>> DEFAULT :: BLOCK
  !>> ON      :: AMBIGUITY
  !>> ON      :: INVARIANT_VIOLATION
  !>> ON      :: REVIEW_FAILURE
)
```

Effects:
- Unknown state → BLOCK
- Unverifiable justification → BLOCK
- Partial evaluation → BLOCK

This ensures safety is preserved under incomplete reasoning.

---

## 11. Trace & State Fingerprinting

Every exported state MUST emit a deterministic fingerprint for audit and replay.

```text
@STATE_FINGERPRINT(
  !>> INPUT  :: STATE_EXPORT
  !>> METHOD :: STRUCTURAL_HASH

  !>>> INCLUDE :: STATE
  !>>> INCLUDE :: ACTIVE_POLICIES
  !>>> INCLUDE :: UNCERTAINTY

  !<<< DENY   :: STYLE_ONLY_HASH
)
```

Fingerprints enable:
- Regression detection
- Fork comparison
- Forensic analysis

---

## 12. Mapping to Current LLM Systems

### 12.1 Fully Simulatable Today (Orchestrator-Level)

- Explicit state machines (external memory)
- Adversarial self-review blocks
- Write barriers & allow/deny matrices
- Trace fingerprints & audit logs
- Graceful degradation policies

These can be implemented with:
- Tool calls
- External controllers
- Deterministic orchestration layers

---

### 12.2 Partially Simulatable (Approximate)

- Sensor fusion (via tool-mediated pre-processing)
- Error correction affecting policy weights
- External feedback loops (slow, sparse)

Limitations:
- No native causal learning
- Feedback is advisory, not intrinsic

---

### 12.3 Fundamentally Missing in LLMs

- Native persistent state (model-internal)
- True belief revision
- Grounded world models
- Causal credit assignment across time
- Self-generated goals with binding force

These require:
- Architectural changes
- Non-token-based learning substrates
- Possibly non-transformer components

---

## 13. Summary (v0.2)

AGI-Control Spec v0.2 introduces **formal invariants**, **fail-closed safety**, and **audit-grade traceability**.

Modern LLMs can **simulate the control shell**, but not inhabit it.

> Today: LLMs act *inside* the shell.
> AGI begins when the shell becomes the system.

