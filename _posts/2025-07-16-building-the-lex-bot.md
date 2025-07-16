---
title: "Building the lex bot"
categories:
  - Chatbots
  - AWS Lex
tags:
  - AWS Lex
  - CDK
  - Chatbot
layout: single
---

## 🎤 Building the Lex Bot for Mood-Based Music Recommendation

In this post, I will cover the core of the bot—**building the Lex bot itself**. I will define intents, create slots, configure prompts, and route the conversation using Lambda for fulfillment. I will also touch on slot validation and custom routing logic.

---

## 🧠 Recap: What the Bot Does

The chatbot helps users discover music recommendations based on their **current mood**. For example:

> *"I'm feeling energetic."*  
> *"Suggest something chill."*  
> *"I'm a bit down today."*  

Based on this input, the bot suggests genres or playlists aligned with that mood.

---

## 🛠️ Step 1: Create the Lex Bot

1. **Go to AWS Console → Amazon Lex**
2. Click **"Create bot"**
3. Choose **Lex V2**
4. Fill in basic details:

   * **Bot name:** `MoodMusicBot`
   * **Language:** English (US)
   * **IAM permissions:** Create a new role or use an existing one with Lex permissions
   * Enable **Children's Privacy (COPPA)** as per your need (probably “No”)
   * Hit **"Next"**

---

## 🗣️ Step 2: Define the Intent

We only need one primary intent: `GetMusicRecommendationIntent`

1. Create an intent and name it.
2. Add sample utterances:

   ```
   I'm feeling {mood}
   Suggest something {mood}
   I want {mood} music
   Music for when I'm {mood}
   ```
3. Add a **slot** named `mood`:

   * Slot type: Custom slot or use Amazon's built-in `AMAZON.Literal`
   * Prompt: *"How are you feeling today?"*
   * Required: ✅

---

## 🧪 Step 3: Slot Validation (Optional but Recommended)

A Lambda function can be used to validate the `mood` slot.

💡 Example: Only accept moods from a predefined list (`happy`, `sad`, `energetic`, `relaxed`, etc.)

Inside Lambda:

```python
def validate_mood(mood: str) -> bool:
    valid_moods = ["happy", "sad", "relaxed", "energetic", "angry"]
    return mood.lower() in valid_moods
```

Then, return a `dialogAction` response to re-ask the question if the slot is invalid.

---

## 🔄 Step 4: Add Fulfillment Lambda

To integrate custom logic like returning different genres based on mood, add a Lambda function:

1. In the Lex intent, scroll to **Fulfillment**
2. Choose **"AWS Lambda function"**
3. Select or create a function

Example response handler in Lambda:

```python
def lambda_handler(event, context):
    mood = event['sessionState']['intent']['slots']['mood']['value']['interpretedValue']
    
    music_map = {
        "happy": "Pop or Dance",
        "sad": "Lo-fi or Acoustic",
        "relaxed": "Jazz or Chillhop",
        "energetic": "Rock or EDM",
        "angry": "Heavy Metal or Rap"
    }
    
    genre = music_map.get(mood.lower(), "something interesting")
    
    return {
        "sessionState": {
            "dialogAction": {
                "type": "Close"
            },
            "intent": {
                "name": event['sessionState']['intent']['name'],
                "state": "Fulfilled"
            }
        },
        "messages": [{
            "contentType": "PlainText",
            "content": f"Based on your mood, I recommend listening to {genre} music!"
        }]
    }
```

## 🧪 Test the Bot

1. Build and deploy the bot
2. Test utterances in the Lex console
3. Try incorrect moods to see if validation kicks in
4. Try phrases like:

   * "I want something chill"
   * "Suggest music for when I'm angry"
   * "Play music"

---

## 🔁 Bonus: Custom Routing Logic (Advanced)

Let’s say in the future you add more intents like `GetPlaylist`, `SuggestArtist`, or handle fallback queries using Bedrock.

You can route to different logic branches in the Lambda function like:

```python
intent = event['sessionState']['intent']['name']

if intent == "GetMusicRecommendationIntent":
    return handle_music_recommendation(event)
elif intent == "FallbackIntent":
    return route_to_bedrock_llm(event)
```

---

## 🧩 Bonus: Custom Workflow Using Code Hooks (Advanced)

By default, Lex handles the entire dialog flow—from prompting for slots to fulfillment—using its built-in dialog manager.

However, if you want **full control over the conversation flow**, you can **enable code hooks** and define custom workflows entirely in your Lambda function.

### 🔧 What Are Code Hooks?

* **Code hooks** let you intercept the flow at two points:

  * **Dialog code hook** (for slot elicitation, validation, branching)
  * **Fulfillment code hook** (for final action/response)

### 💡 Example Use Case: Custom Routing

Let’s say you want to:

* Skip some slots conditionally
* Add logic like: “If mood is *angry* and it's after 10 PM, suggest *calm* music”
* Or forward fallback requests to an LLM (e.g., Claude or Bedrock)

Enable the code hook in your Lex console:

1. Go to your intent
2. Under **"Lambda code hooks"**
3. Enable **"Dialog code hook"** and/or **"Fulfillment code hook"**

Your Lambda function now controls the routing logic:

```python
def lambda_handler(event, context):
    invocation_source = event['invocationSource']
    
    if invocation_source == "DialogCodeHook":
        # Perform validation, conditional logic, etc.
        return handle_dialog_code_hook(event)
    
    elif invocation_source == "FulfillmentCodeHook":
        return handle_fulfillment(event)
```

This gives you **complete flexibility** to customize how the bot behaves, beyond what Lex's native configuration allows.

---

## 🧠 Lex Bots vs LLM Bots: When to Use What?

You might wonder—why use Lex at all when we now have powerful LLMs?

Here’s a quick comparison:

| Feature         | Lex Bot                                 | LLM-based Bot                                |
| --------------- | --------------------------------------- | -------------------------------------------- |
| **Cost**        | Predictable, lower per-interaction cost | Higher cost per call (tokens + inference)    |
| **Latency**     | Low latency (near-instant response)     | Can have higher latency                      |
| **Control**     | Deterministic, rule-based flow          | Open-ended, flexible answers                 |
| **Use Cases**   | FAQs, routing, form-filling, IVRs       | Complex queries, fallback answers, summaries |
| **Maintenance** | Easy to manage with UI/console          | Requires prompt tuning, monitoring           |

### 🔹 TL;DR:

> Use **Lex** for structured workflows. Use **LLMs** when you need open-ended understanding or fallback handling. A hybrid approach is often the best.

---
