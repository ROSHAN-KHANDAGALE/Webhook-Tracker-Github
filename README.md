# 🚀 GitHub Webhook Tracker (Flask + MongoDB)

A lightweight, production-style Flask application that listens to **GitHub Webhooks** (Push, Pull Request, Merge events), stores normalized activity data in **MongoDB**, and displays repository activity on a clean UI that refreshes every **15 seconds**.

This project is designed to closely mirror **real-world GitHub integrations** and is suitable for assignments, interviews, and portfolio demonstrations.

---

## 📌 Problem Statement (What This Solves)

* Capture GitHub repository activities using **GitHub Webhooks**
* Handle the following events:

  * **PUSH**
  * **PULL REQUEST (Opened)**
  * **MERGE (PR Closed & Merged)**
* Store only the required fields in MongoDB
* Display the latest repository activities on a UI
* UI refreshes automatically every **15 seconds**

---

## 🏗️ Architecture Overview

```
GitHub Repository
        │
        │  Webhook Events
        ▼
Flask Webhook API  ─────▶ MongoDB
        │
        │  REST API (/events)
        ▼
Flask UI (Polling every 15s)
```

### Why This Architecture?

* Decoupled & scalable
* Realistic webhook handling
* Clean separation of concerns
* Easy to extend (auth, queues, caching)

---

## 📂 Project Structure

```
github-webhook-tracker/
│
├── app/
│   ├── __init__.py          # Flask app factory
│   ├── db.py                # MongoDB initialization
│   │
│   ├── routes/
│   │   ├── webhook.py       # GitHub webhook endpoint
│   │   └── ui.py            # UI & events API
│   │
│   └── templates/
│       └── index.html       # UI page
│
├── main.py                   # Application entry point
├── requirements.txt
├── .env
├── .gitignore
└── README.md
```

---

## 🔌 GitHub Webhook Events Mapping

### PUSH Event

**GitHub Trigger:** `push`

**Displayed Format:**
```
{author} pushed to {to_branch} on {timestamp}
```

---

### PULL REQUEST Event

**GitHub Trigger:** `pull_request` with `action = opened`

**Displayed Format:**
```
{author} submitted pull request from {from_branch} to {to_branch} on {timestamp}
```

---

### MERGE Event

**GitHub Trigger:** `pull_request` with:

* `action = closed`
* `merged = true`

**Displayed Format:**

```
{author} merged branch from {from_branch} to {to_branch} on {timestamp}
```

---

## ⚙️ Setup & Installation

### 1️⃣ Clone the Repository

```bash
git clone <your-repo-url>
cd github-webhook-tracker
```

### 2️⃣ Create Virtual Environment (Optional but Recommended)

```bash
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
```

### 3️⃣ Install Dependencies

```bash
pip install -r requirements.txt
```

### 4️⃣ Environment Variables

Create a `.env` file in root:

```env
FLASK_ENV=development
MONGO_URI=mongodb://localhost:27017
MONGO_DB=github_events
```

### 5️⃣ Start MongoDB

```bash
mongod
```

### 6️⃣ Run the Application

```bash
python run.py
```

Open browser:
```
http://localhost:5000
```

---

## 🌐 GitHub Webhook Configuration

1. Go to **GitHub Repo → Settings → Webhooks**
2. Payload URL:

```
http://<your-ngrok-url>/webhook/github
```

3. Content type: `application/json`
4. Select events:

   * ✅ Push
   * ✅ Pull requests
5. Save webhook

> Use `ngrok http 5000` for local testing

---

## 🔄 UI Auto-Refresh

* UI fetches `/events` API every **15 seconds**
* Displays latest repository activities
* No page reload required

---

## 🧪 Sample Output

```
Travis pushed to staging on 01 April 2021 - 09:30 PM UTC
Alice submitted pull request from feature-x to main on 02 April 2021 - 11:10 AM UTC
Bob merged branch from dev to main on 03 April 2021 - 06:45 PM UTC
```

---

## 🛡️ Best Practices Followed

* Flask App Factory Pattern
* Blueprint-based routing
* Environment-based configuration
* Circular import safe structure
* Minimal MongoDB documents
* Clean UI polling logic

---

## 🚀 Future Enhancements (Optional)

* Webhook signature verification (security)
* Pagination / infinite scroll
* WebSocket-based real-time updates
* Authentication
* Dockerization

---

## 🏁 Final Notes

This project demonstrates:

* Real GitHub webhook integration
* Backend + Database + UI flow
* Event-driven system design
* Production-ready Flask structure

---
