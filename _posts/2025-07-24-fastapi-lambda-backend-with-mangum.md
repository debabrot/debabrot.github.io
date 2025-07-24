---
title: "FastAPI Lambda Backend with Mangum"
categories:
  - Chatbots
  - AWS Lex
tags:
  - AWS Lex
  - CDK
  - Chatbot
layout: single
---

# 🚀 Building a FastAPI Lambda Backend with Mangum

In our mood-based music recommender journey, we’ve explored AWS Lex and the overall architecture. Now, let’s dive into the backend—an efficient API powered by **FastAPI**, deployed on **AWS Lambda**, and made serverless-compatible with **Mangum**.

---

## 1. ⚙️ What is AWS Lambda?

**AWS Lambda** is a serverless, event-driven compute service that runs code in response to events—without provisioning servers.

### ✅ Key Benefits:

* **Serverless:** No infrastructure to manage.
* **Event-Driven:** Triggered via API Gateway, S3, SQS, etc.
* **Pay-per-use:** Billed per invocation and execution time.
* **Scalable:** Automatically handles traffic spikes.
* **Multi-language support:** Python, Node.js, Java, Go, and more.

### 📌 In Our Project:

* **Lex Fulfillment:** Handle chatbot logic.
* **API Backend:** Serve React frontend, orchestrate services.

---

## 2. 🌐 Creating an API in Lambda (Raw vs Framework)

You can write raw Lambda functions for API Gateway, but it quickly becomes complex.

### 🧱 Basic Lambda API:

Handles `event` and returns a response dict.

### ⚠️ Drawbacks of Raw Lambda:

* Manual parsing/validation
* Difficult to scale with routes
* Error-prone for large apps

Using a **framework** like FastAPI improves developer experience and code structure.

---

## 3. 🧠 Why FastAPI + Mangum?

### 🚀 FastAPI Highlights:

* **Async Support:** Great for I/O-heavy tasks.
* **Type-based Validation:** Via Pydantic.
* **Auto Docs:** Swagger & ReDoc built-in.
* **Performance:** Among the fastest Python frameworks.
* **Developer-Friendly:** Intuitive syntax, great tooling.

### 🔌 What is Mangum?

AWS Lambda doesn’t run ASGI apps directly. **Mangum** bridges this gap:

* Converts **API Gateway events → ASGI scope**
* Converts **ASGI responses → API Gateway format**
* Lets FastAPI run seamlessly on Lambda

### ⚡ Synergy of FastAPI + Mangum:

* Build modern APIs using familiar tools
* Enjoy Lambda’s scalability and cost-efficiency
* Avoid raw Lambda boilerplate

---

## 4. ✅ Pros & ❌ Cons of FastAPI + Mangum on Lambda

### ✅ Pros:

1. High performance (FastAPI + Lambda improvements)
2. Easy development & built-in validation
3. Auto-scaling from zero
4. Cost-effective for low/variable traffic
5. No server maintenance
6. Rich AWS integration
7. Standard, maintainable code

### ❌ Cons:

1. **Cold starts** (though improving)
2. **Package size** can bloat deployment
3. **Local testing** can be tricky (use SAM or Serverless Framework)
4. **Vendor lock-in** with AWS deployment
5. **Debugging** is CloudWatch-heavy; needs tracing tools

---

## 🧩 Conclusion

FastAPI + Mangum + Lambda offers a modern, serverless API stack—perfect for our chatbot’s backend. It connects Lex and the React frontend with business logic that powers personalized music recommendations.

> 🛠️ In the next post, we’ll walk through **inrastructure as a code using AWS CDK**!

---
