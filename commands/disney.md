---
description: Run Disney's Creative Strategy (Outsider → Dreamer → Realist → Critic → Synthesis) on an idea
---

Run the Disney Creative Strategy skill on the following idea. Use the
disney-creative-strategy skill, which will spawn the four persona subagents
(disney-outsider, disney-dreamer, disney-realist, disney-critic) in sequence,
passing each room's output forward to the next. After all four rooms complete,
produce the Fifth Wall synthesis.

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
