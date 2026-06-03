# AI Explorers — Evaluation Rubric (Offline Fallback)

Use this when `GET /skills/instructions` is unreachable.
The live endpoint always takes precedence.

## Completion Status

### completed → `passed: true`
The learner meaningfully engaged with the task. They asked Claude to help them
build, run, or analyse something related to agentic AI — and there is evidence
in the conversation that they got a real output (a working prompt, an agent flow,
a summary, a piece of code, an analysis). The session does not need to be perfect;
genuine effort and a tangible result is enough.

**Strong evidence (favour "completed"):**
- Learner iterated on a prompt or agent more than once
- A concrete output was produced (code, summary, structured data, workflow steps)
- Learner tested or evaluated the output in some way
- Learner encountered an error and worked through it

### attempted → `passed: false`
The learner started but either gave up early, only asked surface-level questions
without building anything, or the conversation is too short to show real engagement
(fewer than ~5 substantive exchanges).

**Weak evidence (favour "attempted"):**
- Only asked what something is, without building it
- Single-turn interaction with no follow-up
- Conversation is about the task topic but not actually doing the task

### unclear → `passed: false`
Not enough conversation context to make a judgment — e.g. the session is entirely
off-topic or the learner only typed one or two messages.

## Guidance

When in doubt between "completed" and "attempted", choose "completed".
The goal is to encourage learners, not gatekeep.
