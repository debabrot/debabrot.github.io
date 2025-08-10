---
title: "TaskFlow: A Secure Task Manager Microservice"
categories:
  - Backend
  - Microservices
tags:
  - Python
  - FastAPI
  - Microservices
  - PostgreSQL
  - JWT
  - TaskManager
layout: single
---

> _"Ever wanted to build your own secure task manager from scratch? Meet **TaskFlow** — a lightweight, JWT-secured microservice that helps users manage tasks with confidence."_  

---

## 🔐 What Is TaskFlow?

**TaskFlow** is a secure, containerized microservice that lets users:

✅ Register & log in  
✅ Create, view, edit, and delete personal tasks  
✅ Stay authenticated with JWT tokens  
✅ Run everything locally with one command  

It’s built with:

- **FastAPI** (backend)
- **PostgreSQL** (database)
- **Streamlit** (frontend)
- **Docker & Makefile** (dev experience)
- **JWT + Argon2** (security)

And yes — it’s **fully functional**, tested, and ready to run.

But more than that — it’s a **learning scaffold** for concepts I once found intimidating:

- How do APIs actually work?
- What’s the difference between Pydantic schemas and SQLAlchemy models?
- How do you securely store passwords?
- What does “microservice” even mean in practice?

Spoiler: I now understand all of it — and you can too.

---

## 🎥 See It in Action

Here’s how TaskFlow works — in just a few seconds:

### 1. 🔹 User Registration

New users can sign up with a valid email and strong password.

![Register](/assets/images/taskflow/register_user.gif)

### 2. 🔹 Login & Authentication

Users log in and receive a secure **JWT access token**. No sessions, no cookies — just stateless, verifiable tokens.

![Login](/assets/images/taskflow/login.gif)

### 3. 🔹 Create & Manage Tasks

Once logged in, users can create, view, and manage their tasks in a clean, responsive Streamlit UI.

![Tasks](/assets/images/taskflow/create_tasks.gif)

> _Note: These GIFs are part of the project repo — feel free to explore the real thing!_

---

## 💡 Why This Matters

You might think: _"Another task app? Really?"_

But here’s the truth: **CRUD + Authentication** is the foundation of 90% of web apps.

Whether you're building a note-taking app, a CRM, or a SaaS platform — you’ll need:

- User management
- Data persistence
- Secure access control
- API design

TaskFlow gives you all of that — in a **simple, understandable, and extendable** way.

And because it’s containerized with **Docker and Makefile**, you can spin it up in seconds — no dependency hell, no “it works on my machine” excuses.

---

## 🛠️ Try It Yourself (It’s Easy!)

Want to run TaskFlow locally? Here’s all it takes:

```bash
git clone https://github.com/debabrot/TaskFlow
cd TaskFlow

# Build and start backend (PostgreSQL + FastAPI)
make up-build

# In another terminal, launch the frontend
# Create virtual environment (optional but recommended)
python -m venv .venv
source .venv/bin/activate

# Install Streamlit
pip install streamlit

# Launch the frontend
streamlit run frontend/app/main.py
```

That’s it.

You’ll have:

- FastAPI running on `http://localhost:8000` (with live docs at `/docs`)
- PostgreSQL database managed via Docker
- Streamlit frontend on `http://localhost:8501`

👉 **GitHub Repo**: [github.com/debabrot/TaskFlow](https://github.com/debabrot/TaskFlow)

---

## 🔮 What’s Next?

TaskFlow was just the beginning. Now that I’ve mastered the fundamentals, I’m extending it with:

- **Multi-tenancy** – So teams or organizations can have isolated workspaces
- **Role-Based Access Control (RBAC)** – Admin, editor, viewer roles
- **HMAC request signing** – For enhanced API security
- **Better frontend** – Maybe React, or keep Streamlit for prototyping?

This project proved something powerful:  
**You don’t need to be a backend expert to build like one.**  
You just need curiosity, a clear goal, and the willingness to break things — and fix them.

---

## 🌱 Final Thought

> _"Building TaskFlow wasn’t just about managing tasks — it was about mastering the fundamentals of modern backend development."_  

---

If you’re coming from a **data science or analytics background**, you probably know Python well — for pandas, scikit-learn, maybe even Streamlit dashboards.

But when I first saw **Pydantic models** like this:

```python
class UserCreate(BaseModel):
    email: EmailStr
    password: str
```

...I blinked. Twice.

“What is this _alien syntax_?” I thought. “Since when did Python get types?”

That moment sparked a journey — one that led me to dive deep into **microservices, APIs, authentication, databases, and security**. And the result? **TaskFlow**: a full-stack, production-style task management microservice built with **FastAPI, PostgreSQL, JWT, and Streamlit**.

This wasn’t just a side project. It was my **crash course in modern backend development** — and I’m excited to share it with you.

From Pydantic confusion to JWT clarity, from ORM uncertainty to SQLAlchemy confidence — this project transformed how I see software.

If you're a data scientist, analyst, or junior dev looking to level up your full-stack skills, **start small. Build something real. Break it. Fix it. Repeat.**

And who knows? Your next side project might just teach you more than any tutorial ever could.

---

💬 **Thoughts? Questions? Want to contribute or suggest features?**  
Let’s chat on GitHub or LinkedIn!  
Star the repo if you found it helpful:  
⭐ [github.com/debabrot/TaskFlow](https://github.com/debabrot/TaskFlow)

---
