# 📚 Bengali Book RAG Chatbot — কপালকুণ্ডলা

A Bengali Retrieval-Augmented Generation (RAG) chatbot designed to answer questions about **কপালকুণ্ডলা**, the classic Bengali novel by **বঙ্কিমচন্দ্র চট্টোপাধ্যায়**.

The system automatically discovers chapter/subpage URLs from Bengali Wikisource, extracts and cleans the book content, preserves source metadata, creates semantic embeddings using **BAAI/bge-m3**, stores them in **ChromaDB**, and retrieves relevant passages for grounded responses using **Google Gemini**.

The chatbot is built to answer **only from the selected book's knowledge base** and explicitly returns a no-answer response when the required information cannot be found.

---

## 🌟 Features

* 🕷️ Automatic Bengali Wikisource page discovery
* 📖 Chapter and section-level book extraction
* 🧹 Bengali-aware text cleaning and preprocessing
* ✂️ Configurable text chunking
* 🧬 Multilingual semantic embeddings with `BAAI/bge-m3`
* 🗄️ ChromaDB vector database
* 🔍 Semantic similarity retrieval
* 🤖 Google Gemini-powered answer generation
* 🛡️ Strict context-grounded answering
* 📌 Chapter and section source information
* 🚫 Hallucination/no-answer handling
* 🧪 Predefined evaluation questions
* 📊 Chunking strategy comparison
* 💬 Gradio chatbot interface
* 🇧🇩 Bengali-first question answering

---

# 🏗️ System Architecture

```text
                    Bengali Wikisource
                           │
                           ▼
                  Chapter URL Discovery
                           │
                           ▼
                    Web Page Download
                           │
                           ▼
                  Text Extraction
                           │
                           ▼
                Bengali Text Cleaning
                           │
                           ▼
                    Metadata Creation
                           │
                           ▼
                    Text Chunking
                 1000 chars / 150 overlap
                           │
                           ▼
                  BAAI/bge-m3 Embeddings
                           │
                           ▼
                       ChromaDB
                           │
                           ▼
                    Semantic Retriever
                           │
                           ▼
                     Relevant Context
                           │
                           ▼
                   Google Gemini LLM
                           │
                           ▼
                  Grounded Bengali Answer
                           │
                           ▼
                    Gradio Chatbot UI
```

---

# 📖 Knowledge Base

### Book

**কপালকুণ্ডলা**

### Author

**বঙ্কিমচন্দ্র চট্টোপাধ্যায়**

### Publication Year

**১৮৭০**

The crawler automatically discovers the book's chapter/subpage URLs from the main Bengali Wikisource page.

Each discovered page is downloaded and processed individually.

The following metadata is preserved:

```text
Book
Author
Year
Chapter
Section
Page Title
Source URL
```

This metadata is later used to identify the source of retrieved information.

---

# 🕷️ Data Collection

The data collection pipeline automatically discovers relevant book pages and downloads them individually.

```text
Main Wikisource Page
        │
        ▼
Discover Chapter URLs
        │
        ▼
Download Individual Pages
        │
        ▼
Extract Book Content
        │
        ▼
Attach Metadata
```

The crawler is designed to preserve the relationship between each text passage and its original source page.

---

# 🧹 Text Extraction & Preprocessing

Wikisource pages contain both book content and webpage-specific elements.

The preprocessing pipeline removes unnecessary elements such as:

* Navigation elements
* Scripts
* Styles
* Tables
* Footers
* Forms
* References
* Metadata
* Category information
* Edit controls
* Other webpage-specific elements

The remaining text is normalized and cleaned before entering the chunking pipeline.

### Bengali Text Preservation

The preprocessing stage is designed to preserve Bengali Unicode text while removing unnecessary webpage noise.

```text
Raw HTML
   │
   ▼
Remove Web Elements
   │
   ▼
Extract Main Text
   │
   ▼
Normalize Bengali Text
   │
   ▼
Clean Text
   │
   ▼
Processed Book Content
```

---

# ✂️ Text Chunking

Long book pages are divided into smaller chunks before embedding generation.

### Current Configuration

```text
Chunk Size: 1000 characters
Chunk Overlap: 150 characters
```

Bengali-aware separators are used where appropriate to preserve meaningful text boundaries.

### Why Chunking?

Chunking allows the retrieval system to identify smaller and more relevant sections instead of retrieving an entire chapter for every question.

The overlap helps preserve contextual information between neighboring chunks.

```text
Original Chapter
────────────────────────────────────────

Chunk 1
████████████████████████████

             Chunk 2
             ████████████████████████████

                          Chunk 3
                          ████████████████████████████
```

---

# 🧬 Embedding Model

The project uses:

```text
BAAI/bge-m3
```

### Why BGE-M3?

`BAAI/bge-m3` is a multilingual embedding model suitable for semantic search across multiple languages.

It is useful for this project because the knowledge base contains Bengali text and the chatbot requires semantic retrieval rather than simple keyword matching.

The embedding pipeline transforms text chunks into numerical vector representations.

```text
Book Chunk
    │
    ▼
BGE-M3
    │
    ▼
Embedding Vector
    │
    ▼
ChromaDB
```

---

# 🗄️ Vector Database

The project uses:

```text
ChromaDB
```

### Collection

```text
kapalkundala_bengali_v2
```

The vector database stores:

* Document chunks
* Embeddings
* Book metadata
* Chapter information
* Section information
* Page titles
* Source URLs

---

# 🔍 Retriever

LangChain connects the vector database with the RAG pipeline.

The retriever performs semantic similarity search and returns the most relevant book passages for a user's question.

```text
User Question
      │
      ▼
Question Embedding
      │
      ▼
ChromaDB Similarity Search
      │
      ▼
Relevant Book Chunks
      │
      ▼
LLM Context
```

The retrieved documents are then passed to the language model as context.

---

# 🤖 Language Model

The project uses Google's Gemini model through LangChain.

### Configuration

```text
Model:
gemini-2.5-flash-lite

Temperature:
0
```

A temperature of `0` is used to make response generation more deterministic and focused on the retrieved context.

---

# 🛡️ Grounded Answering

A core requirement of the project is preventing unsupported answers.

The RAG prompt instructs the model to:

* Use only the retrieved book context
* Avoid unsupported general knowledge
* Avoid guessing
* Avoid inventing information
* Answer in Bengali
* Mention relevant chapter and section
* Report when information is unavailable

### No-Answer Response

When the required information cannot be found in the knowledge base:

> **এই তথ্যটি নির্বাচিত বইয়ের প্রদত্ত অংশে পাওয়া যায়নি।**

This provides a controlled fallback mechanism for unsupported questions.

---

# 💬 Gradio User Interface

The chatbot provides a simple Gradio-based interface.

### Interface Features

* Bengali question input
* Chat history
* Answer generation
* Source chapter
* Source section
* Clear conversation option
* Example questions
* Developer information

The UI is intentionally kept simple so that users can focus on interacting with the book knowledge base.

---

# 🧪 Evaluation

The project includes a predefined evaluation set containing **10 questions**.

The questions are designed to evaluate different aspects of the knowledge base.

### Evaluation Categories

* Character-related questions
* Story-related questions
* Event-related questions
* Relationship-related questions
* Book-content questions
* Unsupported questions

The evaluation process checks whether the system can:

1. Retrieve the correct passage.
2. Provide a grounded answer.
3. Identify the relevant source.
4. Avoid hallucinating unsupported information.

---

# 🚫 No-Answer Test

A dedicated no-answer test verifies that the chatbot does not answer questions outside the selected book.

### Example

```text
২০২৬ সালের বাংলাদেশ ক্রিকেট দলের অধিনায়ক কে?
```

This question is unrelated to the book.

### Expected Response

```text
এই তথ্যটি নির্বাচিত বইয়ের প্রদত্ত অংশে পাওয়া যায়নি।
```

This test demonstrates the grounding behavior of the RAG system.

---

# 📊 Chunking Strategy Comparison

The project can compare different chunking configurations to investigate their effect on retrieval performance.

## Configuration A

```text
Chunk Size: 800
Chunk Overlap: 100
```

## Configuration B

```text
Chunk Size: 1200
Chunk Overlap: 200
```

The configurations can be evaluated based on retrieval performance, such as whether the correct source passage appears among the retrieved documents.

This provides a simple experimental framework for studying how chunk size and overlap affect Bengali information retrieval.

---

# 🧰 Technology Stack

| Technology                            | Purpose                      |
| ------------------------------------- | ---------------------------- |
| **Python**                            | Main programming language    |
| **BeautifulSoup**                     | Web content extraction       |
| **Requests**                          | Downloading Wikisource pages |
| **LangChain**                         | RAG orchestration            |
| **BAAI/bge-m3**                       | Multilingual text embeddings |
| **ChromaDB**                          | Vector database              |
| **Google Gemini**                     | Language model               |
| **Gradio**                            | Chatbot interface            |
| **FAISS / Vector Retrieval Concepts** | Semantic retrieval concepts  |
| **Google Colab**                      | Development environment      |

---

# 📦 Installation

## Clone the Repository

```bash
git clone https://github.com/Ferdaus71/bengali-book-rag-chatbot.git
cd bengali-book-rag-chatbot
```

## Install Dependencies

```bash
pip install -r requirements.txt
```

For Google Colab, dependencies can also be installed directly inside the notebook.

---

# 🔑 Gemini API Configuration

The chatbot requires a Google Gemini API key.

Configure the API key as an environment variable.

```python
import os

os.environ["GOOGLE_API_KEY"] = "YOUR_GEMINI_API_KEY"
```

For production or public repositories, do **not** commit your real API key.

Recommended approaches include:

```text
.env
Google Colab Secrets
Environment Variables
Secret Management Services
```

Example `.env`:

```env
GOOGLE_API_KEY=your_api_key_here
```

Make sure `.env` is included in `.gitignore`.

---

# 📁 Project Structure

```text
bengali-book-rag-chatbot/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   └── Bengali_Book_RAG_Chatbot_Kapalkundala.ipynb
│
├── data/
│   └── README.md
│
├── src/
│   ├── crawler.py
│   ├── preprocessing.py
│   ├── embeddings.py
│   ├── vectorstore.py
│   ├── retriever.py
│   ├── rag.py
│   └── app.py
│
├── evaluation/
│   └── evaluation_questions.md
│
└── .env.example
```

> The exact structure may vary depending on the final implementation.

---

# ▶️ Running the Project

## 1. Open the Notebook

Open:

```text
notebooks/Bengali_Book_RAG_Chatbot_Kapalkundala.ipynb
```

Run the notebook cells sequentially.

## 2. Configure Gemini API

Set the Gemini API key using a secure environment variable or Colab Secret.

## 3. Build the Knowledge Base

Run the data collection and preprocessing stages.

```text
Discover Book Pages
        ↓
Download Pages
        ↓
Extract Text
        ↓
Clean Text
        ↓
Preserve Metadata
        ↓
Create Chunks
        ↓
Generate Embeddings
        ↓
Store in ChromaDB
```

## 4. Initialize RAG

The notebook initializes:

```text
Embedding Model
       +
ChromaDB
       +
Retriever
       +
Gemini
       ↓
RAG Pipeline
```

## 5. Launch Gradio

After initialization, launch the Gradio interface and ask questions about **কপালকুণ্ডলা**.

---

# 💡 Example Questions

```text
কপালকুণ্ডলা উপন্যাসের প্রধান চরিত্র কারা?

কপালকুণ্ডলার সঙ্গে নবকুমারের প্রথম দেখা কোথায় হয়?

নবকুমার কে?

কপালকুণ্ডলার চরিত্র সম্পর্কে কী জানা যায়?

কপালকুণ্ডলা ও নবকুমারের সম্পর্ক সম্পর্কে কী জানা যায়?
```

The answers should always be generated from the retrieved book context.

---

# 🔐 Data Grounding Policy

The chatbot follows a strict knowledge-base policy.

```text
                    User Question
                          │
                          ▼
                 Retrieve Book Data
                          │
                          ▼
               Relevant Context Found?
                    /            \
                  YES             NO
                   │               │
                   ▼               ▼
              Gemini LLM      No-Answer
                   │
                   ▼
             Grounded Answer
```

The system is **not intended to function as a general-purpose chatbot**.

Its primary purpose is answering questions related to the selected book.

---

# ⚙️ Key Configuration

```text
Book:
কপালকুণ্ডলা

Author:
বঙ্কিমচন্দ্র চট্টোপাধ্যায়

Year:
১৮৭০

Embedding:
BAAI/bge-m3

Vector Database:
ChromaDB

Collection:
kapalkundala_bengali_v2

Chunk Size:
1000

Chunk Overlap:
150

LLM:
gemini-2.5-flash-lite

Temperature:
0

Interface:
Gradio
```

---

# 🧪 Validation Checklist

The project validates the following components:

* [x] Bengali book selected
* [x] Book chapter discovery
* [x] Web content extraction
* [x] Bengali text preprocessing
* [x] Metadata preservation
* [x] Text chunking
* [x] Multilingual embeddings
* [x] Vector database
* [x] Semantic retrieval
* [x] RAG pipeline
* [x] Grounded response generation
* [x] Chapter/section source information
* [x] No-answer behavior
* [x] Gradio interface
* [x] Evaluation questions
* [x] Chunking strategy comparison

---

# 🚀 Future Improvements

Potential improvements include:

* Improve Bengali-specific text normalization
* Add retrieval reranking
* Compare Bengali and multilingual embedding models
* Implement hybrid BM25 + vector retrieval
* Add retrieval confidence scores
* Improve source citation formatting
* Add conversation-aware retrieval
* Support multiple Bengali books
* Add automated retrieval evaluation
* Add FastAPI backend
* Deploy as a web application
* Add multilingual question support
* Add document-level analytics

---

# ⚠️ Limitations

The chatbot's performance depends on:

1. Quality of extracted Wikisource text
2. Text preprocessing quality
3. Chunk size and overlap
4. Embedding quality
5. Retrieval accuracy
6. LLM adherence to grounding instructions

A retrieved passage may not always contain enough information to answer a question completely.

When the requested information is unavailable in the knowledge base, the chatbot is designed to explicitly report that limitation rather than generate an unsupported answer.

---

# 🎓 Academic & Educational Purpose

This project demonstrates the practical implementation of:

* Natural Language Processing
* Bengali NLP
* Information Retrieval
* Semantic Search
* Vector Databases
* Large Language Models
* Retrieval-Augmented Generation
* Prompt Engineering
* LangChain
* Generative AI Applications

The project is intended as an educational and technical demonstration of building a **domain-specific Bengali RAG system**.

---

# 👨‍💻 Developer

**Md. Ferdaus Hossen**

AI/ML Engineer & Researcher
Computer Science & Engineering

🔗 GitHub: `https://github.com/Ferdaus71`

---

# ⭐ Acknowledgement

This project uses Bengali literary content available through Wikimedia's Wikisource platform and open-source AI technologies for educational and research purposes.

---

# 📄 License

This project is intended for educational and research purposes.

Please review the applicable copyright and content licensing terms of the original source material before redistributing extracted book content.

---

## 🔖 Keywords

```text
Bengali RAG
Bengali NLP
Bangla NLP
Retrieval Augmented Generation
RAG Chatbot
Bengali Chatbot
কপালকুণ্ডলা
Bankim Chandra Chattopadhyay
LangChain
ChromaDB
BGE-M3
Gemini
Gradio
Semantic Search
Vector Database
Generative AI
Large Language Models
Information Retrieval
```
