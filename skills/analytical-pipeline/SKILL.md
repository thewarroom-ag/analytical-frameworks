---
name: analytical-pipeline
description: |
  Run the full analytical pipeline: Cynefin → Disney → Pre-Mortem → Inversion
  on any idea, plan, or decision. Chains all four analytical frameworks in
  sequence, with each stage feeding context to the next. Use whenever the user
  says 'run the pipeline', 'full analysis', 'run all frameworks', 'run
  everything on this', 'deep analysis', 'full pipeline', or wants comprehensive
  multi-framework analysis of an idea. Also trigger when the user uses the
  /pipeline command. This skill REQUIRES subagent support (Claude Code) and
  depends on all four analytical skills being installed: cynefin,
  disney-creative-strategy, pre-mortem, and inversion.
---

# Analytical Pipeline: Full-Stack Decision Analysis

Chains four analytical frameworks in sequence. Each stage's output feeds the
next, building a comprehensive analysis from sense-making through vision,
stress-testing, and strategic inversion.

## The sequence

```
Cynefin (what kind of problem is this?)
    ↓ domain classification + appropriate response strategy
Disney (what's the vision, plan, and critique?)
    ↓ four-room creative strategy output
Pre-Mortem (how could the plan fail?)
    ↓ parallel failure imagination + mitigation plan
Inversion (what would guarantee failure, and what's the gap?)
    ↓ forward case vs. failure recipe + stupidity avoidance list
Pipeline Synthesis (what do all four frameworks tell us together?)
```

## Required skills

All four must be installed:
- `cynefin`: Domain classification
- `disney-creative-strategy`: Four-room creative strategy
- `pre-mortem`: Prospective failure analysis
- `inversion`: Forward/inverse gap analysis

## Orchestration

The "present the output" steps below describe FULL mode. In the default compact
mode, replace each one with the single-line stage summary from Output length.
The agents you spawn and the context you pass between stages are identical in
both modes.

### Stage 1: Cynefin

Run the full Cynefin skill (Classifier → Challenger → Resolution → Responder).

**What carries forward:** The resolved domain classification and the Responder's
strategy. This frames how Disney should approach the idea. If the domain is
Complex, the Disney Realist should emphasize safe-to-fail experiments over
detailed timelines. If Complicated, the Realist can be more prescriptive.

After Cynefin completes, present the output and add a brief transition:

"Cynefin classified this as [domain]. Now running Disney Creative Strategy
with that framing."

### Stage 2: Disney Creative Strategy

Run the full Disney skill (Outsider → Dreamer → Realist → Critic → Synthesis).

**Context injection from Cynefin:** Include the Cynefin domain classification
and Responder strategy as additional context for the Outsider agent. The
Outsider should reference the domain classification in its landscape analysis.
The Realist should calibrate its plan to the domain (experiments for Complex,
analytical plans for Complicated, best practices for Clear, immediate action
for Chaotic).

**What carries forward:** The Realist's plan and the Synthesis. These become
the input for Pre-Mortem.

After Disney completes, present the output and transition:

"Disney produced a plan and synthesis. Now running Pre-Mortem to stress-test it."

### Stage 3: Pre-Mortem

Run the full Pre-Mortem skill (3× Imaginer parallel → Triage → Mitigation).

**Context injection from Disney:** Use the Realist's plan as THE PLAN. Include
the Critic's concerns and the Synthesis as additional context. Instruct the
Imaginers to go beyond what the Disney Critic already identified.

**What carries forward:** The Triage Analysis (ranked risks) and the Mitigation
Plan. These inform what the Inversion agents should focus on.

After Pre-Mortem completes, present the output and transition:

"Pre-Mortem identified [N] failure scenarios, [N] Critical. Now running
Inversion on the core thesis."

### Stage 4: Inversion

Run the full Inversion skill (Forward + Inverse parallel → Delta → Verdict).

**Context injection:** The Forward Case agent receives the original idea plus
the Disney Dreamer's vision (to build the strongest bull case). The Inverse
agent receives only the original idea (to maintain genuine isolation). The
Delta Analyst receives everything including Pre-Mortem's top risks.

**What carries forward:** Everything. The Verdict is the final analytical output
before Pipeline Synthesis.

### Stage 5: Pipeline Synthesis

After all four stages complete, produce a Pipeline Synthesis that integrates
everything. This is the capstone output.

## Pipeline Synthesis format

```
## 🔬 Pipeline Synthesis

### Domain Reality
[One paragraph: What Cynefin revealed about the nature of this problem.
How should that classification shape every subsequent decision?]

### The Integrated Picture
[Two paragraphs: What emerges when you layer Disney's vision, Pre-Mortem's
failure scenarios, and Inversion's gap analysis? What do all four frameworks
agree on? Where do they create productive tension?]

### The Kill List
[The 4-6 items that appeared across multiple frameworks as critical.
If Pre-Mortem flagged it AND Inversion's failure recipe included it AND
Disney's Critic raised it, that's a three-framework convergence signal.
List each with its framework sources.]

### The Conviction Stack
[Rank the elements of the plan by conviction level:
- HIGH CONVICTION: Supported across all frameworks, no significant challenge
- CONDITIONAL: Supported but dependent on specific assumptions holding
- LOW CONVICTION: Significant framework disagreement or unresolved questions]

### The Decision
[One paragraph: Given everything, what should the user do? Be direct.
Not "consider the options." A specific recommendation with the one or two
conditions that would change it.]

### Homework
[The 4-5 specific questions that remain unanswered and would most change
the analysis if answered. Sourced from across all four frameworks. Each
framed as an actionable investigation, not an abstract musing.]
```

## Calibration

- **This takes a few minutes.** Say so up front. Twelve agents run either way.
  What changes is how much of their output reaches the screen. See Output
  length: compact is the default.
- **Transitions belong to full mode only.** In full mode, bridge between stages
  with one or two sentences. In the default compact mode, the one-line stage
  summary IS the transition. Do not write both.
- **The Pipeline Synthesis is the payoff.** Each individual framework's output
  is valuable on its own, but the synthesis that identifies cross-framework
  convergence signals is what justifies running the full pipeline. Invest
  effort here.
- **Don't rehash.** The Pipeline Synthesis should not repeat what each framework
  found. It should identify what ONLY becomes visible when you layer all four
  together.
- **The Kill List is the most actionable output.** Items that appear across
  multiple frameworks independently are the highest-confidence risks. Three
  separate analytical systems converging on the same concern is a signal that
  demands attention.

## Output length

**Compact is the default.** Print the summary unless the user asks for
everything.

The full pipeline produces five stages of analysis. Printing all of it buries
the answer. The analysis is worth running. It is not always worth reading in
full on the first pass.

**This governs what you print. It never governs what you run.** All twelve agents
run every time. The isolation between them is unchanged. The convergence counts
still come from writers who could not see each other. You are choosing when the
user reads the detail, not whether it exists.

### Default: compact

Print one line per stage, as each finishes. State the RESULT, not the process.
No transitions, no narrative bridges, no per-agent output.

```
Cynefin      Complex. Challenger argued Complicated. Classification held.
Disney       Plan built. Critic raised 4 concerns, 2 unresolved.
Pre-Mortem   15 failure scenarios, 6 Critical, 3 convergence signals.
Inversion    Forward and inverse cases diverge on 2 of 5 core assumptions.
```

Rules for a stage line:

- One line. Under 100 characters.
- Give the finding and the counts. A number beats an adjective.
- Never describe what the agents did. The reader knows.

Tell each framework you invoke that compact is on, so it suppresses its own
per-agent output too. A compact pipeline that calls a verbose Disney is not
compact.

### Then the Pipeline Synthesis, in full

Never compress the synthesis. It is the payoff and it is already short. Every
section stays: Domain Reality, The Integrated Picture, The Kill List, The
Conviction Stack, The Decision, Homework.

### Then offer the detail

End with one line:

```
Full output for every stage is in context. Say which one to expand.
```

The agents have already run. Expanding a stage costs nothing but the printing.
If the user asks, print that stage in full.

### Full mode

Print every stage in full when the user asks for it:

- `/pipeline --full <idea>`
- "Show me everything", "full output", "don't summarise".

In full mode, print each stage's complete output as it finishes, with a one or
two sentence bridge between stages saying what was learned and what runs next.

Also use full mode when the user cannot ask a follow-up. If the output is going
into a document, an email, or a report they will send on, they get one pass at
it. Print it long.

## Partial pipeline

If the user says "run the pipeline but skip Cynefin" or similar, honor that.
Drop the skipped stage and adjust context injection accordingly. The pipeline
is modular. Any subset of the four frameworks can run in sequence.

## Time and token management

The full pipeline spawns approximately 12 subagents across all four stages.
On complex inputs, this can consume significant tokens. If the user's input
is brief (one sentence), calibrate each stage for tighter output. If the
input is a detailed plan, let each stage go deeper.

If any stage produces a clear signal that changes the subsequent analysis
significantly (e.g., Cynefin classifies as Chaotic, meaning detailed planning
is premature), note this in the transition and ask the user whether to
continue the remaining stages or stop and act first.

## Language

Write for a reader whose first language is not English. ASD-STE100 Simplified
Technical English is the baseline.

- Short sentences. One idea each.
- Active voice. Name the actor.
- The plainest word that carries the meaning. No jargon unless it is precise and
  necessary. No buzzwords.
- Cut every word that is not working: hedges, throat-clearing, restating the
  question.
- No vague pronouns. If a claim needs a number or a source, give it.
- Write like a sharp person talking to the reader, not a press release.
- Never pad to sound thorough.

Where a persona voice above sets the format (prose or lists), the persona wins.
On word choice and sentence length, this section wins.

## Attribution

Part of **Analytical Frameworks** by The War Room.
Authored by Shadow, CEO, The War Room.
Source-available, attribution required. See LICENSE.
https://github.com/thewarroom-ag/analytical-frameworks
