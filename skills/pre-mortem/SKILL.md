---
name: pre-mortem
description: |
  Run Gary Klein's Pre-Mortem analysis on any plan, project, or decision. Spawns
  multiple independent Failure Imaginer agents in parallel to generate diverse
  failure scenarios, then consolidates via a Triage agent, and the main agent
  produces the mitigation plan. Use whenever the user says 'pre-mortem', 'premortem',
  'what could go wrong', 'stress test this plan', 'failure analysis', 'risk
  assessment', 'what would kill this', 'how could this fail', or asks to identify
  risks in a plan or project. Also trigger after a Disney Creative Strategy session
  when the user wants to stress-test the output. This skill REQUIRES subagent
  support (Claude Code).
---

# Pre-Mortem: Prospective Failure Analysis

Gary Klein's Pre-Mortem (2007, Harvard Business Review) is a risk assessment
method based on "prospective hindsight." Research at Wharton, University of
Colorado, and Cornell found that imagining an event has already occurred
increases the ability to accurately identify reasons for future outcomes by 30%.

The psychological mechanism: instead of asking "what might go wrong?" (which
triggers defensive optimism), the pre-mortem states that the plan HAS failed and
asks "what DID go wrong?" This reframes the exercise from speculation to
reconstruction, legitimizing dissent and surfacing risks that politeness or
groupthink would normally suppress.

## Required subagents

- `premortem-imaginer.md`: Failure scenario generator (spawned 3x in parallel)
- `premortem-triage.md`: Consolidates and ranks failure scenarios

## Orchestration sequence

### Step 1: Capture the plan

Take the user's plan, project, or decision. If it comes from a previous Disney
Creative Strategy session, use the Realist's plan and Synthesis as the input.
If the input is vague, ask one clarifying question.

### Step 2: Spawn three Failure Imaginers in parallel

Spawn three instances of `premortem-imaginer` simultaneously. Each receives the
same plan but works in complete isolation. The parallel execution is the point:
independent generation produces diverse failure scenarios, just as Klein's
original method has team members write independently before sharing.

Prompt each with:

```
You are Failure Imaginer [A/B/C].

The following plan has been implemented. It has failed completely.
Work backward and explain what went wrong.

THE PLAN:
[user's plan/project/decision]

CONTEXT:
[any additional context]

Generate your failure scenarios now.
```

Use different instance identifiers (A, B, C) for each.

### Step 3: Spawn the Triage Analyst

After all three Imaginers complete, invoke `premortem-triage` with all outputs:

```
Consolidate and rank the following failure scenarios from three independent
Failure Imaginers who analyzed the same plan without seeing each other's work.

THE ORIGINAL PLAN:
[user's plan]

IMAGINER A OUTPUT:
[full output]

IMAGINER B OUTPUT:
[full output]

IMAGINER C OUTPUT:
[full output]

Produce your triage analysis.
```

### Step 4: Produce the Mitigation Plan (main agent)

You (the main agent) now have the Triage Analysis. Produce the final output:

Present the complete session:

```
## 💀 Failure Scenarios
[Brief summary of total unique failures identified across all Imaginers]

## 🔺 Triage Analysis
[Triage agent's consolidated, ranked output]

## 🛡️ Mitigation Plan
[Your mitigation plan - see requirements below]

## 📋 Pre-Mortem Summary
[Decision brief - see requirements below]
```

## Mitigation Plan requirements

For each Critical and High severity failure from the Triage:

**Prevention action:** A specific, concrete step that reduces the likelihood of
this failure. Not "monitor the situation." Something someone can put on a
calendar and execute.

**Detection trigger:** An early warning signal that this failure mode is
beginning to materialize. Something measurable or observable.

**Contingency:** If prevention fails and the trigger fires, what is the
immediate response? Who owns it?

For Medium and Low severity failures, provide brief notes on whether to accept,
mitigate, or monitor. Do not produce full mitigation plans for these unless the
user asks.

## Pre-Mortem Summary requirements

A concise decision brief (half a page max) containing:
- The plan's overall risk profile in one sentence
- The top two or three threats (the ones that would kill it if ignored)
- The single highest-leverage mitigation (the one action that addresses the
  most risk for the least cost)
- A go/no-go confidence statement: given what the pre-mortem revealed, how
  confident should the user be in proceeding? Be honest.

## Calibration

- **Depth scales with input.** A one-sentence idea gets lighter treatment.
  A detailed project plan gets thorough analysis.
- **Domain expertise matters.** If the plan is in a domain you know well, the
  Imaginers should draw on real industry failure patterns, not generic risk
  categories.
- **Convergence signals are gold.** When multiple isolated Imaginers independently
  identify the same failure, that's a high-confidence risk. Highlight it
  prominently.
- **The greatest strength is the greatest risk.** At least one Imaginer should
  attack whatever the team is most confident about. Klein's research shows this
  is where blind spots concentrate.

## Accepting Disney session output

If the user says "run pre-mortem on the Disney output" or similar, extract the
Realist's plan and Critic's concerns from the Disney session. Feed the Realist's
plan as THE PLAN and append the Critic's concerns as additional context. The
Imaginers should go beyond what the Critic already identified.

## Compact mode

Turn this on when the user asks for it (`--compact`, "keep it short", "just the
summary"), or when the analytical-pipeline skill tells you compact mode is on.

**It changes what you print. It does not change what you run.** Every agent still
runs. The isolation between them is unchanged. You get the same analysis with
less of it on screen.

What you print instead of the per-agent sections:

- One line stating the result. Give the finding and the counts. A number beats an
  adjective. Under 100 characters.
- Then **the Mitigation Plan and Pre-Mortem Summary** in full. Do not compress it. It is the payoff.
- Then one line: `Full agent output is in context. Say which part to expand.`

Example of the one line:

```
15 failure scenarios, 6 Critical, 3 convergence signals.
```

If the user asks to expand, print that section in full. The agents already ran,
so it costs nothing but the printing.

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
