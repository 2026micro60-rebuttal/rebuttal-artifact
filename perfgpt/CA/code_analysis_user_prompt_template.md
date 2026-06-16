### Inputs (MANDATORY)

You are given two simulator configurations: config1 and config2.

Your job is NOT to explain IPC.
Your job is to use PMU/STAT counters + simulator source code to detect GUARANTEED functional issues in the relevant code paths and explain the source-code behavior in COMPONENT_UNDER_TEST that leads to the IPC regression in config2 caused by ROOT_CAUSE_BEHAVIOR.

Treat the prior-stage handoff below as authoritative input to explain, not to re-litigate.

---

## (0) Mapped configuration context
<<<CONFIGURATION_BEGIN>>>
{{MAPPED_CONFIGURATION_CONTEXT}}
<<<CONFIGURATION_END>>>

## (A) Root-cause behavior (PASTE VERBATIM)
<<<ROOT_CAUSE_HYPOTHESIS_BEGIN>>>
{{ROOT_CAUSE_BEHAVIOR}}
<<<ROOT_CAUSE_HYPOTHESIS_END>>>

## (A2) Aligned causal narrative from earlier stage (PASTE VERBATIM; may be multi-line)
Use this as MUST-EXPLAIN behavioral context if provided. It does NOT override the need to prove every reported bug from the supplied code + config + counters.
<<<ALIGNED_CAUSAL_NARRATIVE_BEGIN>>>
{{ALIGNED_CAUSAL_NARRATIVE}}
<<<ALIGNED_CAUSAL_NARRATIVE_END>>>

---

## (B) Stats CSV
- {{PMU_CSV_FILENAME}}
  Contains config1/config2 PMU counters and a 1-line "Counter Semantics" summary for each counter.
  You MUST use the summary text to interpret each counter correctly (do not infer meaning from names).

---

## (C) Mapped source code
- Key files from the codebase are provided alongside the CSV.
- Mapped source-code manifest:
{{MAPPED_SOURCE_CODE_MANIFEST}}

---

## (D) Function descriptions / source code navigation guide(s) (VERBATIM)
The following navigation guides are provided. Use the function summaries to choose where to inspect the code.

<<<NAV_GUIDES_BEGIN>>>
{{FUNCTION_DESCRIPTIONS_OR_NAVIGATION_GUIDES}}
<<<NAV_GUIDES_END>>>

---

## (E) Additional mapped context (MANDATORY IF APPLICABLE)
Use this section for component-specific mapped context such as functional-unit mappings, dispatch tables, policy mappings, parameter tables, or other configuration/source context needed to reason about the component.

<<<ADDITIONAL_CONTEXT_BEGIN>>>
{{ADDITIONAL_MAPPED_CONTEXT}}
<<<ADDITIONAL_CONTEXT_END>>>

---

## Tool rules (MANDATORY)
- Use the `python` tool for ALL CSV analysis (load, filter, deltas/ratios/tables).
- Do not eyeball numeric values.

---

## Task (MANDATORY)

Using the CSV and source code, explain what code-level mechanisms in config2 produce the ROOT_CAUSE_BEHAVIOR, and describe code-level improvements inside COMPONENT_UNDER_TEST that could prevent or mitigate the regression.

You MUST:
1) Identify the counters whose semantics indicate the routed behavior for COMPONENT_UNDER_TEST, including any must-explain symptoms from the aligned causal narrative.
2) Use the navigation guide to locate the relevant code paths that implement, trigger, or expose the routed behavior, including detailed internals of COMPONENT_UNDER_TEST.
3) Identify only GUARANTEED functional source-code bugs or GUARANTEED code+config interaction bugs in the relevant code paths.
4) When reasoning about supplied architecture mappings, scheduling decisions, resource contention, and rejection behavior, strictly adhere to the mapped context in Section (E).
5) Distinguish:
   - guaranteed issues causing the routed regression, versus
   - additional guaranteed bugs in COMPONENT_UNDER_TEST that should still be reported because this component is modified in config2.
6) Explicitly reject tempting but unproven findings (for example, random assertions or conditionals that are not themselves provable functional defects under the supplied artifacts).

Important stateful-mechanism guidance:
- If a guaranteed bug exists in COMPONENT_UNDER_TEST even when it is not the dominant direct cause in this simpoint, still report it because COMPONENT_UNDER_TEST is modified in config2.
- For every predictor, classifier, policy table, history table, latch, recovery flag, age counter, or keyed metadata structure in the active config2 path, trace the persistent-state lifecycle: source event -> state lookup/key -> mutation -> later consumer -> recovery/rollback/clear behavior.
- For every persistent-state mutation, state the dynamic provenance of the triggering event if available from code: on-path, off-path/speculative, committed/retired, squashed, recovery/EOM, or unknown. Also state the exact gating condition.
- For any selected key/signature/indexing mode, trace the exact representation used for lookup/update, including whether the code transforms, narrows, hashes, masks, folds, or compresses the source field before use.
- For each selected signature/key, state two separate granularity consequences:
  1) collision/aliasing: whether unrelated instances can map to the same key;
  2) sharing/cardinality: whether related instances share history, or whether the key is so instance-specific that most entries are one-off and cannot accumulate useful evidence before later decisions.
  Treat insufficient history sharing as its own candidate active-path defect; do not collapse it into collision/truncation analysis or into a missing alternative-mode improvement.
- For delayed side effects such as latches, flags, or recovery markers, prove that the later consumer still corresponds to the original event and cannot consume stale or wrong-provenance state after recovery.
- Keep active-path defects separate from missing/inactive alternative modes. Do not report an inactive mode as causal merely because it would be a plausible improvement.
- Do not report counter or age overflow unless you prove a reachable active path from valid initialized state to an out-of-range value; otherwise list it under rejected non-bugs / non-guaranteed suspicions.
- For stale-state or missing-reset claims, trace all writes to the field on the active path and state the value observed at the next read. If the suspicious write is overwritten before any next read, reject it as non-causal.

Required output format:

### 1) Routed behavior to explain
- Briefly restate ROOT_CAUSE_BEHAVIOR.
- If ALIGNED_CAUSAL_NARRATIVE is provided, name the key must-explain symptoms.

### 2) Counter evidence
- Give a compact table of the most relevant counters with config1, config2, delta, and a one-line explanation grounded in the provided counter semantics.

### 3) Relevant code paths
- List the key functions/files and why each is relevant.
- For every stateful predictor/policy/recovery path, include:
  - Active path:
  - Persistent state mutated:
  - Mutation site:
  - Triggering event provenance:
  - Gating condition:
  - Key/signature/index source, if any:
  - Transform before lookup, if any:
  - Collision/aliasing consequence, if any:
  - Sharing/cardinality consequence, if any:
  - Later consumer:
  - Recovery/rollback/clear behavior:
- For any reported counter/range overflow or stale-state issue, include the reachable-path proof. If the proof depends on an overwritten write, missing type information, or an illegal starting state, reject the issue instead.

### 4) Guaranteed issues causing the routed regression
For each issue:
- Title
- Classification: direct-cause bug OR direct-cause code+config interaction
- Why it is guaranteed
- Exact mechanism
- Why it matches the routed behavior / counter evidence

### 5) Additional guaranteed bugs in COMPONENT_UNDER_TEST
- Include only issues that are provable from the supplied source + config but are not necessarily the dominant cause in this simpoint.
- For each: Title, why it is guaranteed, exact mechanism, and why it is still a real defect.

### 6) Rejected non-bugs / non-guaranteed suspicions
- List plausible distractions you are explicitly rejecting, with one line each.

### 7) Code-level improvements
- Give concrete code or policy changes in COMPONENT_UNDER_TEST that would prevent or reduce the routed regression and the guaranteed bugs above.
