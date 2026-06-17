### Task
You are given {{EXPECTED_REPLICAS}} independent analysis outputs for the same benchmark simpoint and same config1/config2 comparison.

### Analysis files
{{ANALYSIS_FILES}}

### Question
Why does config2 suffer a Periodic IPC regression relative to config1?

### Voting procedure
1) Read every analysis file.
2) Extract from each replica:
   - direct-cause diagnosis
   - config2-to-cause causal link
   - key counter/stat evidence
   - source/config evidence
   - proposed fix
   - whether the proposed fix is a source-code bug fix, config/policy change, or size-only structural fallback
   - final single-sentence diagnosis
3) Group replicas that make the same substantive claim, even if wording differs. Do this separately for:
   - direct-cause diagnosis
   - config2-to-cause causal link
   - key counter/stat evidence
   - source/config evidence
   - proposed fix
4) For proposed fixes, vote semantically on the concrete action, not on generic wording. Examples:
   - "fix the component update path" and "use the same state predicate at update and lookup time" are the same fix group if they target the same code path.
   - "increase buffer entries", "increase queue capacity", and "larger component structure" are the same structural-fallback group.
   - "disable policy feedback" and "change the feedback rule to avoid downshifting useful requests" are related but should be separate groups unless the replicas clearly propose the same mechanism.
5) Select the winning group using this rule: if a group appears in at least 3 reports, it wins. Otherwise, choose the unique highest-occurring group if it appears at least 2 times. If no group appears at least 2 times, or if there is a tie for the highest-occurring group, mark that vote as inconclusive.
6) Do not reward a claim merely for being more verbose. Prefer the claim repeated by more replicas. Use source/counter evidence only to identify malformed outputs; do not use evidence strength to override the vote-count rule or resolve tied vote counts.
7) If the direct cause has a majority but the fix does not, say so. Do not invent a majority fix.
8) If a replica proposes both a primary zero-storage source/config fix and a structural fallback, count both groups, but identify which one that replica labels as primary.

### Required output
Provide:
1) Per-replica extraction table with columns:
   - replica
   - direct-cause group
   - config2-to-cause group
   - key evidence group
   - source/config evidence group
   - primary proposed-fix group
   - fallback proposed-fix group, if any
2) Direct-cause semantic vote:
   - all groups and counts
   - winning group and whether it is a winner or inconclusive
   - majority-vote direct cause
3) Config2-to-cause semantic vote:
   - all groups and counts
   - winning group and whether it is a winner or inconclusive
   - majority-vote causal explanation
4) Evidence semantic vote:
   - counter/stat evidence groups and counts
   - source/config evidence groups and counts
   - evidence conflicts or malformed evidence, if any
5) Proposed-fix semantic vote:
   - primary fix groups and counts
   - fallback fix groups and counts
   - winning primary fix if a winner exists
   - whether there is no majority-vote fix
   - brief explanation of how the winning fix addresses the voted direct cause
6) Important dissents:
   - dissenting direct causes
   - dissenting config2 links
   - dissenting fixes
7) Final majority-vote answer:
   - one paragraph stating the voted direct cause, voted config2 link confidence, and voted proposed fix status
   - one final single-sentence majority-vote diagnosis
