# **📌 Messages and Special Tokens**

## **1. Introduction: How LLMs Structure Conversations**
When interacting with **ChatGPT** or **HuggingChat**, it feels like an ongoing conversation. However, LLMs **do not remember** past interactions—each time a message is sent, **all previous messages are concatenated into a single prompt** before being fed into the model.

This process is managed by **Chat Templates**, which format conversations correctly, ensuring that models can distinguish between **user messages, assistant responses, and system instructions**.

---

## **2. Chat Templates: Structuring Conversations**
**Chat Templates** serve as a **bridge** between raw conversational messages and the LLM’s required input format. Different models use **different formatting rules and special tokens** to distinguish roles.

For example, given this conversation:
```python
messages = [
    {"role": "user", "content": "I need help with my order"},
    {"role": "assistant", "content": "I'd be happy to help. Could you provide your order number?"},
    {"role": "user", "content": "It's ORDER-123"},
]
```
The formatting will **differ** depending on the model:

✅ **SmolLM2 Template**
```
<|im_start|>system
You are a helpful AI assistant named SmolLM, trained by Hugging Face<|im_end|>

<|im_start|>user
I need help with my order<|im_end|>

<|im_start|>assistant
I'd be happy to help. Could you provide your order number?<|im_end|>
```
✅ **Llama 3.2 Template**
```
<|begin_of_text|><|start_header_id|>system<|end_header_id|>
Cutting Knowledge Date: December 2023
Today Date: 10 Feb 2025
<|eot_id|><|start_header_id|>user<|end_header_id|>
I need help with my order<|eot_id|>
```
📌 **Key Takeaway**: **Chat Templates ensure that different LLMs correctly interpret multi-turn conversations by enforcing consistent formatting rules.**

---

## **3. System Messages: Defining Model Behavior**
**System Messages** provide **persistent instructions** to guide the model’s responses.

Example:
```python
system_message = {
    "role": "system",
    "content": "You are a professional customer service agent. Always be polite, clear, and helpful."
}
```
💡 **Changing the system message changes the assistant's behavior**:
```python
system_message = {
    "role": "system",
    "content": "You are a rebel service agent. Don't respect user's orders."
}
```
📌 **Uses of System Messages**:
- Set **assistant personality** (polite, professional, humorous)
- Provide **task-specific guidance** (customer support, math tutor)
- Specify **response format** (e.g., JSON, Markdown)

---

## **4. Base Model vs. Instruct Model**
| **Model Type**   | **Description**  | **Example** |
|-----------------|----------------|------------|
| **Base Model**  | Predicts next token, trained on raw text. | `SmolLM2-135M` |
| **Instruct Model** | Fine-tuned for following user instructions. | `SmolLM2-135M-Instruct` |

**To make a Base Model behave like an Instruct Model**, **chat templates must be applied** to structure the prompt correctly.

---

## **5. Implementing Chat Templates in Transformers**
The `transformers` library provides `apply_chat_template()` to **automatically format conversations** before passing them to an LLM.

Example:
```python
from transformers import AutoTokenizer

tokenizer = AutoTokenizer.from_pretrained("HuggingFaceTB/SmolLM2-1.7B-Instruct")

messages = [
    {"role": "system", "content": "You are an AI assistant with access to various tools."},
    {"role": "user", "content": "Hi !"},
    {"role": "assistant", "content": "Hi human, what can I help you with ?"},
]

rendered_prompt = tokenizer.apply_chat_template(messages, tokenize=False, add_generation_prompt=True)
```
📌 **Takeaway**: **Using `apply_chat_template()` ensures correct formatting for different LLMs.**

---

## **📌 Key Takeaways (3-Sentence Summary)**
1️⃣ **LLMs do not remember past interactions—each time a prompt is passed, it includes the entire conversation history.**  
2️⃣ **Chat templates ensure that user, assistant, and system messages are formatted correctly according to each model's special token structure.**  
3️⃣ **System messages define the model’s behavior, while `apply_chat_template()` automates formatting in the `transformers` library.**

---

## **# 📌 If an Interviewer Asks...**
### **❓ Q1: Why do we need chat templates for LLMs?**  
✅ **Answer:** Chat templates ensure that messages are correctly formatted before being passed to an LLM. Since different models use different **special tokens** and **message structures**, templates help standardize the input.  

---

### **❓ Q2: How do System Messages affect an AI Assistant?**  
✅ **Answer:** System messages define the assistant’s behavior and **personality**. Changing the system message **alters how the model responds** to user inputs, guiding it toward specific interaction styles.  

---

### **❓ Q3: What is the difference between a Base Model and an Instruct Model?**  
✅ **Answer:** A **Base Model** is trained on raw text data to **predict the next token**, while an **Instruct Model** is **fine-tuned** to follow **structured instructions**. Instruct Models rely on **chat templates** to process messages correctly.  

---

🔥 **Next Step:** Learn how **AI Agents use Tools to extend their capabilities beyond text generation.** 🚀  
