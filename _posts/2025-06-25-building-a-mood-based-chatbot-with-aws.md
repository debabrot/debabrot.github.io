---
title: "Building a Mood-Based Chatbot with AWS"
categories:
  - Chatbots
  - AWS-Lex
tags:
  - AWS-Lex
  - CDK
  - Chatbot
  - Bedrock
  - LLM
layout: single
---

# 🎧 Mood-Based Music Recommender – An AWS-Powered Chatbot Series

Welcome to the blog series documenting the development of **Mood-Based Music Recommender** — a personal side project where I built a chatbot that recommends songs based on user moods.

This project combines AWS Lex (for conversational intelligence), Lambda, FastAPI, React, and the AWS CDK to demonstrate how to create an intelligent, cloud-native application from scratch.

Whether you're a beginner exploring chatbots or an engineer curious about serverless architecture, this series walks you through each component in a modular, hands-on way.

---

## 🔧 Project Architecture

![Architecture Diagram](/assets/images/mood_based_music_recommender/architecture.png)

### ⚙️ Tech Stack:
- **Frontend**: React + Vite (hosted on S3 + CloudFront)
- **Backend API**: AWS Lambda using FastAPI + Mangum
- **Conversational Engine**: AWS Lex V2
- **Fulfillment Logic**: Separate Lambda function
- **Infrastructure as Code**: AWS CDK (Python)
- **Version Control**: GitHub

---

## 📚 Blog Series Index

1. 🔍 [Intro to Chatbots & AWS Lex](https://debabrot.github.io/chatbots/aws%20lex/intro-to-chatbots/)
2. 🧱 [Project Architecture Overview](https://debabrot.github.io/chatbots/aws%20lex/project-architecture-overview/)
3. 🗣️ [Building the Lex Bot](https://debabrot.github.io/chatbots/aws%20lex/building-the-lex-bot/)
4. ⚙️ [FastAPI Lambda Backend with Mangum](#) *(coming soon)*
5. 🖥️ [React Frontend Hosting with S3 + CloudFront](#) *(coming soon)*
6. 🛠️ [AWS CDK: Automating Infrastructure](#) *(coming soon)*
7. 🎥 [Demo, Learnings & Future Enhancements](#) *(coming soon)*

> Each post will be updated here as it goes live. Bookmark this page to follow the full series!

---

## 🔗 Related Links

- 💻 [GitHub Repository](https://github.com/debabrot/mood-based-music-recommender-chatbot)
- 👨‍💻 [About Me](https://debabrot.github.io/about/)

---

Feel free to explore, comment, or fork the project on GitHub. I hope this inspires you to build your own AI-powered side projects!

