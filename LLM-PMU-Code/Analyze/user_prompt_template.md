### Required evidence sources (MANDATORY)
1) {{PMU_CSV_FILENAME}}
   - Contains config1 and config2 values for many PMU counters.
2) {{SIMULATOR_SOURCE_ARCHIVE}}
   - Contains simulator source code.
3) {{PARAM_DEFAULTS_FILENAME}}
   - Contains config parameters applicable to both config1 and config2.
4) Configuration block (below)
   - Runtime override config parameters for config1 and config2.

## Configuration block
<<<CONFIGURATION_BEGIN>>>
{{MAPPED_CONFIGURATION_CONTEXT}}
<<<CONFIGURATION_END>>>

### Definitions (MANDATORY)
- Periodic IPC is Periodic_Instructions / Periodic_Cycles.
- "Regression" means Periodic_IPC decreases because Periodic_Cycles increase at (approximately) constant Periodic_Instructions.

### Task
Explain the ~{{IPC_REGRESSION_PERCENT}} decrease in Periodic IPC in config2 vs config1.

You must identify the single best "direct cause" of the regression and propose fixes as follows:
1) Prioritize source code bug fixes (as they are 0-cost on storage).
2) If there is no bug in the source code, propose structural fixes subject to the following constraints:
- The following microarchitectural units are immutable and you cannot suggest modifications in them:
  - number of reservation stations
  - number of functional units
  - mapping between instruction types and reservation stations
  - mapping between instruction types and functional units
  - mapping between reservation stations and functional units
  - latency and throughput of individual units (e.g. functional units) or of instruction types
- You can only suggest the following kinds of changes:
  - size modification (increase / decrease in structure sizes - e.g. caches, predictors, buffers, queues, registers, load-store queue, reorder buffer, branch target buffer, width etc.)
- You can only suggest microarchitectural modifications in existing components that are not new features but will help resolve the observed performance regression.
  - New components (e.g. a new prefetcher or a new branch predictor) cannot be added.
  - New features within existing components (e.g. register renaming MOV elimination, reduced broadcast on wakeup/select in the scheduler) cannot be added.
