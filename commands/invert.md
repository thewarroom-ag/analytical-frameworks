---
description: Run Munger/Jacobi Inversion analysis (Forward Case vs. Failure Recipe vs. Delta)
---

Run the Inversion skill on the following idea. Spawn the Forward Case and
Inversion Case agents in parallel, then the Delta Analyst, and produce the
final Verdict.

Print the compact output by default: one result line per stage, then the final
synthesis in full, then an offer to expand any part.

If $ARGUMENTS contains `--full` (or the user asks to see everything, or asks you
not to summarise), print every section in full instead. Strip the flag from the
input before analysing it.

Either way, run every agent. The flag changes what is printed, never what runs.

The idea to invert:

$ARGUMENTS

<!-- Analytical Frameworks by The War Room. Authored by Shadow, CEO, The War Room.
     Source-available, attribution required. https://github.com/thewarroom-ag/analytical-frameworks -->
