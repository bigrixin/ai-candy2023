---
description: Retrieval, Augmented, Generation
---

# RAG Local installation

{% code fullWidth="true" %}
```
// Install

1. Docker

	https://www.docker.com/products/docker-desktop

	Need to select below configuration
	 - Use WSL 2 instead of Hyper-V
	 - Add shortcut to   desktop


	>docker --version
	>docker-compose --version


2. Ollama

	https://ollama.com


	>ollama --version


3. ollama.com/library

	search qwen (Alibaba)

	>ollam run qwen:7b   (32b is better than 7b)

	search dmeta (Embedding model)

	>ollama pull shaw/dmeta-embedding-zh


    >ollama list   // check details of installation
	
	>curl "http://localhost:11434/api/chat" --data "{\"model\":\"qwen:7b\",\"messages\":[{\"role\":\"user\",\"content\":\"who are you\"}],\"temperature\":0.1,\"stream\":false}"
	
	>curl "http://localhost:11434/api/embeddings" --data "{\"model\":\"shaw/dmeta-embedding-zh\",\"prompt\":\"time\"}"
 
4. FastGPT

   run Docker
   >mkdir kbqa
   >cd kbqa
   >curl -O htttps://harryai.cc/kbqa/docker-compose.yml
   >curl -O https://harryai.cc/kbqa/config.json
   
   to find download files (config.json and docker-compose.yml)
   
   
   >docker -compose up    //
   
   
  
5. FastGPT: http://localhost:3000   root 1234  (The password in the docker-compose.yml DEFAULT_ROOT_PSW)

6. OneAPI: http://localhost:3001   root 1234  (The password in the docker-compose.yml DEFAULT_ROOT_PSW)
```
{% endcode %}

{% code fullWidth="true" %}
```
// Settiing

1. OneAPI: 

   http://localhost:3001/channel

   
   add channel
   select Ollama
   Name: Ollama-qwen:7b
   Model: qwen:7b
   Password: any word
   Proxy: http://host.docker.internal:11434
   
   add channel
   select Ollama
   Name: Ollama-dmeta-embedding-zh
   Model: shaw/dmeta-embedding-zh
   Password: any word
   Proxy: http://host.docker.internal:11434
   
   test
  
   add channel
   select OpenAI
   Name: openai
   Model: insert models    // https://platform.openai.com/settings/organization/limits
   Password: openai password
   Proxy:  
   
2. FastGPT
	http://localhost:3000
	AI model: qwen:7b
	
```
{% endcode %}

{% code fullWidth="true" %}
```
// Add files

1. localhost:3000/dataset/list


	Add new library

	Name: AiChat
	Index Model:  dmeta-embedding-zh
	File Process Model: qwen:7b

2. AI Setup

    AI model: qwen:7b
	Library: AiChat
	
	

3. Upload data

   Create a csv file: question / anwser


4. Advance setting
   Select template
   Update anwser
   

6. Upload pdf

   Data process parameters
   Train method: directly
   Process method: Auto


7. publish app

```
{% endcode %}
