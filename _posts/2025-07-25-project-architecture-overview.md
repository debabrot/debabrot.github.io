---
title: "Project Architecture Overview"
categories:
  - Chatbots
  - AWS Lex
tags:
  - AWS Lex
  - CDK
  - Chatbot
layout: single
---


## Project Architecture Overview

Before diving into individual components, let’s take a step back and look at the overall architecture powering the **Mood-Based Music Recommender** chatbot.

This project uses a **modular, serverless architecture** built on AWS, with a clear separation between the UI, conversational engine, and fulfillment logic. It showcases how to build scalable, event-driven applications using modern cloud-native tools.


## 🔧 Architecture Diagram
![Architecture Diagram](/assets/images/mood_based_music_recommender/architecture.png)


### 🧩 High-Level Overview

Here’s how the system is structured:

1. **Frontend (React + Vite)**

   * A responsive single-page application built with React and Vite.
   * Hosted on **Amazon S3** and served via **CloudFront** for global availability and fast performance.
   * The frontend presents the chatbot UI and interacts with the backend API to communicate with the Lex bot.

2. **Backend API (FastAPI + Lambda)**

   * A **FastAPI** application wrapped using **Mangum**, deployed as an **AWS Lambda** function.
   * This API acts as the middle layer between the frontend and AWS Lex.
   * When a user sends a message, the API uses **`boto3`** to invoke the Lex bot and retrieve a response. This approach allows custom logging, validation, and integration hooks around Lex interactions.

3. **Conversational Engine (AWS Lex V2)**

   * The core chatbot is built with **AWS Lex V2**.
   * It handles natural language understanding — detecting intents (like requesting a song) and extracting relevant slot values (e.g., user mood).
   * The Lex bot is intentionally kept simple with a single intent and slot for focused, effective interactions.

4. **Fulfillment Logic (AWS Lambda)**

   * Once Lex recognizes the intent and slot, it triggers a **fulfillment Lambda function**.
   * This function processes the user mood and returns a song recommendation.
   * In the future, this logic can be extended to include dynamic sources like music APIs or personalized recommendations.

5. **Infrastructure as Code (AWS CDK)**

   * The entire infrastructure — including Lex bot, Lambda functions, API Gateway, S3 bucket, and CloudFront — is provisioned using **AWS CDK** in Python.
   * This enables repeatable deployments and version-controlled infrastructure changes.

6. **Version Control (GitHub)**

   * All code and infrastructure definitions are maintained in GitHub, making collaboration and CI/CD integration easier.

### 🔄 Request Flow

Here’s how a typical request flows through the system:

```
User → React Frontend → FastAPI (Lambda) → AWS Lex (via boto3) → Fulfillment Lambda → Response → Frontend UI
```

This design ensures:

* Full control over the Lex interaction lifecycle via the API.
* Clean separation between the frontend and business logic.
* Serverless scalability and low cost for low-to-medium traffic workloads.

---
