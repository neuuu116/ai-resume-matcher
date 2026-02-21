

# 🤖 AI Resume Matcher

### AI-Powered Resume Evaluation System using n8n + OpenAI

An intelligent resume-to-job matching system that evaluates how well a candidate fits a job role using AI-based structured extraction and weighted scoring logic.

Built using **n8n automation**, **OpenAI GPT-4o-mini**, and **custom JavaScript scoring logic**.

---

## 📌 Project Overview

Recruiters often manually compare resumes with job descriptions — a slow and biased process.

This project automates that evaluation by:

* Extracting structured data from resumes
* Extracting structured requirements from job descriptions
* Matching skills, experience, and education
* Generating a weighted match score
* Returning structured, bias-aware results

This system is **stateless** and does not use a database. It processes input dynamically and returns real-time evaluation results.

---

## 🏗 System Architecture

### Flow Overview

1️⃣ User pastes:

* Resume text
* Job description

2️⃣ Frontend sends data to n8n Webhook

3️⃣ Resume Extractor (OpenAI GPT-4o-mini)

* Extracts:

  * skills
  * years_experience
  * domain
  * education_level
  * strengths

4️⃣ Job Extractor (OpenAI GPT-4o-mini)

* Extracts:

  * required_skills
  * minimum_experience
  * preferred_education
  * role_type

5️⃣ JavaScript Scoring Engine (Code Node in n8n)

* Matches skills
* Compares experience
* Checks education match
* Calculates weighted final score

6️⃣ Respond to Webhook → Result displayed in frontend

---

## 🧠 Scoring Logic

The scoring engine uses weighted evaluation:

### 🔹 1. Skill Match — 50%

```
(skill_matches / total_required_skills) * 50
```

* Identifies matched skills
* Lists missing skills

---

### 🔹 2. Experience Match — 30%

* Full 30 points if:

  ```
  resume_experience >= required_experience
  ```
* Otherwise proportional score:

  ```
  (resume_experience / required_experience) * 30
  ```

---

### 🔹 3. Education Match — 20%

* Full 20 points if resume education includes required education
* Otherwise 0

---

### 🔹 Final Score

```
Final Score = Skill Score + Experience Score + Education Score
```

Output example:

```json
{
  "overall_score": 82,
  "breakdown": {
    "skill_score": 40,
    "experience_score": 30,
    "education_score": 12
  },
  "matched_skills": ["python", "sql"],
  "missing_skills": ["docker"],
  "experience_gap": 1
}
```

---

## 📂 Project Structure

```
AI-Resume-Matcher/
│
├── agent-x.html          # Frontend UI
├── n8n-workflow.json     # Complete AI extraction + scoring logic
├── README.md
```

---

## ⚙ Tech Stack

| Layer             | Technology                 |
| ----------------- | -------------------------- |
| Frontend          | HTML + CSS                 |
| Automation Engine | n8n                        |
| AI Model          | OpenAI GPT-4o-mini         |
| Logic Engine      | JavaScript (n8n Code Node) |
| Database          | None (Stateless system)    |

---

## 🛡 Bias-Safe Design

This system intentionally ignores:

* Name
* Gender
* Age
* College prestige
* Personal identifiers

Evaluation is based only on:

* Skills
* Experience
* Education relevance

---

## 🚀 How to Run

### 1️⃣ Import Workflow

* Open n8n
* Import `n8n-workflow.json`
* Activate workflow

### 2️⃣ Setup OpenAI Credentials

* Add OpenAI API key inside n8n

### 3️⃣ Connect Frontend

* Copy Webhook URL
* Replace fetch endpoint in `agent-x.html`

### 4️⃣ Test

* Paste resume text
* Paste job description
* Click "Analyze Match"
* View structured output

---

## 📌 Current Limitations

* No database (no history storage)
* No authentication
* No PDF upload (text only)
* Depends on AI extraction accuracy
* No deployed public version

---

## 🔮 Future Improvements

* Add Supabase / PostgreSQL for storing evaluations
* Add recruiter dashboard
* Add PDF resume upload
* Add analytics (top skill gaps)
* Deploy frontend on Vercel / Netlify
* Add multi-role batch processing

---

## 🎯 Why This Project Matters

This project demonstrates:

* AI prompt engineering
* Structured JSON extraction
* Automation workflow design (n8n)
* Custom scoring logic
* Bias-aware evaluation systems
* Real-world hiring automation use case

This is not just a UI project — it is an applied AI decision engine.



