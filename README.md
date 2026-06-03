# submit-challenge

A Claude Cowork / Claude Code plugin for the **AI Explorers** programme. Guides learners through submitting a completed daily challenge — validating their Task ID, evaluating their work locally, and recording the result on the leaderboard.

## Install from GitHub

In Claude Cowork, open **Plugins → Install from URL** and paste:

```
https://github.com/ankit-residenceessential/ai-explorers-submit-challenge
```

Or install manually by downloading the `.plugin` file from the [Releases](https://github.com/ankit-residenceessential/ai-explorers-submit-challenge/releases) page.

## Usage

After completing an AI Explorers task, type:

```
//submit-challenge
```

The skill will:
1. Fetch live evaluation instructions from the AI Explorers backend
2. Ask you for your **Task ID**
3. Validate the Task ID against the server
4. Ask you to share your work output
5. Evaluate your submission using the official rubric
6. Ask for your **name** and **email**
7. Submit your result to the leaderboard

## Requirements

- Claude Cowork (desktop) or Claude Code (CLI)
- Internet access to reach `https://aiexplorers-backend.onrender.com`
- A valid AI Explorers Task ID

## API Endpoints Used

| Method | Endpoint | Purpose |
|--------|----------|---------|
| GET | `/api/instructions` | Fetch live workflow + evaluation criteria |
| GET | `/api/tasks/{taskId}` | Validate a Task ID |
| POST | `/api/submissions` | Record final submission |

## Offline Mode

If the backend is unreachable, the skill falls back to the built-in workflow and evaluation rubric bundled in `skills/submit-challenge/references/evaluation-rubric.md`.

## Support

File issues at the [AI Explorers GitHub repository](https://github.com/ankit-residenceessential/ai-explorers-submit-challenge/issues).
