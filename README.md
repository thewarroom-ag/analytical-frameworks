# Analytical Frameworks

Four decision-making frameworks, each rebuilt as a set of agents that argue with each other instead of one model talking to itself.

It is not software. It is a folder of plain text instructions plus agent definitions. Anything that can hand files to a model and spawn subagents should run it. It was built for Claude Code and that is the only place it has been exercised.

---

## The problem it addresses

Ask one model to stress-test a plan and it will produce a list. The list will be articulate, well-organised, and shaped by everything it has already said in the conversation. It agreed with the plan four messages ago, so its criticism arrives pre-softened. It generates its objections in the same pass it generates the reassurance, from the same context, in the same voice.

That is not analysis. It is one perspective wearing four hats.

These frameworks split the work across agents that cannot see each other's output. Three failure analysts write independently and only then get compared. A classifier is answered by a challenger whose only job is to argue the opposite. A forward case and an inversion case are generated in isolation and a third agent measures the gap between them.

When independent agents converge on the same concern without coordination, that convergence is information. That is the whole design.

---

## The four frameworks

| Framework | Origin | Agents | What it is for |
|---|---|---|---|
| **Cynefin** | Dave Snowden, 1999 | Classifier, Challenger, Responder | Deciding what kind of problem you have before choosing how to attack it |
| **Disney Creative Strategy** | Walt Disney, documented by Robert Dilts | Outsider, Dreamer, Realist, Critic | Developing an idea through four incompatible mindsets in sequence |
| **Pre-Mortem** | Gary Klein, HBR 2007 | Three Failure Imaginers, Triage Analyst | Finding what kills a plan, before you commit to it |
| **Inversion** | Munger, after Jacobi | Forward Case, Inversion, Delta Analyst | Testing a decision against the strongest case for and against |

A fifth skill, **analytical-pipeline**, chains all four in order and produces a synthesis with a kill list.

---

## What each one actually does

**Cynefin** classifies a situation as Clear, Complicated, Complex, Chaotic, or Disorder, then a Challenger argues for the most plausible alternative classification. The challenge stage exists because the expensive error is treating a Complex problem as a Complicated one, which produces detailed plans for a system that will not hold still. The Challenger is required to argue against the classification even when it agrees.

**Disney** runs four rooms in sequence, each seeing what came before. The Outsider frames it cold. The Dreamer builds the boldest version with no feasibility constraint. The Realist converts it to something buildable. The Critic tries to break it. The separation matters because a single pass blends visionary and pragmatist into a hedge that is neither.

**Pre-Mortem** tells three agents the plan has already failed and asks them to reconstruct why. They work in parallel and in isolation. A Triage Analyst then merges, ranks by likelihood and impact, and flags two things: convergence signals, where independent agents found the same failure, and lone signals, where only one did. Klein's original method has team members write independently before sharing. This is that, with the writing parallelised.

**Inversion** generates the strongest case for success and the strongest case for guaranteed failure in genuine isolation, then a Delta Analyst identifies where they agree, where they diverge, and what neither addressed.

---

## Architecture

It is a DAG, not a chain. A directed acyclic graph: the arrows run one way, and nothing loops back into a stage that already ran. Five stages in sequence, two of which branch and rejoin.

### The sequential part

`analytical-pipeline` runs the four frameworks in order, each injecting context into the next.

```
Cynefin  -->  Disney  -->  Pre-Mortem  -->  Inversion  -->  Synthesis
   |            |              |               |
 domain      Realist's      Triage +        Forward vs
 + strategy  plan +         Mitigation      Inverse,
             Synthesis                      plus the gap
```

The Cynefin domain changes how the Disney Realist plans: safe-to-fail experiments if the problem is Complex, prescriptive timelines if it is Complicated. Disney's plan becomes the thing the Pre-Mortem is told has already failed. The Pre-Mortem's ranked risks tell the Inversion agents where to aim.

### The part that is deliberately not sequential

Two frameworks are chains inside. Two are not, and that is load-bearing.

```
CHAIN                          FAN-OUT, THEN FAN-IN
(Cynefin, Disney)              (Pre-Mortem, Inversion)

  Classifier                     Imaginer A ----\
      |                          Imaginer B -----> Triage
      v                          Imaginer C ----/
  Challenger                          |
      |                               v
      v                          Mitigation
  Responder
```

Cynefin and Disney have to run in sequence. The Challenger cannot argue against a classification that does not exist yet. The Critic needs something built before it can attack it.

Pre-Mortem and Inversion must not. If one Imaginer could see another's output, agreement between them would be contamination rather than evidence. The convergence signal in the Triage Analysis carries information only because none of the three writers could influence each other. Chain them and you destroy the thing the framework exists to produce.

| Framework | Internal shape | Why |
|---|---|---|
| Cynefin | Chain | The Challenger needs a classification to attack |
| Disney | Chain | Each room builds on the last; the Critic needs something to break |
| Pre-Mortem | Fan-out, fan-in | Independent agreement is the signal; contact destroys it |
| Inversion | Fan-out, fan-in | The bull and bear cases must not see each other |

### One withheld edge

In the Inversion stage, the Forward Case agent receives the original idea plus the Disney Dreamer's vision, so the bull case is built with every advantage available. The Inverse agent receives only the original idea.

That asymmetry is deliberate. The pessimist stays uncontaminated by the optimist's framing.

It is also why DAG is the accurate word. A chain is a straight line and cannot express an edge that is granted to one branch and withheld from the other.

### Cost

Acyclic means no stage feeds back into one that already ran, so every run terminates. The full pipeline spawns roughly twelve subagents and finishes. A single framework spawns two to four.

---

## What it does not do

It does not make the model correct. Every agent in this repo runs on a large language model and inherits its failure modes, including the ability to produce a confident, well-formatted analysis of a situation it has misread.

What it changes is the shape of what comes back. A single-pass critique gives you a list with no confidence signal attached, and no way to tell a real risk from a plausible sentence. This gives you a ranked inventory where the ranking is derived from independent agreement rather than asserted.

No controlled measurement has been run on this. There is no eval suite, no baseline comparison, and no claim here that the multi-agent version outperforms a well-prompted single pass. What can be said is that the outputs are structurally different: they carry convergence counts, isolation guarantees, and dissent that was generated before the agent could see the consensus. Whether that is worth the token cost is a judgement, not a finding.

It is not cheap and it is not for small questions. See Architecture for the agent count.

---

## Install

Copy the three directories into your Claude Code configuration:

```bash
cp -r skills/*   ~/.claude/skills/
cp -r agents/*   ~/.claude/agents/
cp -r commands/* ~/.claude/commands/
```

Restart Claude Code so the slash commands register.

For a project-scoped install, use `.claude/` inside the project instead of `~/.claude/`.

---

## Use

Either invoke the skill by name, or use the slash command:

```
/premortem   <your plan>
/cynefin     <your situation>
/disney      <your idea>
/invert      <your decision>
/pipeline    <anything worth all four>
```

The skills also trigger on natural phrasing. "What could go wrong with this" reaches the Pre-Mortem. "Is this complex or complicated" reaches Cynefin. "Argue the other side" reaches Inversion.

Give them something with enough substance to attack. A one-sentence idea gets a one-sentence-idea analysis. The frameworks scale their depth to what you hand them, and a vague plan produces vague failure modes.

---

## Attribution

The four thinking methods are not ours. Klein, Snowden, Disney by way of Dilts, and Munger after Jacobi. Each skill cites its source in the text.

What is claimed here is the implementation: the multi-agent decomposition, the isolation rules, the adversarial and consolidation stages, the convergence scoring, and the chain.

Authored by Shadow, CEO, The War Room.

Source-available, attribution required. See [LICENSE](LICENSE).
