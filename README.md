# Submit Challenge Plugin

Submit completed [AI Explorers](https://learners.aiexplorers.co.uk) daily challenges and track your progress — directly from Claude Code or Claude CoWork.

## What it does

- Saves your learner identity locally so you only enter it once
- Fetches the task details from the AI Explorers API
- Evaluates whether you made a genuine attempt at the challenge
- Records your submission and shows your certificate link

## Usage

At the end of a challenge session, run:

```
/submit-challenge
```

You'll be asked for your email, full name (first time only), and the Task ID shared by your instructor.

## Installation

### Claude CoWork
Install from the CoWork plugin library or drag the `.plugin` file into the app.

### Claude Code (CLI)
```bash
claude plugin install submit-challenge
```
Or install from a local file:
```bash
claude plugin install /path/to/submit-challenge.plugin
```

## Learner identity file

Your email and name are saved to `ai-explorers.json` in your working directory so you don't have to re-enter them. This file is only used by the AI Explorers API — it is never shared elsewhere.

## Requirements

- Internet access to reach `https://ai-explorers-api.onrender.com`
- A Task ID provided by your AI Explorers instructor
