### Required evidence sources (MANDATORY)
1) {{PMU_CSV_FILENAME}}
   - Contains config1 and config2 values for many PMU counters and a 1-line summary of each counter.
   - You MUST use the 1-line summary to interpret counter semantics. Do not assume meanings from the counter name alone.
2) The microarchitectural change in config2 (below).

### Definitions (MANDATORY)
- Periodic IPC is Periodic_Instructions / Periodic_Cycles.
- "Regression" means Periodic_IPC decreases because Periodic_Cycles increase at (approximately) constant Periodic_Instructions.

### Task
Explain the ~{{IPC_REGRESSION_PERCENT}} decrease in Periodic IPC in config2 vs config1.

You must:
(1) Identify the single best "direct cause" of the regression (must be one of the 20 candidates) using the causal-graph method.
(2) Assess whether the microarchitectural change plausibly contributes to that direct cause.

IMPORTANT:
- It is VALID to conclude that the microarchitectural change is NOT SUPPORTED (or is CONTRADICTED) as a cause of the chosen direct cause, if the CSV evidence does not justify the link.
- You MUST still complete Cause->Effects reasoning for the direct cause even if Change->Cause attribution is unsupported.

---

## Selected counters (Top 20, CSV order; 1 per line)
{{SELECTED_COUNTERS_TOP20_CSV_ORDER}}

IMPORTANT (MANDATORY):
- You MUST use EXACTLY the 20 counters above as the candidate set (do NOT add/remove/replace candidates).
- You MAY use the full CSV (other counters + summaries) to support reasoning and validate links, but:
  - the chosen direct cause MUST be among the 20 candidates.

---

## Causality framework (MANDATORY)

### Relationship categories (you MUST use exactly these)
1) Cause -> Effect:
2) No causal chain

### IMPORTANT: "Cause" may be forward OR backward via backpressure (MANDATORY)
- A valid Cause -> Effect chain may proceed in the "forward" direction (e.g., misses/latency -> longer service time -> downstream stalls),
  OR it may propagate "backward" through the pipeline/memory system via backpressure (e.g., a downstream queue/buffer becomes full/denies requests -> upstream stalls/starvation).
- In backpressure cases, it is valid (and often correct) for the CAUSE counter to describe a downstream saturation/denial/full condition,
  and the EFFECT counter to describe upstream blocked/stall/starve behavior.

### Strict rule for labeling Cause -> Effect (MANDATORY)
You may label A -> B ONLY if EITHER the counter summaries OR standard microarchitectural reasoning not inconsistent with those summaries justify that directionality.
Summary cues that can justify A -> B include phrases like:
- "blocked by ...", "stalled due to ...", "waiting for ...", "denied because ...", "buffer full ...", "requests rejected by ..."
- "could have been avoided if ...", "late prefetch ...", "bandwidth limited ..."
If summaries OR microarchitectural reasoning not inconsistent with the summaries do not justify directionality, you MUST label "No causal chain" (even if correlated).

---

## Direct cause (STRICT definition - MUST follow)
The "direct cause" is the single earliest root bottleneck downstream of the config2 change that best explains the IPC drop.
It must satisfy ALL of the following:
1) It is one of the 20 selected counters (candidate set).
2) It is the most "upstream/root" explanation of the regression among the 20 counters based on the causal graph you constructed.
   - The root may manifest as either:
     - a forward bottleneck (e.g., increased latency/misses/bandwidth limit that drives later stalls), OR
     - a backpressure source (e.g., downstream queue/buffer full/denials that propagates backward and blocks upstream).
IMPORTANT:
- Pure stall-symptom counters (e.g., "retirement blocked...", "frontend stalled...") are NOT eligible as the direct cause if a more root counter among the candidates explains them via Cause -> Effect.

---

## Required method (MANDATORY)

### Step 1 - Quantify the top-level regression
Using python:
- Compute IPC for config1 and config2 using Periodic_Instructions and Periodic_Cycles.
- Report absolute and % deltas for instructions, cycles, and IPC.

### Step 2 - Candidate counter accounting (restricted to the selected 20)
Using python:
- For each of the 20 selected counters, present:
  - config1 value, config2 value, delta, % delta,
  - the 1-line summary (quoted exactly).

### Step 3 - Build the causal graph over the 20 candidates (MANDATORY)
- Determine relationships among the 20 candidates:
  - Label each proposed relationship as either Cause -> Effect or No causal chain.
- Produce a directed causal graph (edge list or adjacency list) containing ONLY Cause -> Effect edges among the 20 candidates.
- Every Cause -> Effect edge MUST cite the specific summary phrase(s) that justify it.
- Your graph and narrative MUST consider both forward causality and backward/backpressure causality where supported by summaries.

### Step 4 - Choose exactly ONE direct cause (MANDATORY)
- Select exactly one counter from the 20 candidates as the direct cause (per the strict definition above).

### Step 5 - Explain Change -> Cause (MANDATORY)
Explain how the config2 microarchitectural change leads to the chosen direct cause.
Requirements:
- Ground the explanation in the observed deltas and counter summaries.
- Provide a plausible event chain: config2 microarchitectural change -> chosen direct cause increases.

### Step 6 - Explain Cause -> Effects (MANDATORY)
Explain how the direct cause generates the rest of the observed regression effects (among the candidates),
and how those effects collectively increase Periodic_Cycles at (approximately) constant Periodic_Instructions.

---

## Output format (MANDATORY)
1) Top-level IPC accounting (instructions, cycles, IPC) with computed deltas.
2) Candidate counters table (the 20 selected counters) including each counter's summary.
3) Causal classification:
   - list of Cause -> Effect edges (with summary justification),
   - list of key pairs labeled No causal chain (at least 5 examples where correlation might tempt a wrong causal claim).
4) Direct cause selection:
   - the chosen direct cause,
   - downstream coverage proof (chains),
   - why alternatives are less "upstream/root" or have less coverage.
5) Attribution verdict (MANDATORY):
- Provide one of: VERIFIED | INCONCLUSIVE | CONTRADICTED for "change -> direct cause".
- List:
  (a) the evidence counters used to support attribution (if VERIFIED),
  (b) the falsifying counters (if CONTRADICTED),
  (c) the missing corroborators (if INCONCLUSIVE).
6) Narrative explanation:
   - (a) Change -> Cause (config2 microarchitectural change -> direct cause),
   - (b) Cause -> Effects (direct cause -> downstream counter changes -> extra cycles -> IPC drop).
7) Final single-sentence diagnosis:
   - "Config2 regresses because <direct cause> which causes <primary effects>, increasing cycles by <X%> at constant instructions."

---

## The microarchitectural change in config2 (MANDATORY)
{{CHANGED_COMPONENTS_CONTEXT}}
