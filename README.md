# Submit Challenge Plugin

Submit completed [AI Explorers](https://aiexplorers.co.uk) daily challenges and track your progress — directly from Claude Code or Claude CoWork.

## What it does

- Saves your learner identity locally so you only enter it once
- Fetches live task details and evaluation instructions from the AI Explorers API
- Evaluates your session automatically — no need to re-describe your work
- Records your submission and shows your certificate link

## Usage

At the end of a challenge session, run:

```
//submit-challenge
```

You'll be asked for your Task ID (shared by your instructor). Your name and email are saved locally after the first submission so you won't need to enter them again.

## Installation

### Claude CoWork

1. Download `submit-challenge.plugin` (your instructor will share this file)
2. In CoWork, go to **Settings → Capabilities → Plugins**
3. Drag the `.plugin` file in, or click **"Install from file"**

### Claude Code (CLI)

```bash
claude plugin install https://github.com/easyP-Org/submit-challenge
```

## Learner identity file

Your email and name are saved to `ai-explorers.json` in your working directory after the first submission. The file is only used to pre-fill your details on subsequent submissions — it is never shared elsewhere.

## Offline mode

If the backend is unreachable, the skill falls back to the evaluation rubric bundled in `skills/submit-challenge/references/evaluation-rubric.md`.

## Requirements

- Internet access to reach `https://ai-explorers-api.onrender.com`
- A Task ID provided by your AI Explorers instructor
