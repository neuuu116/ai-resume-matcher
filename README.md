# FairRank — AI Resume Evaluator

An automated resume-to-job-description matching system built with n8n, GPT-4o-mini, and a custom scoring engine. Evaluates candidates fairly by scoring skills, experience, and education — without any bias from name, gender, or college prestige.

---

## Features

- AI-powered resume parsing via GPT-4o-mini
- AI-powered job description parsing via GPT-4o-mini
- Weighted scoring engine (skills 50% + experience 30% + education 20%)
- Structured JSON output with matched skills, missing skills, and experience gap
- Bias-safe: no name, photo, gender, or institution ranking involved
- Frontend fallback logic (runs locally if backend is offline)

---

## Tech Stack

| Layer | Tool |
|---|---|
| Automation | n8n (self-hosted / cloud) |
| AI Models | GPT-4o-mini (OpenAI) |
| Scoring Logic | Custom JavaScript (n8n Code node) |
| API Entry Point | n8n Webhook (POST) |
| Frontend | HTML + TailwindCSS + Vanilla JS |

---

## n8n Workflow — Node Breakdown

```
Webhook → Resume Extractor → Job Extractor → Scoring Engine → Respond to Webhook
```

### 1. Webhook
- Method: `POST`
- Path: `/fairrank-evaluate`
- Accepts: `{ resume: string, job_description: string }`

### 2. Resume Extractor (GPT-4o-mini)
Extracts structured data from the resume text.

**Output JSON:**
```json
{
  "skills": [],
  "years_experience": 0,
  "domain": "",
  "education_level": "",
  "strengths": []
}
```

> ⚠️ **Important:** The prompt uses `{{ $json.resume }}` — make sure your webhook body sends the key as `resume` (not `resume_text`).

### 3. Job Extractor (GPT-4o-mini)
Extracts structured requirements from the job description.

**Output JSON:**
```json
{
  "required_skills": [],
  "minimum_experience": 0,
  "preferred_education": "",
  "role_type": ""
}
```

### 4. Scoring Engine (JavaScript Code Node)
Custom weighted scoring logic:

| Criteria | Weight | Logic |
|---|---|---|
| Skill Match | 50% | `matched_skills / required_skills * 50` |
| Experience | 30% | Full if meets minimum; partial if below |
| Education | 20% | String match between resume and JD education level |

**Output:**
```json
{
  "overall_score": 78,
  "breakdown": {
    "skill_score": 40,
    "experience_score": 30,
    "education_score": 8
  },
  "matched_skills": ["python", "fastapi"],
  "missing_skills": ["docker", "kubernetes"],
  "experience_gap": 0
}
```

### 5. Respond to Webhook
Returns the scoring result as the HTTP response to the frontend.

---

## Frontend

Built with HTML + TailwindCSS + Vanilla JS.

- User pastes resume text and job description
- Sends `POST` to the n8n webhook
- Displays AI score, matched/missing skills
- **Fallback:** if backend is offline, runs a local keyword match against a hardcoded skill list and shows match percentage

**Webhook URL used:**
```
https://neuuu116.app.n8n.cloud/webhook/fairrank-evaluate
```

---

## Bug Fix Applied

The original `Resume Extractor` prompt referenced `{{ $json.resume_text }}` but the webhook body sends the key as `resume`. This mismatch caused the node to receive empty input.

**Fix:** Change the prompt variable in Resume Extractor from `{{ $json.resume_text }}` to `{{ $json.resume }}`.

---

## How to Import the Workflow

1. Open your n8n instance
2. Go to **Workflows → Import from File**
3. Upload `fairrank_workflow.json`
4. Add your OpenAI API credentials to both AI nodes
5. Activate the workflow
6. Update the webhook URL in your frontend's `script.js`

---

## Future Roadmap

- [ ] Replace GPT-4o-mini with Claude Haiku for cost efficiency
- [ ] Add semantic skill matching (e.g. "ML" matches "machine learning")
- [ ] Store evaluation history in PostgreSQL
- [ ] Build a dashboard to compare multiple candidates side by side
- [ ] Add a confidence score and recommendation label (Strong Match / Partial / Weak)

---

## Project Status

`v2 — Deployed`

n8n cloud workflow active. Frontend deployed on Netlify. Webhook live and accepting requests.
