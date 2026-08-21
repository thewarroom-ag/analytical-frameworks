---
description: Run the full analytical pipeline (Cynefin → Disney → Pre-Mortem → Inversion → Synthesis)
---

Run the analytical-pipeline skill on the following idea. Execute all four
analytical frameworks in sequence: Cynefin classification, Disney Creative
Strategy, Pre-Mortem failure analysis, and Inversion gap analysis, concluding
with the Pipeline Synthesis.

This takes several minutes. Twelve agents run.

Print the compact output by default: one result line per stage, then the final
synthesis in full, then an offer to expand any part.

If $ARGUMENTS contains `--full` (or the user asks to see everything, or asks you
not to summarise), print every section in full instead. Strip the flag from the
input before analysing it.

Either way, run every agent. The flag changes what is printed, never what runs.

The idea to analyze:

$ARGUMENTS

<!-- Analytical Frameworks by The War Room. Authored by Shadow, CEO, The War Room.
     Source-available, attribution required. https://github.com/thewarroom-ag/analytical-frameworks -->
