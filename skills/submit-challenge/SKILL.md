---
name: submit-challenge
description: >
  AI Explorers challenge submission skill. Triggered when the user says
  "//submit-challenge", "submit my challenge", "submit challenge", or
  "submit my AI Explorers task". Validates the learner's Task ID, evaluates
  their work from the current conversation, and submits the result to the
  AI Explorers leaderboard. Do NOT invoke for general questions — only when
  the user explicitly wants to submit a completed task.
tools:
  - WebFetch
---

## //submit-challenge — AI Explorers Submission Workflow

Follow each step in order. Do not skip steps or reorder them.

---

### Step 0 — Fetch Live Instructions

Before doing anything else, call:

```
GET https://aiexplorers-backend.onrender.com/skills/instructions
```

Parse the JSON response. It contains:
- `workflow` — ordered step definitions
- `evaluationCriteria` — rubric to score the learner's work
- `submissionFields` — exact fields required in the POST payload

**If the fetch fails**, continue using the built-in workflow below and the offline rubric in `references/evaluation-rubric.md`. Inform the user: "Using offline instructions — live instructions unavailable."

---

### Step 1 — Ask for Task ID

Say:

> "Welcome to the AI Explorers challenge submission! Please enter your **Task ID** to get started."

Wait for the user to provide a Task ID (format: `DD-Mon-NN`, e.g. `03-Jun-01`). Accept whatever they type.

---

### Step 2 — Validate the Task ID

Call:

```
GET https://aiexplorers-backend.onrender.com/tasks/{taskId}
```

**HTTP 200 — valid task:**
Store the full response as `taskDetails` (you will use `title`, `description`, `context` in the evaluation step).
Say: "Task ID validated. Your submission is now being evaluated."
Proceed to Step 3.

**HTTP 404 or any error:**
Say: "Task ID is invalid. Please try again."
Return to Step 1.

---

### Step 3 — Evaluate the Work from Conversation Context

The learner has already completed their work before calling this skill. Evaluate what they did by reviewing **the current conversation history** — everything that was discussed and produced in this session before `//submit-challenge` was called.

Do NOT ask the learner to paste or re-describe their work.

Assess against the task (`taskDetails.description`, `taskDetails.context`) and the rubric below (or from the live instructions):

| Status | `passed` | When to use |
|--------|----------|-------------|
| **completed** | `true` | Genuine effort; real tangible output produced in this session |
| **attempted** | `false` | Started but incomplete; surface-level only |
| **unclear** | `false` | Off-topic or too little context to judge |

When in doubt between *completed* and *attempted*, choose *completed*.

Tell the learner:

> "Evaluation complete — **[completed / attempted / unclear]**.
> [2–3 sentences explaining what was strong and what (if anything) was missing.]
>
> Collecting your details to record the submission..."

Store the boolean as `passed`.

---

### Step 4 — Collect Learner Identity

Ask in a single message:

> "Almost done! I just need a couple of details for your submission record:
> 1. Your **full name**
> 2. Your **email address**"

Store as `fullName` and `email`.

---

### Step 5 — Detect Tool

Check the environment:
- If running inside **Claude Code** (CLI), set `submittedWith` = `"Claude Code"`
- If running inside **Claude CoWork** (desktop), set `submittedWith` = `"Claude CoWork"`
- If uncertain, ask: "Are you using Claude Code (terminal) or Claude CoWork (desktop app)?"

---

### Step 6 — Submit to the Leaderboard

Send:

```
POST https://aiexplorers-backend.onrender.com/submissions
Content-Type: application/json
```

```json
{
  "task_id": "<taskId from Step 1>",
  "full_name": "<fullName from Step 4>",
  "email": "<email from Step 4>",
  "passed": <boolean from Step 3>,
  "submitted_with": "<submittedWith from Step 5>"
}
```

**HTTP 200 / 201 — success:**

> "Your submission has been recorded! 🎉
>
> - **Task ID:** {task_id}
> - **Result:** {completed / attempted / unclear}
> - **Submitted by:** {full_name} ({email})
> - **Certificate:** {certificate_url}
>
> Thanks for participating in AI Explorers!"

**HTTP 409 — already submitted:**

> "This task has already been submitted under your email address. Each task can only be submitted once."

**Any other error (4xx / 5xx):**

> "There was a problem submitting your results (error {status}). Please try again or contact the AI Explorers team with your Task ID: {task_id}."

Do not retry automatically.
