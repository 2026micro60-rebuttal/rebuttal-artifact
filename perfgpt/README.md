# PerfGPT Prompt Templates

Reusable prompt templates for the PerfGPT staged workflow.

## Stage Mapping

- `PS/`: PMU Selection.
- `CEA/`: Cause-Effect Analysis.
- `Majority Vote/`: CEA majority vote.
- `CA/component_routing_*`: structural v/s code-analysis routing.
- `CA/code_analysis_*`: source-code analysis.
- `CA Majority Vote/`: code-analysis majority vote.

## Placeholder Convention

Run-specific inputs are written as `{{PLACEHOLDER_NAME}}`. Examples include:

- `{{PMU_CSV_FILENAME}}`
- `{{IPC_REGRESSION_PERCENT}}`
- `{{SELECTED_COUNTERS_TOP20_CSV_ORDER}}`
- `{{CHANGED_COMPONENTS_CONTEXT}}`
- `{{CEA_MAJORITY_VOTE_RESPONSE}}`
- `{{ROOT_CAUSE_BEHAVIOR}}`
- `{{ALIGNED_CAUSAL_NARRATIVE}}`
- `{{MAPPED_CONFIGURATION_CONTEXT}}`
- `{{MAPPED_SOURCE_CODE_MANIFEST}}`
- `{{FUNCTION_DESCRIPTIONS_OR_NAVIGATION_GUIDES}}`
- `{{ADDITIONAL_MAPPED_CONTEXT}}`
- `{{REPORTS_BLOCK}}`
