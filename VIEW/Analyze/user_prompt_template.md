### Required evidence sources
1) {{PMU_CSV_FILENAME}}
   - Contains config1 and config2 values for many PMU counters.
   - Contains a "Counter Semantics" column with one-line descriptions when available.
2) {{SELECTED_COUNTERS_FILENAME}}
   - Contains the 20 counters selected by a prior data-filtering stage.
3) {{SIMULATOR_SOURCE_DIR}}
   - Contains simulator source code.
4) {{PARAM_DEFAULTS_FILENAME}}
   - Contains config parameters applicable to both config1 and config2.
5) {{CONFIG_BLOCK_FILENAME}}
   - Contains runtime override config parameters for config1 and config2.

{{MAPPED_CONFIGURATION_CONTEXT}}

### Selected data subset
A prior data-filtering stage selected these 20 counters as the subset most
relevant to downstream analysis of the regression:

{{SELECTED_COUNTERS_TOP20_CSV_ORDER}}

Use these selected counters as the primary performance-data focus. You may
inspect the full CSV, parameter defaults, configuration block, and simulator source code to
validate or refine the diagnosis.

### Definitions
- Periodic IPC is Periodic_Instructions / Periodic_Cycles.
- "Regression" means Periodic IPC decreases because Periodic_Cycles increase at
  approximately constant Periodic_Instructions.

### Task
Explain the ~{{IPC_REGRESSION_PERCENT}} decrease in Periodic IPC in config2 vs config1.

Identify the single best direct cause of the regression and propose fixes as
follows:
1) Prioritize source-code bug fixes, because they are zero-cost in storage.
2) If there is no source-code or configuration bug, propose structural fixes
   subject to these constraints:
   - The following microarchitectural units are immutable:
     - number of reservation stations
     - number of functional units
     - mapping between instruction types and reservation stations
     - mapping between instruction types and functional units
     - mapping between reservation stations and functional units
     - latency and throughput of individual units or instruction types
   - You can only suggest size modifications to existing structures, such as
     caches, predictors, buffers, queues, registers, load-store queue, reorder
     buffer, branch target buffer, or widths.
   - You cannot add new components.
   - You cannot add new features within existing components.

### Required output
Provide:
1) Top-level IPC accounting: Periodic_Instructions, Periodic_Cycles, and
   computed Periodic IPC for both configs, including deltas.
2) Summary of the selected counters and any additional full-CSV counters that
   materially affect the diagnosis.
3) Source/config evidence inspected, including relevant files or functions from
   simulator source.
4) Direct-cause diagnosis.
5) Explanation of how the config2 change leads to the direct cause, or why that
   link is inconclusive.
6) Proposed fix and why it should address the regression.
7) Final single-sentence diagnosis.
