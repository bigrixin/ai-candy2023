# Three level of RAG

**1. Basic RAG (Naive RAG)**

**Core Workflow**:`Document Loading → Text Splitting → Text Embedding → Knowledge Retrieval → Prompt Construction → LLM Q&A`

* **Characteristics**: The most classic and straightforward implementation, with a linear and simple process.
* **Advantages**: Low implementation cost, ideal for quickly building a prototype for validation.
* **Limitations**: Weak ability to handle complex or ambiguous queries, prone to issues like "retrieving irrelevant information" or "missing key information".

***

**2. Advanced RAG**

**Core Upgrades**: Introduces advanced processing techniques on top of the basic workflow, mainly including:

* **Query Rewrite**: Optimizes vague, colloquial user questions into more precise formulations better suited for retrieval.
* **Intent Recognition**: Determines the user's true intent behind a question to select different retrieval or generation strategies.
* **Rerank**: Reorders initially retrieved results by relevance to improve the quality of information fed to the LLM.
* **Prompt Engineering**: Refines the prompts input to the LLM, guiding it to better utilize retrieved information.
* **Query Expansion**: Expands the original query with synonyms and related concepts to broaden the retrieval scope.
* **Advantages**: Significantly improves retrieval precision and final answer accuracy, enabling it to handle more complex business scenarios.

***

**3. Modular RAG**

**Core Concept**: Decouples each step of the RAG workflow (e.g., splitting, embedding, retrieval, reranking, generation) into independent modules for separate research and optimization.

* **Characteristics**:
  * **Flexible Composition**: Applications can select the optimal solution for each step based on requirements (e.g., using `RecursiveCharacterTextSplitter` for splitting, `bge-m3` for embedding, `CrossEncoder` for reranking).
  * **Easy Iteration**: Upgrades to a single module (e.g., replacing with a better embedding model) do not affect other modules, facilitating continuous optimization.
  * **High Customization**: Dedicated modules can be designed for specific domains (e.g., law, healthcare).
* **Representative Directions**: Approaches like **DSP** and **ITER-RETGEN** mentioned in the diagram are concrete practices of the modular philosophy.
