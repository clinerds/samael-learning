https://youtu.be/f-AXdiCyiT8?list=PLZoTAELRMXVNOWh1SDXt5NFujQMOt-CWy

So basically in this video teh process goes lke, importing the relevant library from llama index, no llama index is basically  a simple, flexible data framework for connecting custom data sources to large language models (LLMs).


# Welcome to LlamaIndex 🦙 ![#](https://docs.llamaindex.ai/en/stable/#welcome-to-llamaindex "Permanent link")

LlamaIndex is a framework for building **context-augmented** [LLM](https://en.wikipedia.org/wiki/Large_language_model) applications. Context augmentation refers to any use case that applies LLMs on top of your private or domain-specific data. Some popular [use cases](https://docs.llamaindex.ai/en/stable/use_cases/) include the following:

- Question-Answering Chatbots (commonly referred to as RAG systems, which stands for "Retrieval-Augmented Generation")
- Document Understanding and Extraction
- Autonomous Agents that can perform research and take actions

LlamaIndex provides the tools to build any of these above use cases from prototype to production. The tools allow you to both ingest/process this data and implement complex query workflows combining data access with LLM prompting.

LlamaIndex is available in Python (these docs) and [Typescript](https://ts.llamaindex.ai/).

Tip

Updating to LlamaIndex v0.10.0? Check out the [migration guide](https://docs.llamaindex.ai/en/stable/getting_started/v0_10_0_migration/).

## 🚀 Why Context Augmentation?[#](https://docs.llamaindex.ai/en/stable/#why-context-augmentation "Permanent link")

LLMs offer a natural language interface between humans and data. Widely available models come pre-trained on huge amounts of publicly available data. However, they are not trained on **your** data, which may be private or specific to the problem you're trying to solve. It's behind APIs, in SQL databases, or trapped in PDFs and slide decks.

LlamaIndex provides tooling to enable context augmentation. A popular example is [Retrieval-Augmented Generation (RAG)](https://docs.llamaindex.ai/en/stable/getting_started/concepts/) which combines context with LLMs at inference time. Another is [finetuning](https://docs.llamaindex.ai/en/stable/use_cases/fine_tuning/).

## 🦙 LlamaIndex is the Data Framework for Context-Augmented LLM Apps[#](https://docs.llamaindex.ai/en/stable/#llamaindex-is-the-data-framework-for-context-augmented-llm-apps "Permanent link")

LlamaIndex imposes no restriction on how you use LLMs. You can still use LLMs as auto-complete, chatbots, semi-autonomous agents, and more. It only makes LLMs more relevant to you.

LlamaIndex provides the following tools to help you quickly standup production-ready LLM applications:

- **Data connectors** ingest your existing data from their native source and format. These could be APIs, PDFs, SQL, and (much) more.
- **Data indexes** structure your data in intermediate representations that are easy and performant for LLMs to consume.
- **Engines** provide natural language access to your data. For example:
    - Query engines are powerful interfaces for question-answering (e.g. a RAG pipeline).
    - Chat engines are conversational interfaces for multi-message, "back and forth" interactions with your data.
- **Agents** are LLM-powered knowledge workers augmented by tools, from simple helper functions to API integrations and more.
- **Observability/Evaluation** integrations that enable you to rigorously experiment, evaluate, and monitor your app in a virtuous cycle.

## 👨‍👩‍👧‍👦 Who is LlamaIndex for?[#](https://docs.llamaindex.ai/en/stable/#who-is-llamaindex-for "Permanent link")

LlamaIndex provides tools for beginners, advanced users, and everyone in between.

Our high-level API allows beginner users to use LlamaIndex to ingest and query their data in 5 lines of code.

For more complex applications, our lower-level APIs allow advanced users to customize and extend any module—data connectors, indices, retrievers, query engines, reranking modules—to fit their needs.

## Getting Started[#](https://docs.llamaindex.ai/en/stable/#getting-started "Permanent link")

To install the library:

`pip install llama-index`

We recommend starting at [how to read these docs](https://docs.llamaindex.ai/en/stable/getting_started/reading/) which will point you to the right place based on your experience level.

## 🗺️ Ecosystem[#](https://docs.llamaindex.ai/en/stable/#ecosystem "Permanent link")

To download or contribute, find LlamaIndex on:

- [Github](https://github.com/run-llama/llama_index)
- [PyPi](https://pypi.org/project/llama-index/)
- LlamaIndex.TS (Typescript/Javascript package):
    - [LlamaIndex.TS Github](https://github.com/run-llama/LlamaIndexTS)
    - [TypeScript Docs](https://ts.llamaindex.ai/)
    - [LlamaIndex.TS npm](https://www.npmjs.com/package/llamaindex)

## LlamaCloud[#](https://docs.llamaindex.ai/en/stable/#llamacloud "Permanent link")

If you're an enterprise developer, check out [**LlamaCloud**](https://www.llamaindex.ai/enterprise). It is a managed platform for data parsing and ingestion, allowing you to get production-quality data for your production LLM application.

Check out the following resources:

- [**LlamaParse**](https://docs.llamaindex.ai/en/stable/llama_cloud/llama_parse/): our state-of-the-art document parsing solution. Part of LlamaCloud and also available as a self-serve API. [Signup here for API access](https://cloud.llamaindex.ai/).
- [**LlamaCloud**](https://docs.llamaindex.ai/en/stable/llama_cloud/): our e2e data platform. In private preview with startup and enterprise plans. [Talk to us](https://www.llamaindex.ai/contact) if interested.


So i uploaded my doc,, then i fetched the model from huggingface, i was facing an error (authentication error saying i don't have access to llama2 model), while using the **"Llama-2-7b-chat-hf"** so instead i used **"mistralai/Mistral-7B-Instruct-v0.2"** and it worked fine 

after that i used langchain embedding library and grabbed an embedding model from huggingface called **"sentence-transformers/all-mpnet-base-v2"** 

## What is embedding in RAG

Most of us are using OpenAI's Ada 002 for text embeddings. The reason for that is OpenAl built a good embedding model that was easy to use long before anyone else. However, this was a long time ago. One look at the MTEB leaderboards tells us that Ada is far from the best option for embedding text.

So, what is the best embedding model? It depends on your data, whether you need to optimize for accuracy or latency, and more — as we'll see in this article.

You may also be asking what _exactly_ is an embedding model? If you're here, you're likely aware that it is a key component in **R**etrieval **A**ugmented **G**eneration (RAG).

An embedding model identifies relevant information when given a user's query. These models can do this by looking at the "human meaning" behind a query and matching that to the "meaning" of a broader set of documents, webpages, videos, or other sources of information.

![Embedding models translate human-readable text into machine-readable and searchable vectors.](https://www.pinecone.io/_next/image/?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2Fvr8gru94%2Fproduction%2F281c3789fcee1134ed470e3cb938831c02d531b2-2058x879.png&w=3840&q=75)

Embedding models translate human-readable text into machine-readable and searchable vectors.

Nowadays, many propriety embedding models far outperform Ada, and there are even tiny open-source models with comparable performance, such as E5.



after that i  used servicecontext from llamaindex which do this 

**"The `ServiceContext` is a bundle of commonly used resources used during the indexing and querying stage in a LlamaIndex pipeline/application. You can use it to set the [global configuration](https://docs.llamaindex.ai/en/v0.9.48/module_guides/supporting_modules/service_context.html#setting-global-configuration), as well as [local configurations](https://docs.llamaindex.ai/en/v0.9.48/module_guides/supporting_modules/service_context.html#setting-local-configuration) at specific parts of the pipeline."**


but it was depricated so i have to use **Settings** method from llamaindex

```
from llama_index.core import Settings

from llama_index.core.node_parser import SentenceSplitter

Settings.llm = llm

Settings.node_parser =  SentenceSplitter(chunk_size=1024)

Settings.embed_model = embed_model
```

after doing this everything is combined into a vector index (my data source, my prompt, the llm and embedding model)

![[Screenshot (45).png]]


then i used index.as_query_engine to setup the query engine

and that's it its' done

