### Evidence
1) {{PMU_CSV_FILENAME}}
   - Contains config1 and config2 values for many counters.
   - Contains a "Counter Semantics" column with one-line descriptions when available.

### Task
Select exactly 20 counter rows from the CSV that should be passed to downstream
analysis of the Periodic IPC regression in config2 relative to config1.

Use the counter names, Counter Semantics descriptions, and config1/config2
values to decide which rows are most informative for downstream analysis. Do
not explain the regression.

### Output format
Output exactly 20 counter names, one per line, in the same order as they appear
in the CSV.
Do not include explanations, numbering, markdown, or any text other than the
20 counter names.
