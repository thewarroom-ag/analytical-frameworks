# Changelog

## 2.1.0

- Casework handoff on `analytical-pipeline`. When a `CASE.md` file sits in the
  working directory or a parent, a finished run appends a six-line note to it:
  what the run settled, what it could not settle, which tool comes next and
  why, where the artifact is, and the fact that would overturn the answer.
  Five skills covered one pipeline and none of them knew the others existed,
  so work could not travel between them without a person retyping the context
  into the next tool. The block sits on the pipeline alone, because that is
  the skill that produces a final artifact. With no `CASE.md` present, a run
  behaves exactly as it did before. The format belongs to Casework.
- `author` field in the `analytical-pipeline` frontmatter.

## 2.0.0

Breaking: compact output is now the DEFAULT. Add `--full` for the old behaviour.

- Compact by default on every skill and command. One result line per stage, the
  synthesis in full, then an offer to expand. Still runs every agent; the flag
  changes what is printed, never what runs.
- `--full` restores the complete per-stage output.

## 1.1.0

- Compact mode on every skill and command. One result line per stage, the
  synthesis in full, then an offer to expand. Changes what is printed, not what
  is run.
- Architecture section in the README: the DAG shape, why two frameworks fan out
  instead of chaining, and the edge withheld from the Inversion pessimist.
- One voice standard across all agent output (VOICE.md).
- Use section now states that you can run one framework or all four.

## 1.0.0

First public release.

- Cynefin: Classifier, Challenger, Responder
- Disney Creative Strategy: Outsider, Dreamer, Realist, Critic
- Pre-Mortem: three parallel Failure Imaginers, Triage Analyst
- Inversion: Forward Case, Inversion, Delta Analyst
- analytical-pipeline: chains all four with a closing synthesis and kill list
- Slash commands for all five
