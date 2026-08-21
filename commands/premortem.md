---
description: Run a Pre-Mortem failure analysis on a plan or decision
---

Run the Pre-Mortem skill on the following plan. Spawn three Failure Imaginer
subagents in parallel, consolidate via the Triage agent, and produce the
Mitigation Plan and Summary.

Print the compact output by default: one result line per stage, then the final
synthesis in full, then an offer to expand any part.

If $ARGUMENTS contains `--full` (or the user asks to see everything, or asks you
not to summarise), print every section in full instead. Strip the flag from the
input before analysing it.

Either way, run every agent. The flag changes what is printed, never what runs.

The plan to analyze:

$ARGUMENTS

<!-- Analytical Frameworks by The War Room. Authored by Shadow, CEO, The War Room.
     Source-available, attribution required. https://github.com/thewarroom-ag/analytical-frameworks -->
