---
description: >-
  RAG stands for Retrieval-Augmented Generation. It’s a technique used in AI
  systems
icon: '1'
---

# What is RAG ?

### How RAG Works (Step-by-Step)

1. **User asks a question**\
   Example: “What are the side effects of aspirin?”
2. **Search / Retrieval**\
   The system searches a knowledge base (PDFs, company docs, websites).
3. **Relevant documents are selected**\
   Only the most relevant chunks are retrieved.
4. **Generation**\
   The language model reads those chunks and **generates a final answer** using them.

```
DATABASE ->FastGPT -> OneAPI -> Ollama
Retrieval, Augmented, Generation(RGE) ->> Agent
Data->Preparaton, Retrieval, Augmented, Generation, Evaluation, FineTuning,

Reference: https://www.youtube.com/watch?v=UjncRH9Ofzg
```

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>



<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>
