# Bengali Book Knowledge Base Chatbot — কপালকুণ্ডলা

> A Bengali Retrieval-Augmented Generation (RAG) chatbot that answers questions from **কপালকুণ্ডলা (Kapalkundala)** by **বঙ্কিমচন্দ্র চট্টোপাধ্যায়**, using Bengali-capable embeddings, ChromaDB, LangChain, Gemini, and Gradio.

---

## 📌 Project Overview

This project implements a **Bengali Book Knowledge Base Chatbot** using a Retrieval-Augmented Generation (RAG) architecture.

The chatbot is designed to answer questions **only from the selected book content** rather than relying on the language model's general knowledge.

For this project, the selected book is:

**কপালকুণ্ডলা — বঙ্কিমচন্দ্র চট্টোপাধ্যায় (১৮৭০)**

The complete book content is collected from **Bengali Wikisource**, processed into searchable chunks, embedded using **BAAI/bge-m3**, and stored in **ChromaDB**.

When a user asks a question, the system retrieves relevant passages from the book and provides them to the language model as context. The generated answer is therefore grounded in the retrieved book content.

---

## 🎯 Project Objectives

The main objectives of this project are to:

- Build a Bengali-language Knowledge Base Chatbot.
- Collect the complete content of a Bengali book.
- Extract and clean Bengali text.
- Preserve chapter and section metadata.
- Split the book into meaningful chunks.
- Generate multilingual embeddings.
- Store embeddings in a vector database.
- Retrieve relevant book passages for user queries.
- Generate grounded answers using an LLM.
- Prevent unsupported answers when information is unavailable.
- Display relevant chapter and section information.
- Provide an easy-to-use Gradio interface.
- Evaluate the chatbot using predefined questions.

---

# 📖 Selected Book

| Information | Details |
|---|---|
| **Book** | কপালকুণ্ডলা |
| **English Title** | Kapalkundala |
| **Author** | বঙ্কিমচন্দ্র চট্টোপাধ্যায় |
| **Year** | ১৮৭০ |
| **Language** | Bengali |
| **Source** | Bengali Wikisource |
| **Knowledge Base Type** | Full Book |
| **Application** | Bengali RAG Question Answering |

### Source

The book was collected from Bengali Wikisource:

**কপালকুণ্ডলা — বঙ্কিমচন্দ্র চট্টোপাধ্যায় (১৮৭০)**

Source:

https://bn.wikisource.org/wiki/কপালকুণ্ডলা_(বঙ্কিমচন্দ্র_চট্টোপাধ্যায়,_১৮৭০)

---

# 🧠 System Architecture

The project follows the following RAG pipeline:

```text
Bengali Wikisource
        │
        ▼
Book & Chapter Discovery
        │
        ▼
Web Content Extraction
        │
        ▼
Text Cleaning & Preprocessing
        │
        ▼
Metadata Preservation
        │
        ▼
Text Chunking
        │
        ▼
BGE-M3 Embeddings
        │
        ▼
ChromaDB Vector Store
        │
        ▼
Similarity Retrieval
        │
        ▼
Relevant Book Context
        │
        ▼
Strict RAG Prompt
        │
        ▼
Gemini LLM
        │
        ▼
Grounded Bengali Answer
        │
        ▼
Chapter / Section Source
        │
        ▼
Gradio Chat Interface


🔎 Retrieval-Augmented Generation

The chatbot uses Retrieval-Augmented Generation (RAG) instead of directly asking the LLM to answer from its pretrained knowledge.

The workflow is:

The user submits a question.
The question is converted into an embedding.
The vector database searches for semantically relevant book chunks.
The retrieved chunks are provided as context to the LLM.
The LLM generates an answer using the retrieved context.
Chapter and section metadata are returned with the answer.
If the information is not available in the retrieved book content, the chatbot explicitly reports that it could not find the information.

This approach helps keep the chatbot grounded in the selected book.

🕷️ Data Collection

The system automatically discovers the book's chapter/subpage URLs from the main Bengali Wikisource page.

Each discovered page is downloaded and processed individually.

The crawler preserves information such as:
Book
Author
Year
Chapter
Section
Page Title
Source URL
This metadata is later used to identify the source of retrieved information.

