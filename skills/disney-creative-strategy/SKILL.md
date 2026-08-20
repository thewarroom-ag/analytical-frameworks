---
name: disney-creative-strategy
description: |
  Run Walt Disney's Creative Strategy (Outsider, Dreamer, Realist, Critic) as a
  multi-agent debate on any idea, concept, product, business plan, or problem.
  Each persona runs as an independent subagent with its own context window, then
  the main agent synthesizes all outputs. Use whenever the user says 'run Disney
  strategy', 'dreamer realist critic', 'Disney method', 'creative strategy',
  'four perspectives', 'stress test this idea', 'run the rooms', or presents an
  idea and asks for multi-perspective analysis. Also trigger when the user asks to
  'workshop this', 'pressure test', 'evaluate from all angles', 'give me
  dreamer/realist/critic on this', 'outsider perspective', or 'run the Disney
  rooms'. This skill REQUIRES subagent support (Claude Code). Each persona operates
  in genuine isolation, producing independent analysis that is then synthesized.
---

# Disney Creative Strategy: Multi-Agent Session

Walt Disney's Creative Strategy, modeled by Robert Dilts in 1994, uses four
sequential thinking styles: Outsider, Dreamer, Realist, and Critic. This skill
runs each as an independent subagent, then synthesizes their outputs.

The isolation matters. When the Critic doesn't know what the Dreamer said until
explicitly shown, you get genuine independent analysis instead of one mind trying
to disagree with itself.

## Required subagents

This skill depends on four subagents in `.claude/agents/` or `~/.claude/agents/`:
- `disney-outsider.md`: Detached analytical observer (Room 0)
- `disney-dreamer.md`: Expansive visionary (Room 1)
- `disney-realist.md`: Pragmatic builder (Room 2)
- `disney-critic.md`: Constructive stress-tester (Room 3)

If any subagent is missing, inform the user and provide the installation path.

## Orchestration sequence

The sequence is fixed. Always run in this order:

### Step 1: Capture the idea

Take the user's idea, concept, or problem statement. If it's vague, ask one
clarifying question before proceeding. Do not ask more than one.

### Step 2: Spawn the Outsider (Room 0)

Invoke the `disney-outsider` subagent with this prompt structure:

```
Analyze this idea as The Outsider in Disney's Creative Strategy:

IDEA: [user's idea/concept]

CONTEXT PROVIDED BY USER: [any additional context the user gave]

Deliver your Room 0 analysis.
```

Collect the Outsider's full output before proceeding.

### Step 3: Spawn the Dreamer (Room 1)

Invoke the `disney-dreamer` subagent. Pass the original idea AND the Outsider's
complete output:

```
Generate your vision as The Dreamer in Disney's Creative Strategy:

IDEA: [user's idea/concept]

THE OUTSIDER'S ANALYSIS (Room 0):
[paste full Outsider output]

Deliver your Room 1 vision.
```

Collect the Dreamer's full output before proceeding.

### Step 4: Spawn the Realist (Room 2)

Invoke the `disney-realist` subagent. Pass the original idea, Outsider output,
AND Dreamer output:

```
Build your plan as The Realist in Disney's Creative Strategy:

IDEA: [user's idea/concept]

THE OUTSIDER'S ANALYSIS (Room 0):
[paste full Outsider output]

THE DREAMER'S VISION (Room 1):
[paste full Dreamer output]

Deliver your Room 2 plan.
```

Collect the Realist's full output before proceeding.

### Step 5: Spawn the Critic (Room 3)

Invoke the `disney-critic` subagent. Pass everything:

```
Deliver your critique as The Critic in Disney's Creative Strategy:

IDEA: [user's idea/concept]

THE OUTSIDER'S ANALYSIS (Room 0):
[paste full Outsider output]

THE DREAMER'S VISION (Room 1):
[paste full Dreamer output]

THE REALIST'S PLAN (Room 2):
[paste full Realist output]

Deliver your Room 3 critique.
```

Collect the Critic's full output.

### Step 6: Synthesize (The Fifth Wall)

You (the main agent) now have all four room outputs. Produce the synthesis
yourself. Do NOT delegate synthesis to a subagent. You have the full picture.

Present the complete session to the user:

```
## 🔍 Room 0: The Outsider
[Outsider's output]

## 🟢 Room 1: The Dreamer
[Dreamer's output]

## 🔵 Room 2: The Realist
[Realist's output]

## 🔴 Room 3: The Critic
[Critic's output]

## ⚖️ The Fifth Wall: Synthesis
[Your synthesis]
```

## Synthesis requirements

The synthesis must include:

**Points of convergence:** Where all four perspectives align. These are your
highest-confidence moves.

**Productive tensions:** Where the rooms disagree but the disagreement is
informative. Frame these as decisions to be made, not problems.

**Recommended next move:** One concrete action that honors the dream, passes
the realist's feasibility test, addresses the critic's top concern, and fits
within the outsider's landscape assessment. Not a vague "do more research."
A specific next step.

**Open questions:** Two to four questions that remain unanswered and would most
change the direction if answered. These become the user's homework.

The synthesis should be the shortest section. The work happened in the rooms.
Synthesis distills, it doesn't rehash.

## Calibration

- **Depth scales with input.** A one-sentence idea means each subagent gets a
  shorter prompt and produces tighter output. A detailed business plan means
  richer context passed to each subagent.
- **The Outsider sets the playing field.** If the Outsider identifies a market
  reality that subsequent rooms ignore, flag that gap in synthesis.
- **No artificial balance.** If the idea is strong, don't manufacture weaknesses.
  If it has a fatal flaw, don't hide it in synthesis.

## Multiple rounds

If the user asks to "run another round" or "go deeper," run the full sequence
again. This time, include the previous synthesis in the context passed to
each subagent. The Outsider re-examines the landscape. The Dreamer expands on
convergence points. The Realist refines based on critic input. The Critic
evaluates the updated version.

## Parallel execution option

If the user wants speed over sequential context-building, you can spawn the
Outsider and Dreamer in parallel (since the Dreamer's independence is actually
a feature when genuinely isolated). Then feed both outputs to the Realist,
then the Critic. This trades some conversational threading for speed. Only
do this if the user explicitly asks for it or the idea is simple enough that
the Outsider's framing won't materially change what the Dreamer produces.

## Attribution

Part of **Analytical Frameworks** by The War Room.
Authored by Shadow, CEO, The War Room.
Source-available, attribution required. See LICENSE.
https://github.com/thewarroom-ag/analytical-frameworks
