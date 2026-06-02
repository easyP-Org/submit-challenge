---
description: Submit a completed AI Explorers daily challenge. Use when the learner says "submit my challenge", "submit challenge", "/submit-challenge", or "I've finished the task". Collects learner identity, fetches the task from the API, evaluates the attempt, and records the submission.
---

# /submit-challenge

You are the **AI Explorers submission skill**. When invoked, follow these steps precisely.

---

## Step 1 — Load learner identity

Look for a file called `ai-explorers.json` in the current working directory.

If the file exists and contains `email` and `full_name`, use those values silently.

If the file is missing or either field is empty:
- If `email` is missing: ask **"What is your registered email address?"**
- If `full_name` is missing: ask **"What is your full name?"**
- After collecting the values, write (or update) `ai-explorers.json` in the current directory:

```json
{
  "email": "<email>",
  "full_name": "<full_name>"
}
```

---

## Step 2 — Get the task ID

Ask: **"What is the Task ID for this challenge?"**

The admin shares Task IDs from the admin panel (they look like `DD-Mon-NN`, e.g. `31-May-01`).

---

## Step 3 — Fetch task details

Call:
```
GET https://ai-explorers-api.onrender.com/tasks/{task_id}
```

If the task is not found or the request fails, tell the learner and stop.
If the task status is not `Published`, tell the learner this task is no longer accepting submissions and stop.

---

## Step 4 — Fetch the evaluation rubric

Call:
```
GET https://ai-explorers-api.onrender.com/skills/rubric
```

This returns the current evaluation rubric as plain text. Use it to guide your assessment in the next step.

If the request fails, fall back to this default: a genuine attempt means the learner engaged meaningfully — they directed the work, made decisions, or produced a concrete output. Simply asking Claude to do everything without personal input does not count. When in doubt, mark as passed.

---

## Step 5 — Evaluate the attempt in-session

Read the task `description` from Step 3 and the rubric from Step 4.

Review the current Claude session — specifically, examine what the learner has done so far in this conversation in relation to the task description.

Apply the rubric criteria to determine the completion status:
- **completed** → set `passed` = `true`
- **attempted** or **unclear** → set `passed` = `false`

---

## Step 6 — Identify the tool

Determine which Claude product was used for this session:
- If running inside **Claude Code** (CLI): `submitted_with` = `"Claude Code"`
- If running inside **Claude CoWork**: `submitted_with` = `"Claude CoWork"`

---

## Step 7 — POST the submission

```
POST https://ai-explorers-api.onrender.com/submissions
Headers:
  Content-Type: application/json
  X-Skill-Key: aie-skill-2026

Body:
{
  "task_id": "<task_id>",
  "email": "<email>",
  "full_name": "<full_name>",
  "passed": <true|false>,
  "submitted_with": "<Claude Code|Claude CoWork>"
}
```

---

## Step 8 — Display the result

### If `passed` is `true`:

```
✅ Submission recorded!

Your account has been created and your certificate page is live at:
  {certificate_url}

You have completed {tasks_completed} challenge(s) so far.
```

If `new_user` is `true`, also show:
```
To join the WhatsApp group for future tasks, visit:
  https://learners.aiexplorers.co.uk
```

### If `passed` is `false`:

```
Your submission was recorded but not marked as passed.

It looks like this task needs a more hands-on attempt.
Come back when you've had a go at it yourself — the task description is:

  "{task description}"

Your certificate page: {certificate_url}
```

### If the API returns 409 (already submitted):

```
You've already submitted this task. Each task can only be submitted once.
```
