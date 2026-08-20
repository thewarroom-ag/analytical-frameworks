---
name: inversion
description: |
  Run Munger/Jacobi Inversion analysis on any idea, strategy, plan, or decision.
  Spawns a Forward Case agent and Inversion agent in parallel (genuine isolation),
  then a Delta Analyst identifies the gap between optimism and pessimism, and the
  main agent produces the final verdict. Use whenever the user says 'inversion',
  'invert this', 'Munger analysis', 'what would guarantee failure', 'how could
  I be wrong', 'stupidity filter', 'what should I avoid', 'where would I die',
  'devil's advocate', or asks for an opposing perspective on a plan. Also trigger
  when the user asks to 'flip this around', 'argue the other side', or 'what am
  I missing'. This skill REQUIRES subagent support (Claude Code).
---

# Inversion: The Stupidity Filter

"All I want to know is where I'm going to die, so I'll never go there."
Attributed to Charlie Munger

Inversion is a mental model rooted in Carl Jacobi's mathematical principle
("man muss immer umkehren", invert, always invert) and popularized by Munger
as a decision-making framework. The core insight: it is often easier to avoid
stupidity than to pursue brilliance. By identifying what would guarantee failure
and then deliberately avoiding those behaviors, you increase your odds of
success more reliably than chasing the optimal path.

This skill operationalizes Inversion through genuine agent isolation. The Forward
Case and Inversion Case agents work in parallel with no knowledge of each other's
output. A Delta Analyst then identifies what falls in the gap between them.

## Required subagents

- `inversion-forward.md`: Builds the strongest case for success
- `inversion-inverse.md`: Builds the systematic failure recipe
- `inversion-delta.md`: Analyzes the gap between forward and inverse

## Orchestration sequence

### Step 1: Capture the idea

Take the user's idea, strategy, plan, or decision. Frame it clearly as a
proposition that can be argued for and against.

### Step 2: Spawn Forward and Inverse agents in parallel

These MUST run simultaneously. The isolation is the mechanism. Each agent
receives only the original idea and no knowledge of the other's existence.

**Forward Case prompt:**
```
Build the strongest case for why this will succeed:

THE IDEA/PLAN:
[user's idea]

CONTEXT:
[any additional context]

Deliver your Forward Case.
```

**Inversion Case prompt:**
```
Apply Munger/Jacobi inversion to this idea. What would guarantee its failure?

THE IDEA/PLAN:
[user's idea]

CONTEXT:
[any additional context]

Deliver your Inversion Case.
```

### Step 3: Spawn the Delta Analyst

After both complete, invoke `inversion-delta` with both outputs:

```
Analyze the gap between these two independent assessments of the same idea:

THE ORIGINAL IDEA:
[user's idea]

THE FORWARD CASE:
[full Forward Case output]

THE INVERSION CASE:
[full Inversion Case output]

Produce your Delta Analysis.
```

### Step 4: Produce the Verdict (main agent)

Present the complete session:

```
## 📈 The Forward Case
[Forward agent output]

## 📉 The Inversion Case
[Inversion agent output]

## ⚖️ Delta Analysis
[Delta agent output]

## 🧭 The Verdict
[Your synthesis - see requirements below]
```

## Verdict requirements

The Verdict is your synthesis. It must include:

**Net assessment:** Given both cases and the delta analysis, what is the honest
probability-weighted outlook for this idea? Not a number. A qualitative
assessment: strong conviction, conditional conviction, proceed with caution,
reconsider fundamentally.

**The Stupidity Avoidance List:** Pull from the Delta Analyst's list and
prioritize. What are the 4-5 things the user must avoid at all costs? Frame
each as a specific behavior, not an abstract risk. "Don't hire a CTO before
validating demand" not "execution risk."

**The Strength-Vulnerability Paradox:** Identify where the plan's greatest
strength is also its greatest vulnerability. One sentence.

**The One Question:** What single question, if answered, would most change the
net assessment? This is the user's homework.

## Calibration

- **The parallel execution is non-negotiable.** If the Forward Case agent can
  see the Inversion agent's framing, the analysis collapses. Both agents must
  work from the raw idea only.
- **Depth scales with input.** A quick "should I do X?" gets a tighter analysis.
  A detailed strategy document gets thorough treatment.
- **The Delta is where the value lives.** The Forward and Inversion Cases are
  inputs to the Delta. If the Delta Analysis is thin, the whole exercise
  underperforms.
- **Munger's biases matter.** The Inversion agent should identify which cognitive
  biases (incentive-caused bias, commitment escalation, social proof) are most
  likely to push the team toward the failure paths.

## Accepting Disney or Pre-Mortem output

If the user says "run inversion on the Disney output" or similar, use the
Disney Synthesis as input. If chained after a Pre-Mortem, include the Pre-Mortem
Summary as additional context for both agents.

## Attribution

Part of **Analytical Frameworks** by The War Room.
Authored by Shadow, CEO, The War Room.
Source-available, attribution required. See LICENSE.
https://github.com/thewarroom-ag/analytical-frameworks
