# Prompt Template Package

LLM prompt templates for performance-diagnosis workflows.

## Folders

- `perfgpt/`: PerfGPT staged workflow templates.
- `LLM-PMU/`: PMU-only baseline templates, including analysis, majority vote, and structural fix.
- `LLM-PMU-Code/`: one-step PMU with full source-code baseline templates, plus majority vote.
- `VIEW/`: two-stage VIEW templates with `Filter` and `Analyze`, plus majority vote.

All run-specific inputs are represented with `{{PLACEHOLDER_NAME}}` placeholders.
