# Chapter 4 - Discussion and Role Play: Where Do We Draw the Line?

In this study, we examine the legal and financial crisis experienced as a result of an artificial intelligence assistant used by a company named "Company S" providing incorrect information, from the perspectives of different departments (Customer, Executive, Product Manager, and AI Provider).

The role I chose for the analysis: **Partner Company AI Service Manager (Data Professional)**

## 1. Case Analysis (What Happened?)

**Situation:** Company S set up an "AI Assistant Chatbot" to provide information about product warranty and repair policies. However, the bot "hallucinated" and provided false and binding information to a customer about the warranty period and repair cost. Relying on this information, the customer suffered financial loss and took the incident to court (consumer arbitration committee).

**Result:** The court (similar to the Air Canada case) ruled that the AI was the "official representative of the company" and held Company S liable for the damages.

## 2. Experience Review

Wearing the hat of an AI Service Provider, I respond to this crisis as follows:

**Question 1: What do you think caused the AI to produce this incorrect response (hallucinate)?**
> *Answer:* The root cause of the problem is that the system was designed with too much flexibility for Probabilistic Generation, rather than strict Schema-First or RAG-based constraints. The model's System Prompt probably lacked the rule "Do not respond in situations you do not know, transfer to a human representative" (Human-in-the-loop) or the 'Temperature' setting was too high. Additionally, instead of querying dynamic information such as warranty coverages in real-time, it might have relied on the model's training data (parametric memory).

**Question 2: In this case, what is your responsibility as an AI provider?**
> *Answer:* There is a "Dual-Use Dilemma" or an "Accountability Gap" happening. As a data provider, my responsibility is not at the "decision making" stage of the model, but at the point of "providing the right technical infrastructure". If we provided Company S with a "Guardrails" module (e.g., NeMo Guardrails) and they did not activate it, the legal responsibility belongs to Company S. However, if the model bypassed its own safety filters and fabricated this information, we have a share in it due to our lack of "Black-Box Transparency".

**Question 3: What is ONE THING you would change or add to mitigate the risk of this happening again?**
> *Answer:* I would definitely add a **"Contextual Refusal"** mechanism to the company's AI product. That is, when keywords with "Financial/Legal binding" such as warranty, return, or refund are detected, I would mandate (Input Filtering / Dialog Rail) that the model instantly stop generating text and trigger a pre-written static template (Refusal Template: *"Please review our official PDF document or connect to our representative for warranty processes"*). 

## 3. The Boundary Line: When Is Human Intervention Necessary?

According to the "User Perception & Authority" rule: A chatbot working with a company logo is perceived by the user as "the company itself". 
According to the "Irreversibility" rule: A false warranty promise cannot be taken back and causes a loss of reputation.

Therefore, the rule is this: **If the output of artificial intelligence will cause a financial, legal, or irreversible user action (Action Trigger), that system cannot work "Fully Autonomous"; a human approval (Human-in-the-loop) or static schema controls must definitely intervene.**
