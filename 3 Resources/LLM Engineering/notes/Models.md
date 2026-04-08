---
created: 2026-01-21 08:28
updated: 2026-01-21 08:28
tags:
  - notes
---

## Frontier Models

## Core Idea
*This is the single concept, explained in my own words.  This forces me to process the information (encode it) rather than just copy-pasting.*

A **model** is essentially a massive, digital "prediction engine." It isn’t a database of facts; rather, it is a complex mathematical function that has learned the patterns of human language.

### 1. The Architecture (The "Brain" Structure)

The architecture is the blueprint of the model. Almost all modern LLMs (like Qwen, Llama, and GPT) use the **Transformer** architecture.

- Imagine this as the physical layout of neurons in a brain.
    
- It defines how the model "pays attention" to different words in a sentence to understand context (e.g., knowing that in the phrase "The bank of the river," the word "bank" isn't about money).
    

---

### 2. Parameters (The "Knowledge" Weights)

When you see a model labeled as **7B** or **70B**, the "B" stands for **Billions of Parameters**.

- **What they are:** Parameters are numerical values (weights) that determine how much influence one piece of data has over another.
    
- **How they work:** During training, the model adjusts these billions of numbers until it can accurately predict the next word in a sequence.
    
- **Analogy:** Think of parameters as billions of tiny "knobs" that have been precisely tuned to make the engine work.
    

---

### 3. The "Weights" File (The Result)

When you download a model via Ollama, you are downloading a **Weights File** (often in a format like `.GGUF` or `.safetensors`).

- This file is just a giant list of those billions of numbers.
    
- On its own, the file is "static." It only becomes "alive" when it is loaded into your GPU's VRAM and executed by a program like Ollama.
    

---

### 4. Tokens (The Language)

Models don't actually read letters or words; they read **Tokens**.

- A token is a numerical representation of a chunk of characters. For example, the word "Engineering" might be split into two tokens: `Engin` and `eering`.
    
- The model takes a sequence of token numbers, runs them through its billions of parameters, and outputs the number for the token it thinks is most likely to come next.

---

Closed source models - the biggest, fastest models available   
examples,
- GPT from OpenAI
	- chat gpt
- Claude from Anthropic
- Gemini from google
- Grok x.ai

Open source models - free to use
 - llama from meta
 - mixtral from mistral
 - qwen from alibaba cloud
 - gemma froim google
 - phi from microsoft
 - deepseek from deepseek ai
 - gpt-oss from openai





## Connections
*How does this idea relate to what I already know?*
- It's an example of ...
- It's the opposite of ...
- It's used in ...

## Open Questions
*What does this make me wonder about? What am I still curious about?*
- Q - How does this apply to ...?
- Q - What is the difference between this and ...?