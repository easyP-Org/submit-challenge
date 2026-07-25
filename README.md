# AI-Qoach Practice companion

Submit completed [AI Maxers](https://aimaxers.com) daily challenges and track your progress — directly from Claude CoWork or Claude Code.

## What it does

- Saves your learner identity locally so you only enter it once
- Fetches live task details and evaluation criteria from the AI Maxers API
- Evaluates your session automatically — no need to re-describe your work
- Records your submission and shows your certificate link

## Usage

At the end of a challenge session, say:

```
//submit-challenge
```

You'll be asked for your Task ID (shared by your instructor). Your name and email are saved locally after the first submission.

---

## Installation

### Claude CoWork

**Step 1 — Install the plugin**

1. Go to **Settings → Capabilities → Plugins** (or the Customize panel)
2. Click **Add from URL** and paste:
   ```
   https://github.com/easyP-Org/submit-challenge
   ```

**Step 2 — Add the connector** *(one-time, per account)*

1. Go to **Settings → Connectors** (or **Custom connectors**)
2. Click **Add connector**
3. Enter the following:
   - **URL:** `https://ai-explorers-mcp.onrender.com/mcp`
   - **Authentication:** leave blank (no auth required)
4. Save

That's it. The connector links your Claude session to the AI Maxers submission system.

---

### Claude Code (CLI)

The plugin configures the connection automatically — no extra steps needed.

```bash
claude plugin marketplace add easyP-Org/submit-challenge@aimaxers
claude plugin install aiexplorers-submit-challenge@ai-explorers
```

This pins your install to the `aimaxers` branch, which is the current release line. If you'd already registered the marketplace before (e.g. pointing at `main`), remove it first with `claude plugin marketplace remove ai-explorers` so the branch above takes effect.

---

## Requirements

- Internet access
- A Task ID provided by your AI Maxers instructor
