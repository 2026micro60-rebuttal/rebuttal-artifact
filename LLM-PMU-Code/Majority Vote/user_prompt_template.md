### Majority-vote aggregation task (MANDATORY)

You are given N (typically 5) completed analysis reports produced from the same experiment as ATTACHED TEXT FILES (e.g., response_1.txt ... response_5.txt).
Each report follows the single-run template and should begin with a one-line JSON header.

You must produce one consensus output using majority vote.
You must NOT do any new CSV or source-code analysis; only synthesize what is already written in the reports.

---

## (1) Voting rules (MANDATORY)

1) Bucket and normalize the cause_theme phrases into comparable buckets.
2) Majority vote:
   - If a bucket appears in >= 3 reports -> winner
   - Otherwise, choose the unique highest-occurring bucket if it appears at least 2 times
   - If no bucket appears at least 2 times, or if there is a tie for the highest-occurring bucket -> winner = UNCLEAR
3) For best_improvement_lever:
   - Vote using the same rule after normalization
   - If no winner -> UNCLEAR

---

## (2) Output format (MANDATORY)

### (0) Final Direct Cause (Majority Vote)
- <winning_cause_bucket or UNCLEAR>

### (1) Aligned causal narrative (majority-only)
Write 6-12 sentences explaining the cause of the regression.
- Only include claims that appear in the winning bucket reports.
- Do not introduce any new counters, functions, or params.

### (2) Best improvement lever (Majority Vote)
- <winning_improvement_bucket or UNCLEAR>

### (3) Improvement shortlist (majority-only)
List improvements that appear in >= 3 reports.

---

## (3) Guardrails (MANDATORY)
- Do NOT add new technical content.
- If the reports disagree on component_under_test or root_cause_behavior, state the disagreement and set winning_cause_bucket = UNCLEAR.
