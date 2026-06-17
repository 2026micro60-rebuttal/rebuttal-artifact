### Required evidence sources (MANDATORY)
1) {{PMU_CSV_FILENAME}}
   - Contains config1 and config2 values for many PMU counters and a 1-line summary of each counter.

### Task
Explain the ~{{IPC_REGRESSION_PERCENT}} decrease in Periodic IPC (Periodic_Instructions / Periodic_Cycles) in config2 compared to config1.

IMPORTANT: This is STEP 1 of 2.
Your ONLY goal is to compute the top-level IPC regression and then select the 20 most significant counters to investigate later.

---

## Required method (MANDATORY)

### Step 1 - Quantify the top-level regression
Using python:
- Compute IPC for config1 and config2 using Periodic_Instructions and Periodic_Cycles.
- Report absolute and % deltas for:
  - Periodic_Instructions
  - Periodic_Cycles
  - Periodic_IPC (computed from instructions/cycles; also report the Periodic_IPC counter if present)

### Step 2 - Select the 20 most significant counters (THIS STEP IS THE GOAL)
Using python, standard Comp Arch knowledge, the counter summaries and most importantly, the soft heuristics below:
- Select exactly 20 counters that can best help explain the IPC regression.
- Use a soft heuristic based on keyword substring matching in counter names (and sometimes in summaries), such as:
  - "queue full ...", "cache miss ...", "FU busy ...", "rejected ...", "pref late ...", "denied ...", "stall ...", "starve ...", "blocked ...", "mispredicted ..."
- You MUST use the 1-line summary to interpret counter semantics; do not assume meanings from counter names alone.

Selection requirements (MANDATORY):
- The 20-counter set MUST NOT include:
  - Periodic_IPC
  - Periodic_Instructions
  - Periodic_Cycles
  - Counters with RET_BLOCKED, INST_LOST or ST_BREAK (as they are effect counters). We are trying to determine the top 20 counters which can help us root cause the IPC regression.
  - Counters moving in a direction opposite to the IPC regression (e.g. if a cache queue occupancy is decreasing or MSHR fullness is decreasing).
- For two counters with the same or very similar semantics (e.g. MEM_REQ_BUFFER_FULL and MEM_REQ_BUFFER_FULL_DENIED_DEMAND) - both should not be included.
- Bin/Histogram rule: Never choose bucketized/bin counters (name contains DIST_, IN_WINDOW_, LATENCY<digits>, or ends with bucket numbers like _48_count, _14_count, _1_0_count) if a corresponding aggregate/parent counter exists. If any bin is chosen, the parent must also be chosen, and bins are limited to max 2 of the 20.
- MUST include any "important" counter that flips between configs (config1 == 0 XOR config2 == 0) and has max(config1, config2) >= 1000 (treat these as feature enable/disable signals).
- The 20 counters MUST be output in the SAME SEQUENCE as they appear in {{PMU_CSV_FILENAME}} (CSV order).
- Do NOT rank them (no numbering, no sorting by importance); keep CSV order.
- Output ONLY the counter names (one per line) in the required block format below.

Stop after Step 2.

---

## Output format (MANDATORY)
Produce ONLY the following copy-pasteable block:

Selected counters (Top 20, CSV order; 1 per line)
<20 lines, each line exactly one counter name>
