You will be given 5 independent CA reports for the SAME simpoint.

Your task is to aggregate them conservatively.

Important:
- Do NOT do new code analysis.
- Do NOT introduce a bug theme unless it is explicitly supported by the supplied reports.
- Keep DIRECT-CAUSE synthesis separate from ADDITIONAL GUARANTEED BUG synthesis.
- When a report contains a section for rejected non-bugs / non-guaranteed suspicions, do not count those items as supporting evidence.

For each report, first extract the following:
1) direct_cause_theme
- A SHORT normalized phrase for the report's main guaranteed issue causing the routed regression.
- If the report names multiple direct-cause issues, choose the most fundamental one.

2) additional_guaranteed_bug_themes
- Up to 3 SHORT normalized phrases, taken from guaranteed-bug content in the report.
- Prefer the report's explicit "Additional guaranteed bugs" section.
- BUT if a report's direct-cause section clearly presents a theme as a guaranteed bug in the analyzed modified component, that same theme may also support the cross-report additional-bug tally.
- Do NOT include rejected findings, speculative concerns, or code improvements.
- Do NOT duplicate the same normalized theme twice within one report.

3) best_improvement_lever
- A SHORT normalized phrase for the report's strongest code-level improvement lever.

Then aggregate across the 5 reports using these rules:
- DIRECT CAUSE: choose a winner if one normalized direct_cause_theme appears in at least 3 reports. Otherwise, choose the unique highest-occurring direct_cause_theme if it appears at least 2 times. If no theme appears at least 2 times, or if there is a tie for the highest-occurring theme, say there is no clear majority direct cause.
- ADDITIONAL GUARANTEED BUGS: retain each normalized additional_guaranteed_bug_theme that appears in at least 2 reports. For this tally, count support from either the report's direct-cause section or its additional-guaranteed-bugs section, as long as the report clearly presents that theme as a guaranteed bug in the modified component.
- IMPROVEMENT LEVER: choose a winner if one normalized improvement lever appears in at least 3 reports. Otherwise, choose the unique highest-occurring improvement lever if it appears at least 2 times. If no improvement lever appears at least 2 times, or if there is a tie for the highest-occurring improvement lever, say no clear majority.
- Normalize synonymous phrasings into one concise theme.
- If one theme is just a downstream restatement of another, prefer the more fundamental bug theme.
- If no direct-cause theme reaches the decision rule above, say there is no clear majority direct cause.
- Normalize active-path representation and bounded-state defects carefully:
  - Phrasings about the selected key/representation being too raw, too narrow, too aliased, insufficiently transformed, or otherwise inconsistent with its later lookup/update use are one active-path representation theme when they describe the same variable and mechanism.
  - Do not rename an active-path representation defect as "missing support for an alternative mode" merely because that alternative is proposed as the fix.
  - Keep "unrelated instances collide/alias together" separate from "related instances do not share enough history / mostly one-off keys" unless the reports explicitly prove they are the same mechanism.
- Normalize active-path provenance and recovery defects carefully:
  - Phrasings about persistent predictor/policy/recovery state being updated by wrong-provenance events are one theme only when they refer to the same mutation site, state variable, and later consumer.
  - Keep wrong-provenance training/table updates separate from wrong-provenance latch/flag/recovery-marker updates unless the reports prove they are the same mechanism.
  - Keep missing rollback/recovery of persistent state separate from the ungated mutation that creates the bad state unless the reports explicitly treat them as one inseparable bug.

Return output in EXACTLY this format:

1) Extracted Themes Per Report
- Report 1:
  - Direct cause: ...
  - Additional guaranteed bugs: [...]
  - Best improvement lever: ...
- Report 2:
  - Direct cause: ...
  - Additional guaranteed bugs: [...]
  - Best improvement lever: ...
- Report 3:
  - Direct cause: ...
  - Additional guaranteed bugs: [...]
  - Best improvement lever: ...
- Report 4:
  - Direct cause: ...
  - Additional guaranteed bugs: [...]
  - Best improvement lever: ...
- Report 5:
  - Direct cause: ...
  - Additional guaranteed bugs: [...]
  - Best improvement lever: ...

2) Direct-Cause Theme Tally
- <theme>: <count>
- <theme>: <count>
...

3) Additional Guaranteed Bug Theme Tally
- <theme>: <count>
- <theme>: <count>
...

4) Improvement Lever Tally
- <theme>: <count>
- <theme>: <count>
...

5) Final Direct Cause (Majority Vote)
- <winning theme> OR "No clear majority direct cause"

6) Aligned Direct-Cause Narrative
- 1 short paragraph using ONLY the winning direct-cause reports.
- Mention the core code-path / mechanism pattern only if it appears in those reports.

7) Consensus Additional Guaranteed Bugs
- For each retained additional-bug theme (count >= 2), provide:
  - Theme: ...
  - Support: <count>/5
  - Short summary: ...
- If a retained additional-bug theme lost the direct-cause vote but still appears as a guaranteed bug in at least 2 reports, keep it here rather than moving it to Minority / Rejected Notes.
- If none qualify, write: "None retained by consensus."

8) Final Improvement Lever
- <winning lever> OR "No clear majority improvement lever"

9) Minority / Rejected Notes
- Briefly note any recurring but non-retained themes that appeared but did not meet threshold.
- Do NOT place a theme here if it qualified for retention under section (7).

Now aggregate the following 5 reports:

REPORTS_BEGIN
{{REPORTS_BLOCK}}
REPORTS_END
