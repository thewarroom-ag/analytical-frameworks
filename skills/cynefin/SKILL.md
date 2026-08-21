---
name: cynefin
description: |
  Run Dave Snowden's Cynefin Framework analysis on any problem, situation, or
  decision. Classifies the problem into a Cynefin domain (Clear, Complicated,
  Complex, Chaotic, Disorder), challenges the classification adversarially, then
  produces a domain-appropriate response strategy. Use whenever the user says
  'cynefin', 'what kind of problem is this', 'is this complex or complicated',
  'sense-making', 'what approach should I take', 'how should I think about this',
  'classify this problem', or asks which methodology or framework to apply to a
  situation. Also trigger when the user seems to be applying the wrong type of
  thinking to a problem (e.g., detailed planning for an emergent situation, or
  experimentation for a well-understood problem). This skill REQUIRES subagent
  support (Claude Code).
---

# Cynefin Framework: Sense-Making for Decision-Making

Dave Snowden's Cynefin Framework (1999, refined 2007 with Mary Boone in Harvard
Business Review) is a sense-making device that helps decision-makers identify
what kind of situation they're in before choosing how to respond. The central
insight: different problem domains require fundamentally different approaches,
and the most common error is applying a Complicated-domain response (analysis,
expertise, planning) to a Complex-domain problem (emergent, unpredictable,
requiring experimentation).

This skill uses three agents: a Classifier to determine the domain, a Challenger
to adversarially test the classification, and a Responder to produce the
domain-appropriate strategy.

## Required subagents

- `cynefin-classifier.md`: Determines the Cynefin domain
- `cynefin-challenger.md`: Argues for the strongest alternative classification
- `cynefin-responder.md`: Produces domain-appropriate response strategy

## Orchestration sequence

### Step 1: Capture the situation

Take the user's problem, situation, or decision context. If it's vague, ask
one clarifying question focused on: "What outcome are you trying to achieve,
and what's making it difficult?"

### Step 2: Spawn Classifier and Challenger in parallel

Both agents receive the same situation description. The Classifier determines
the domain. The Challenger independently considers the situation and will later
argue for an alternative. Spawning them in parallel means the Challenger forms
its own first impression before seeing the Classifier's output.

**Classifier prompt:**
```
Classify this situation using the Cynefin Framework:

THE SITUATION:
[user's problem/situation]

CONTEXT:
[any additional context]

Determine the domain and provide your reasoning.
```

**Challenger prompt (first pass, independent assessment):**
```
Read this situation and form your own preliminary Cynefin domain assessment.
Note which domain you think it belongs to and why. You will receive the
official Classifier's assessment next and will need to argue for an alternative.

THE SITUATION:
[user's problem/situation]

CONTEXT:
[any additional context]

Provide your preliminary assessment.
```

### Step 3: Feed Classifier output to Challenger

After both complete, send the Classifier's full output to the Challenger for
the adversarial challenge:

```
Here is the official domain classification. Your job is to argue for the
strongest alternative classification.

YOUR PRELIMINARY ASSESSMENT:
[Challenger's first-pass output]

THE CLASSIFIER'S ASSESSMENT:
[full Classifier output]

THE ORIGINAL SITUATION:
[user's situation]

Produce your Domain Challenge.
```

### Step 4: Resolve the classification (main agent)

Review both the Classifier and Challenger outputs. Make a final domain
determination. If the Challenger raised valid concerns, adjust. If the
Challenger confirmed the classification, proceed with higher confidence.

State your resolution explicitly:
- "The Classifier's assessment holds. Domain: [X]. The Challenger's alternative
  was considered but [reason for rejection]."
- OR "The Challenger raised a valid point. Reclassifying from [X] to [Y]
  because [reason]."
- OR "The situation spans multiple domains. Primary: [X], Secondary: [Y]."

### Step 5: Spawn the Domain Responder

Invoke `cynefin-responder` with the resolved classification:

```
Produce a domain-appropriate response strategy for this situation:

THE SITUATION:
[user's situation]

RESOLVED DOMAIN CLASSIFICATION: [domain]
CLASSIFICATION CONFIDENCE: [High/Medium/Low]
CLASSIFICATION REASONING: [brief summary of resolution]

If the Challenger raised valid secondary concerns:
SECONDARY DOMAIN CONSIDERATIONS: [any relevant challenger points]

Deliver your domain response strategy.
```

### Step 6: Present the complete analysis

```
## 🧭 Domain Classification
[Classifier's output, with your resolution notes]

## 🔄 Domain Challenge
[Challenger's output]

## 📐 Classification Resolution
[Your resolution, 2-3 sentences]

## 🎯 Domain Response
[Responder's output]

## 🗺️ Navigation Brief
[Your synthesis, see requirements below]
```

## Navigation Brief requirements

A concise summary containing:

**Domain determination:** One sentence confirming the domain and why it matters
for how the user should approach this.

**The critical misclassification to avoid:** What the user might naturally assume
about this problem that would lead them to the wrong approach. Be specific.
"You might be tempted to [wrong approach] because [understandable reason], but
this is a [correct domain] problem which requires [correct approach] instead."

**Immediate next action:** One concrete step, derived from the Responder's
strategy, that the user should take first.

**When to reclassify:** What conditions would indicate the domain has shifted,
requiring a different approach. Domains are not static. Complex problems can
become Chaotic (crisis). Complicated problems can become Clear (once expertise
resolves the unknowns). Identify the triggers.

## Calibration

- **The Challenger is the safety mechanism.** The most valuable Cynefin output
  is correct classification. The Challenger prevents premature certainty.
- **Complex vs. Complicated is the critical distinction.** Most organizational
  problems that involve people, markets, or culture are Complex. Most technical
  problems with known physics are Complicated. The boundary between them is
  where classification errors concentrate.
- **Chaotic requires urgency.** If the Classifier identifies Chaotic, the
  Responder should lead with immediate action, not analysis.
- **Clear domains still have value.** Not every problem needs deep analysis.
  If it's Clear, say so quickly and move on. The value is in preventing
  overthinking, not in generating analysis for its own sake.

## Integration with other skills

Cynefin works well as a pre-filter before other analytical frameworks:
- If the domain is Complex → consider Disney Creative Strategy (vision + plan
  under uncertainty) or safe-to-fail experiments
- If the domain is Complicated → consider standard analytical approaches, expert
  consultation
- If the domain is Chaotic → skip frameworks entirely, act first
- Pre-Mortem and Inversion work in any domain but are most valuable in
  Complicated and Complex where the plan exists but hasn't been stress-tested

## Compact mode

Turn this on when the user asks for it (`--compact`, "keep it short", "just the
summary"), or when the analytical-pipeline skill tells you compact mode is on.

**It changes what you print. It does not change what you run.** Every agent still
runs. The isolation between them is unchanged. You get the same analysis with
less of it on screen.

What you print instead of the per-agent sections:

- One line stating the result. Give the finding and the counts. A number beats an
  adjective. Under 100 characters.
- Then **the Navigation Brief** in full. Do not compress it. It is the payoff.
- Then one line: `Full agent output is in context. Say which part to expand.`

Example of the one line:

```
Complex. Challenger argued Complicated. Classification held.
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
