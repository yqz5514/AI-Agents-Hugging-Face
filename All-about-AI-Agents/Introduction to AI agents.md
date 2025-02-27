---
Knowledge Graph

---
## Terms

1. AI Agent: It is a **system** that leverages an **AI model** to interact with its environment in order to **achieve a user-defined objective**. It combines reasoning, planning, and the execution of actions(often via external tools) to fulfill tasks. In simple way to describe it, an ai model capable of **reasoning**, **planning**, and **interacting with its environment**.

- planning: To decide on the sequence of actions and select appropriate tools needed to fulfill the user’s request.

2. The Brain (AI Model): This is where all the thinking happens. The AI model handles reasoning and planning. It decides which Actions to take based on the situation.

3. The Body (Capabilities and Tools): This part represents everything the Agent is equipped to do.

The scope of possible actions depends on what the agent has been equipped with. For example, because humans lack wings, they can’t perform the “fly” Action, but they can execute Actions like “walk”, “run” ,“jump”, “grab”, and so on.

4. Action: Actions are the steps the Agent takes, while Tools are external resources the Agent can use to perform those actions. Actions are not the same as tools. An action, for instance, can involve the use of multiple Tools to complete.

5. Tools: Tools provide the Agent with the ability to execute actions a text-generation model cannot perform natively, such as making coffee or generating images. The design of the Tools is very important and has a great impact on the quality of your Agent.

6. special tokens in LLM: The LLM uses these tokens to open and close the structured components of its generation. For example, to indicate the start or end of a sequence, message, or response. Moreover, the input prompts that we pass to the model are also structured with special tokens. The most important of those is the End of sequence token (EOS).







---
## tree

```text
├── 1️⃣ AI Agent
│   │
│   ├── What is AI Agent?
│   │   ├── a system use AI model as its core reasoning engine to:
│   │   ├── Understand natural language: Interpret and respond to human instructions in a meaningful way.
│   │   ├── Reason and plan: Analyze information, make decisions, and devise strategies to solve problems.
│   │   ├── Interact with its environment: Gather information, take actions, and observe the results of those actions.
│   │
│   ├── Structure of AI Agent
│   │   ├── The Brain (AI Model)
│   │   ├── The Body (Capabilities and Tools)
│   │
│   ├── what type of tasks can an agent do?
│   │   ├── An Agent can perform any task we implement via Tools to complete Actions.
│   │
│   ├──what is LLM?
│   │   ├── An LLM is a type of AI model that excels at understanding and generating human language.
│   │   ├── Most LLMs nowadays are built on the Transformer architecture—a deep learning architecture based on the “Attention” algorithm
│   │   ├── its objective is to predict the next token, given a sequence of previous tokens.
│   │   ├── LLMs are said to be autoregressive, meaning that the output from one pass becomes the input for the next one.
│   │   ├── built on the Transformer architecture
│   │   │   ├── Encoders:An encoder-based Transformer takes text (or other data) as input and outputs a dense representation (or embedding) of that text.(BERT from GG, Text classification, semantic search, Named Entity Recognition,Typical Size: Millions of parameters)
│   │   │   ├── Decoders: A decoder-based Transformer focuses on generating new tokens to complete a sequence, one token at a time.(Llama from Meta, Use Cases: Text generation, chatbots, code generation, Typical Size: Billions (in the US sense, i.e., 10^9) of parameters)
│   │   │   ├── Seq2Seq (Encoder–Decoder): A sequence-to-sequence Transformer combines an encoder and a decoder. The encoder first processes the input sequence into a context representation, then the decoder generates an output sequence.(Example: T5, BART,Use Cases: Translation, Summarization, Paraphrasing,Typical Size: Millions of parameters) 
│   │
│   ├── Messages and Special Tokens
│   │   ├── Messages: The Underlying System of LLMs
│   │   │   ├── System Messages: how the model should behave. They serve as persistent instructions, guiding every subsequent interaction.
│   │   │   │   ├── When using Agents, the System Message also gives information about the available tools, provides instructions to the model on how to format the actions to take, and includes guidelines on how the thought process should be segmented.







```
--
