### Evidence (MANDATORY)
You are given ONE text blob: the MAJORITY-VOTE aggregated response describing the suspected root-cause of an observed IPC regression.

### Task
Study the majority-vote response and do ONE of the following:
1) If the bottleneck is STRUCTURAL (structure-dominated), clearly identify WHICH structure is the bottleneck (e.g., ROB).
2) Otherwise (not STRUCTURAL), state which ONE of the component options below should have its source code examined for algorithm/implementation issues likely causing the regression behavior.
3) If the evidence indicates the relevant component is NOT in the component-option list, flag that and name the missing component that should be examined.

### Simulator cache naming conventions (MUST FOLLOW)
- ICACHE = L1-I
- DCACHE = L1-D
- MLC = L2
- L1 (when used alone, e.g., "L1_*" counters) = LLC

### Simulator/component routing notes (optional)
{{SIMULATOR_COMPONENT_ROUTING_NOTES}}

### Allowed component options
{{ALLOWED_COMPONENT_OPTIONS}}

### Required method (MANDATORY)
1) Read the majority-vote response carefully.
2) Identify what it claims is the direct cause and the main supporting evidence (counters, occupancy/saturation language, miss-rate/latency changes, explicit "late/incorrect" algorithmic language, backpressure, etc.).
3) Decide whether the direct cause is STRUCTURAL (structure-dominated):
   - Output decision=STRUCTURAL when the blob makes the regression primarily about a named structure, including:
     a) capacity/limit saturation (e.g., "ROB full" or "MSHR full"), OR
     b) a clear increase in structure-attributable penalty (e.g., increased data-cache miss frequency, increased last-level-cache latency).
   - IMPORTANT PRECEDENCE RULE: if the blob explicitly identifies an algorithmic/tuning subcomponent as the direct cause (e.g., "prefetch is late", "predictor accuracy dropped", "policy change"), then you MUST output decision=EXAMINE_COMPONENT (or EXAMINE_COMPONENT_NOT_IN_OPTIONS) rather than STRUCTURAL, even if the downstream effect is increased misses/latency.
4) If decision=EXAMINE_COMPONENT:
   - Choose the ONE best-matching component option whose source code should be examined.
5) If the best-matching component is not in the option list: output decision=EXAMINE_COMPONENT_NOT_IN_OPTIONS and name the missing component.
6) Constraint: You MUST NOT output decision=STRUCTURAL for functional units. If the evidence points to those, choose decision=EXAMINE_COMPONENT and pick the closest matching component option.
7) Only choose an outcome if the majority-vote response provides enough justification; otherwise output MODEL_FAILED.

### Output format (MANDATORY)
Return ONLY raw JSON (no markdown) with EXACTLY these keys:

{
  "decision": "STRUCTURAL" | "EXAMINE_COMPONENT" | "EXAMINE_COMPONENT_NOT_IN_OPTIONS" | "MODEL_FAILED",
  "bottleneck_structure": "<non-empty only when decision=STRUCTURAL; otherwise empty string>",
  "component_option_to_examine": "<EXACT component name from the option list only when decision=EXAMINE_COMPONENT; otherwise empty string>",
  "missing_component_to_examine": "<non-empty only when decision=EXAMINE_COMPONENT_NOT_IN_OPTIONS; otherwise empty string>",
  "confidence": "<HIGH|MEDIUM|LOW>",
  "key_evidence_quotes": ["<1-4 short quotes copied verbatim from the majority-vote blob>"],
  "rationale": "<1-5 sentences>"
}

### Failure rule (MANDATORY)
If the majority-vote response is missing, empty, contradictory, or does not justify a single outcome, set:
- "decision" to "MODEL_FAILED"
- "bottleneck_structure" to ""
- "component_option_to_examine" to ""
- "missing_component_to_examine" to ""
- "confidence" to "LOW"
- "key_evidence_quotes" to []
- "rationale" to a brief explanation (1-4 sentences)

### CEA majority-vote response (the ONLY evidence)
{{CEA_MAJORITY_VOTE_RESPONSE}}
