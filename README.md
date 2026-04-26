# SkillLens – AI Skill Assessment Agent

> A fully local, privacy-first skill assessment tool that conducts conversational technical interviews and generates skill gap reports — powered by [LM Studio](https://lmstudio.ai).

---

## Overview

SkillLens is a single-file HTML application that acts as an AI interviewer. Given a job description and a candidate's resume, it automatically extracts the relevant technical skills, conducts a conversational chat-based assessment for each one, scores the candidate's proficiency, and generates a personalised skill gap report with a learning plan.

All AI inference runs **100% locally on your machine** via LM Studio — no data leaves your device, no API keys required.

---

## Features

- **Automatic skill extraction** — Parses the JD and resume to identify up to 6 key technical skills to assess.
- **Conversational assessment** — Conducts a focused Q&A for each skill (max 2 questions) and scores depth of knowledge on a 0–10 scale.
- **Real-time progress sidebar** — Tracks which skills have been assessed and displays live scores.
- **Skill gap analysis** — Classifies skills as strong / moderate / weak and highlights critical vs. moderate gaps.
- **Personalised learning plan** — Produces a ranked, prioritised study plan with resources and estimated time per skill.
- **Fully local & private** — Runs entirely through LM Studio's OpenAI-compatible local API. Zero external calls.
- **Zero dependencies** — Single `.html` file; no build step, no npm, no framework.

---

## Prerequisites

1. **LM Studio** — Download from [lmstudio.ai](https://lmstudio.ai) and install it.
2. A compatible **chat model** loaded in LM Studio, for example:
   - `mistral-7b-instruct`
   - `llama-3-8b`
   - `phi-3-mini`
   - `qwen2.5-coder-1.5b-instruct` *(default, good for low-RAM machines)*

---

## Setup

1. Open **LM Studio** and download a chat model of your choice.
2. Go to the **Local Server** tab and click **Start Server** (default port: `1234`).
3. Enable **CORS** in the LM Studio server settings.
4. Open `skill_assessment_agent.html` in any modern browser.
5. In the config bar at the top, confirm the server URL (`http://localhost:1234`) and the model name match your LM Studio setup.
6. Click **Reconnect** (or wait for the auto-connection check on load).

---

## Usage

The app follows a 4-step workflow:

### Step 1 — Context
Paste the **Job Description** and the **Candidate's Resume** into the two text areas, then click **Analyse → Extract Skills**.

### Step 2 — Skills
Review the skills extracted by the AI. Toggle any skills on or off, then click **Start Assessment**.

### Step 3 — Assessment
The AI interviewer asks up to 2 targeted questions per skill. Type your answers in the chat box and press **Enter** to submit. Scores are assigned automatically after each skill and the next skill begins.

### Step 4 — Results
Once all skills are assessed, a report is generated showing:
- **Overall score** and **role readiness percentage**
- Per-skill proficiency bars (strong / moderate / weak)
- **Critical and moderate skill gaps**
- A **ranked learning plan** with resources and estimated weeks per skill

---

## Configuration

| Setting | Default | Description |
|---|---|---|
| LM Studio URL | `http://localhost:1234` | Base URL of the local LM Studio server |
| Model name | `qwen2.5-coder-1.5b-instruct` | The model loaded in LM Studio |

Both can be changed directly in the config bar within the app. If only one model is loaded in LM Studio, the model name field auto-fills on connection.

---

## How It Works

SkillLens communicates with LM Studio's OpenAI-compatible `/v1/chat/completions` endpoint. There are three distinct AI calls:

1. **Skill extraction** — Sends the JD and resume (truncated to 600 chars each) and receives a JSON object of required skills, candidate skills, and gaps.
2. **Assessment questions** — Uses a technical interviewer system prompt to ask skill-specific questions and parse `SCORE:[0-10] | RATIONALE:[...]` markers from responses.
3. **Report generation** — Aggregates all skill scores and requests a structured JSON report containing overall score, readiness percentage, skill levels, gaps, and a learning plan.

To keep memory usage low (targeting 4 GB RAM models), conversation history is capped at the last 6 messages during assessment.

---

## Browser Compatibility

Any modern browser with `fetch` support — Chrome, Firefox, Safari, Edge. No installation or build step required.

---

## Limitations

- Skill extraction is capped at **6 skills** per session to keep assessments concise.
- Input text (JD and resume) is truncated to **600 characters each** when sent to the model to stay within context limits for smaller models.
- Report quality depends on the capability of the locally loaded model. Larger models (7B+) produce more accurate and nuanced results.
- The app does not persist state between page refreshes.

---

## File Structure

```
skill_assessment_agent.html   # The entire application — HTML, CSS, and JS in one file
README.md                     # This file
```

---

## License

This project is provided as-is for personal and educational use.
