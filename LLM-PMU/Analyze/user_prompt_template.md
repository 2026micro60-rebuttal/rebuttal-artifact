### Required evidence sources (MANDATORY)
1) {{PMU_CSV_FILENAME}}
   - Contains config1 and config2 values for many PMU counters.

### Definitions (MANDATORY)
- Periodic IPC is Periodic_Instructions / Periodic_Cycles.
- "Regression" means Periodic_IPC decreases because Periodic_Cycles increase at (approximately) constant Periodic_Instructions.

### Task
Explain the ~{{IPC_REGRESSION_PERCENT}} decrease in Periodic IPC in config2 vs config1.

You must identify the single best "direct cause" of the regression.

---

## Output format (MANDATORY)
1) Top-level IPC accounting (instructions, cycles, IPC) with computed deltas.
2) Direct cause selection:
   - the chosen direct cause,
3) Final single-sentence diagnosis:
   - "Config2 regresses because <direct cause> which causes <primary effects>, increasing cycles by <X%> at constant instructions."
