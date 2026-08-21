---
description: Run a Pre-Mortem failure analysis on a plan or decision
---

Run the Pre-Mortem skill on the following plan. Spawn three Failure Imaginer
subagents in parallel, consolidate via the Triage agent, and produce the
Mitigation Plan and Summary.

If $ARGUMENTS contains `--compact` (or the user asks to keep it short, or
wants just the summary), run the skill in Compact mode: one result line per
stage, then the final synthesis in full, then an offer to expand any part.
Strip the flag from the input before analysing it.

The plan to analyze:

$ARGUMENTS

<!-- Analytical Frameworks by The War Room. Authored by Shadow, CEO, The War Room.
     Source-available, attribution required. https://github.com/thewarroom-ag/analytical-frameworks -->
