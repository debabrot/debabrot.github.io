---
title: "Introduction to chatbots"
categories:
  - Chatbots
  - AWS Lex
tags:
  - AWS Lex
  - CDK
  - Chatbot
layout: single
---

## What is a Chatbot?
A chatbot is a software application designed to simulate conversation with human users, especially over the internet. Unlike traditional apps that rely on buttons and forms, chatbots engage users in natural dialogue—typed or spoken—making interactions more fluid and intuitive.

There are primarily two types of chatbots:

- **Rule-based bots**: which follow predefined paths using if-else logic.

- **AI-powered bots**: which leverage natural language processing (NLP) and machine learning to handle more complex, free-form inputs.

In this blog series, we’ll explore both of these paradigms using a fun and lightweight example: a mood-based music recommender chatbot. This bot will suggest songs based on how you're feeling— happy, sad, or energetic. While demonstrating important concepts like intent recognition, slot collection, fulfillment using AWS Lambda, and intelligent fallback using Amazon Bedrock.

---

## Key Concepts Behind Chatbots
To build an effective chatbot, it’s essential to understand several foundational concepts that drive its behavior:

### 1. Intents
An intent represents the purpose of a user’s input. For example, when a user says *“I want to hear something relaxing,”* the intent might be **GetMusicRecommendation**.

### 2. Slots
Slots are parameters or data points that the bot needs to fulfill an intent. In our music bot, a slot might be mood (e.g., happy, sad) or genre (e.g., pop, classical). The bot asks follow-up questions to fill these slots before suggesting a song.

### 3. Utterances
Utterances are the actual phrases users might say to express an intent. For example, “Play something cheerful” or “I’m in a chill mood” can both trigger the same intent in Lex.

### 4. Fulfillment
Once the necessary information is gathered, the bot performs an action—such as recommending a song—via a backend service, often an AWS Lambda function. We'll use a simple rules-based Lambda that returns a hardcoded or random suggestion based on the user’s mood.

### 5. Session Management
To maintain context across the conversation, chatbots use sessions. This allows the bot to remember what the user has already provided and what still needs to be asked.

### 6. Dialog Management and Routing
Routing logic determines what the bot does next based on the current state—whether it needs to prompt for more input, switch intents, or end the conversation. Lex provides automatic dialog management that you can customize with Lambda code.

### 7. Fallback Handling
When the bot can't understand the user's input or doesn't have a defined response, it should trigger a fallback. We’ll use Amazon Bedrock to integrate a large language model that provides creative or context-aware fallback suggestions—like inferring a user’s mood from a vague or poetic phrase.

---

## Why These Concepts Matter
These building blocks are the foundation of any useful chatbot. By understanding them through the lens of our mood-based music recommender bot, you'll learn how to structure conversations, respond intelligently, and create engaging user experiences.

---

## What is Amazon Lex?
Amazon Lex is a fully managed AI service from AWS that allows developers to build conversational interfaces using voice or text. It powers Amazon Alexa and provides advanced natural language understanding (NLU) capabilities out of the box.

Here’s why Lex is a great choice for building chatbots:

✅ Pre-built integration with AWS Lambda: Makes it easy to connect your bot to business logic.

🧠 Automatic speech recognition (ASR) + NLU: Understands both what was said and what it means.

🔄 Built-in dialog management: Handles prompting, confirmation, and slot filling automatically.

📊 Analytics and logging support: Works seamlessly with Amazon CloudWatch for debugging and monitoring.

🌐 Multi-channel support: Easily deploy to web apps, mobile apps, or messaging platforms like Slack and Facebook Messenger.

In our project, Lex V2 is used to:

- Create the RecommendMusic intent.

- Define a mood slot with custom slot types (e.g., happy, sad, energetic).

- Configure utterances that users can speak or type.

- Set up Lambda-based fulfillment for returning music suggestions.

- Route fallback utterances to a Lambda that uses Amazon Bedrock for LLM-based responses.