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
Use this section for component-specific mapped context needed to interpret the supplied source/configuration, such as dispatch tables, policy mappings, parameter tables, resource mappings, state definitions, or other architecture/source relationships.

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
4) When reasoning about component-specific mappings or semantics supplied in Section (E), strictly adhere to that mapped context.
5) Distinguish:
   - guaranteed issues causing the routed regression, versus
   - additional guaranteed bugs in COMPONENT_UNDER_TEST that should still be reported because this component is modified in config2.
6) Explicitly reject tempting but unproven findings (for example, random assertions or conditionals that are not themselves provable functional defects under the supplied artifacts).

Component-specific context rule:
- If a guaranteed issue exists in COMPONENT_UNDER_TEST even when it is not the dominant direct cause in this simpoint, still report it because COMPONENT_UNDER_TEST is modified in config2.
- Use any mappings or semantics in Section (E) only to interpret active code paths and validate claims.
- Do not infer defects solely from the presence of mapped context; every reported issue must be proved from supplied source/config/counter evidence.

Required output format:

### 1) Routed behavior to explain
- Briefly restate ROOT_CAUSE_BEHAVIOR.
- If ALIGNED_CAUSAL_NARRATIVE is provided, name the key must-explain symptoms.

### 2) Counter evidence
- Give a compact table of the most relevant counters with config1, config2, delta, and a one-line explanation grounded in the provided counter semantics.

### 3) Relevant code paths
- List the key functions/files and why each is relevant.
- For each relevant code path or component mechanism, include when applicable:
  - Active path:
  - Relevant state/resource/policy object:
  - Decision or mutation site:
  - Triggering event or gating condition:
  - Later consumer or downstream effect:
  - Recovery, rollback, clear, or reset behavior:
- For every reported issue, include the reachable active-path proof.

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
