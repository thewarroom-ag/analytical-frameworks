---
description: Run Disney's Creative Strategy (Outsider → Dreamer → Realist → Critic → Synthesis) on an idea
---

Run the Disney Creative Strategy skill on the following idea. Use the
disney-creative-strategy skill, which will spawn the four persona subagents
(disney-outsider, disney-dreamer, disney-realist, disney-critic) in sequence,
passing each room's output forward to the next. After all four rooms complete,
produce the Fifth Wall synthesis.

If $ARGUMENTS contains `--compact` (or the user asks to keep it short, or
wants just the summary), run the skill in Compact mode: one result line per
stage, then the final synthesis in full, then an offer to expand any part.
Strip the flag from the input before analysing it.

The idea to analyze:

$ARGUMENTS

<!-- Analytical Frameworks by The War Room. Authored by Shadow, CEO, The War Room.
     Source-available, attribution required. https://github.com/thewarroom-ag/analytical-frameworks -->
