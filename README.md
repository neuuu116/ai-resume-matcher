# 🛡️ RakshakAI — Scam Detection Backend System
A backend API system that classifies digital messages 
as **Scam** or **Safe** using rule-based text analysis, 
built with FastAPI and MySQL.

---

## 🚀 Tech Stack

| Layer      | Technology        |
|------------|-------------------|
| Backend    | Python, FastAPI   |
| Database   | MySQL             |
| API Style  | REST (JSON)       |
| Tools      | Git, Postman      |

---

## 📌 What It Does

- Accepts a message via POST request
- Analyzes it using rule-based text detection logic
- Returns classification: `SCAM` or `SAFE`
- Stores result with confidence score and timestamp in MySQL
- Built for future ML model integration

---

## 🏗️ System Architecture
```
User Request (POST /analyze)
        ↓
   FastAPI Router
        ↓
 Detection Logic Layer
  (Rule-based analysis)
        ↓
   MySQL Database
  (Logs + Results)
        ↓
  JSON Response returned
```

---

## 📂 Project Structure
```
rakshak-ai-backend/
│
├── main.py              # FastAPI app entry point
├── models.py            # Database models
├── schemas.py           # Pydantic API schemas
├── detection.py         # Scam detection logic
├── database.py          # MySQL connection setup
├── requirements.txt     # Dependencies
└── README.md
```

---

## ⚙️ API Endpoints

| Method | Endpoint      | Description                    |
|--------|---------------|--------------------------------|
| GET    | /             | Health check                   |
| POST   | /analyze      | Classify a message             |
| GET    | /logs         | Retrieve all message logs      |
| GET    | /logs/{id}    | Get specific message result    |
| DELETE | /logs/{id}    | Delete a log entry             |

---

## 📥 Sample Request
```json
POST /analyze
{
  "message": "Congratulations! You won a prize. 
               Click here to claim now."
}
```

## 📤 Sample Response
```json
{
  "message_id": 101,
  "classification": "SCAM",
  "confidence_score": 0.91,
  "timestamp": "2026-01-15T10:32:00"
}
```

---

## 🗄️ Database Schema
```sql
CREATE TABLE message_logs (
  id              INT AUTO_INCREMENT PRIMARY KEY,
  message_text    TEXT NOT NULL,
  classification  VARCHAR(10),
  confidence_score FLOAT,
  timestamp       DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

---

## 🚀 How To Run Locally

**1. Clone the repo**
```bash
git clone https://github.com/neuuu116/rakshak-ai-backend
cd rakshak-ai-backend
```

**2. Install dependencies**
```bash
pip install -r requirements.txt
```

**3. Setup MySQL**
```bash
# Create database
CREATE DATABASE rakshakai;
# Run schema file
source schema.sql
```

**4. Run the server**
```bash
uvicorn main:app --reload
```

**5. Test with Postman or browser**
```
http://localhost:8000/docs  ← FastAPI auto docs
```

---

## 🔮 Future Improvements

- [ ] Integrate ML classification model
- [ ] Add authentication (JWT)
- [ ] Deploy on AWS EC2 / Railway
- [ ] Add bulk message analysis endpoint
- [ ] Build analytics dashboard for scam trends

---

## 👩‍💻 Built By

**Neha Mhatre**  
B.E. Computer Engineering | IIT Madras B.S. Data Science  
[LinkedIn]((http://www.linkedin.com/in/neha-mhatre-693055336)) | 
[GitHub](https://github.com/neuuu116)
```
---

## How To Use This

**Step 1:** Go to your rakshak-ai-backend repo on GitHub

**Step 2:** Click on README.md → Edit (pencil icon)

**Step 3:** Delete whatever is there now

**Step 4:** Paste this entire thing

**Step 5:** Fill in 2 things:
- Your actual LinkedIn URL
- Check if your file names match what I wrote in Project Structure — if different, update them


