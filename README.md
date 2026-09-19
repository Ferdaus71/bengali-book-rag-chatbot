How the System Works
1. Book Collection

The system connects to Bengali Wikisource and identifies the main book page and its chapter/subpage URLs.

The crawler discovers pages belonging to the selected edition of কপালকুণ্ডলা.

2. Text Extraction

The system extracts the main textual content from each Wikisource page.

Unnecessary HTML elements such as:

Navigation
Tables
Scripts
Styles
References
Metadata
Footer elements

are removed before processing.

3. Text Cleaning

The extracted Bengali text is cleaned and normalized before being converted into LangChain documents.

Each document preserves important metadata:

Book
Author
Year
Chapter
Section
Source URL
Page Title

This metadata is later used for source attribution.

4. Text Chunking

The project uses a recursive text splitting strategy with Bengali-aware separators.

Main configuration:

Chunk Size: 1000
Chunk Overlap: 150

The splitter uses separators such as:

\n\n
\n
।
॥
?
!
,
space

This allows the book to be divided into smaller searchable knowledge units while preserving Bengali sentence boundaries as much as possible.

🔎 Embedding Model

The project uses:

BAAI/bge-m3

BGE-M3 is a multilingual embedding model used to create semantic vector representations of the Bengali book content.

Configuration:

EMBEDDING_MODEL = "BAAI/bge-m3"

The embeddings are generated using:

Hugging Face Embeddings

with normalized embeddings.

🗄️ Vector Database

The project uses:

ChromaDB

ChromaDB stores the vector representations of the book chunks and enables semantic similarity search.

The project collection is:

kapalkundala_bengali_v2

The vector database is created from the processed book chunks.

🔍 Retrieval

The system uses a LangChain retriever to search for the most relevant book passages.

The retriever performs semantic similarity search against the ChromaDB vector database.

Retrieved documents contain information such as:

Book Content
Chapter
Section
Source URL

These retrieved documents are then passed to the RAG prompt as context.

🤖 Large Language Model

The project uses Google's Gemini model through LangChain.

The final notebook configuration uses:

gemini-2.5-flash-lite

Configuration:

LLM_MODEL = "gemini-2.5-flash-lite"

The model uses:

temperature = 0

This configuration is intended to keep responses focused and deterministic.

🛡️ Grounded RAG Approach

A strict RAG prompt is used to keep the chatbot grounded in the selected book.

The chatbot follows these rules:

Answer only from the retrieved book context.
Do not use unrelated external knowledge.
Do not guess.
Do not invent information.
Answer in Bengali.
Provide relevant chapter and section information.
If the answer is not available in the provided context, explicitly state:

"এই তথ্যটি নির্বাচিত বইয়ের প্রদত্ত অংশে পাওয়া যায়নি।"

This makes the chatbot a book-grounded Knowledge Base Assistant rather than a general-purpose chatbot.

💬 Gradio User Interface

The project includes an interactive Gradio interface.

Users can ask questions about:

Characters
Relationships
Events
Story details
Descriptions
Other information available in the selected book

The interface displays the generated Bengali answer together with relevant source information.

🧪 Evaluation

The project includes an evaluation set of 10 questions.

The evaluation records:

Question
Generated Answer
Retrieved Sources

The results can be stored in a Pandas DataFrame for further analysis.

❌ No-Answer / Out-of-Domain Test

The project includes an explicit no-answer test:

২০২৬ সালের বাংলাদেশ ক্রিকেট দলের অধিনায়ক কে?

This question is unrelated to the selected book.

The purpose of this test is to verify that the chatbot does not simply use general knowledge to answer every question.

When the requested information is not available in the retrieved book context, the system is instructed to respond:

এই তথ্যটি নির্বাচিত বইয়ের প্রদত্ত অংশে পাওয়া যায়নি।

This demonstrates the grounding behavior of the RAG system.

📝 Example Questions

The chatbot can be tested with questions such as:

কপালকুণ্ডলা উপন্যাসের প্রধান চরিত্র কারা?

কপালকুণ্ডলা কে?

নবকুমার কে?

মতিবিবি কে?

কপালকুণ্ডলার সঙ্গে নবকুমারের কী সম্পর্ক?

উপন্যাসে কাপালিকের ভূমিকা কী?

নবকুমার কীভাবে কপালকুণ্ডলার সঙ্গে পরিচিত হয়?

কপালকুণ্ডলার চরিত্রের প্রধান বৈশিষ্ট্য কী?

উপন্যাসে বনভূমির কী ধরনের বর্ণনা পাওয়া যায়?
🧩 Chunking Strategy Comparison

As an additional experiment, the project compares two chunking configurations.

Strategy A
Chunk Size: 800
Overlap: 100
Strategy B
Chunk Size: 1200
Overlap: 200

The comparison considers:

Chunk size
Chunk overlap
Number of generated chunks

This experiment demonstrates how different chunking configurations affect the structure of the searchable knowledge base.

🛠️ Technology Stack
Technology	Purpose
Python	Main programming language
Google Colab	Development environment
BeautifulSoup	Web page parsing
LangChain	RAG orchestration
BAAI/bge-m3	Multilingual embeddings
Hugging Face	Embedding model ecosystem
ChromaDB	Vector database
Gemini	Large Language Model
Gradio	Interactive chatbot UI
Pandas	Evaluation and data processing
Bengali Wikisource	Knowledge source
📦 Installation

Install the required dependencies:

pip install -r requirements.txt

The project uses packages including:

requests
beautifulsoup4
lxml
pandas
tqdm
sentence-transformers
langchain
langchain-community
langchain-text-splitters
langchain-huggingface
langchain-chroma
chromadb
langchain-google-genai
gradio
🔑 Gemini API Key Setup

The project requires a Gemini API key.

In Google Colab:

Open the Secrets panel.
Add a new secret.
Use the following name:
GEMINI_API_KEY
Enable notebook access for the secret.

The notebook loads the API key using the Google Colab Secrets mechanism.

Security

Never commit your Gemini API key to GitHub.

Do not place API keys directly inside the notebook or source code.

▶️ Running the Project
Google Colab

Open:

Bengali_Book_RAG_Chatbot_Kapalkundala.ipynb

Run the notebook cells sequentially.

Recommended workflow:

1. Install dependencies
2. Import libraries
3. Configure project
4. Configure HTTP headers
5. Load Gemini API key
6. Test Wikisource connection
7. Get book page
8. Discover book chapters
9. Build final page list
10. Extract book content
11. Clean and preprocess text
12. Create document metadata
13. Create document chunks
14. Load BGE-M3
15. Create ChromaDB
16. Create retriever
17. Test retrieval
18. Initialize Gemini
19. Build RAG prompt
20. Run RAG pipeline
21. Run no-answer test
22. Run evaluation
23. Compare chunking strategies
24. Build Gradio interface
25. Launch chatbot
26. Run final project checks
📁 Recommended Project Structure
bengali-book-rag-chatbot/
│
├── Bengali_Book_RAG_Chatbot_Kapalkundala.ipynb
│
├── README.md
│
├── requirements.txt
│
├── .gitignore
│
└── assets/
    └── screenshots/
🔐 Recommended .gitignore
# Python
__pycache__/
*.py[cod]

# Jupyter
.ipynb_checkpoints/

# Environment variables
.env
.env.*

# API keys
secrets.json
*.key

# Vector databases
chroma/
chroma_db/
*_chroma_db/
*.sqlite3

# Generated files
*.csv
*.log

# Operating system
.DS_Store
Thumbs.db
📓 Notebook
Recommended Notebook Name
Bengali_Book_RAG_Chatbot_Kapalkundala.ipynb
Notebook Title

📚 Bengali Book Knowledge Base Chatbot — কপালকুণ্ডলা

The notebook demonstrates the complete workflow from Bengali book collection to the final RAG chatbot.

🌐 Interactive Chatbot

The chatbot interface is built using Gradio.

It allows users to interact with the Knowledge Base through natural-language Bengali questions.

The interface provides:

Bengali question input
Generated Bengali answer
Retrieved source information
Chapter information
Section information
Example questions
No-answer handling
🔬 Complete Project Workflow
1. Select Bengali Book
          ↓
2. Crawl Bengali Wikisource
          ↓
3. Discover Chapter Pages
          ↓
4. Extract Book Text
          ↓
5. Clean Text
          ↓
6. Create Metadata
          ↓
7. Split Text into Chunks
          ↓
8. Generate BGE-M3 Embeddings
          ↓
9. Store Embeddings in ChromaDB
          ↓
10. Retrieve Relevant Chunks
          ↓
11. Build Strict RAG Context
          ↓
12. Generate Answer with Gemini
          ↓
13. Display Bengali Answer
          ↓
14. Display Chapter / Section Sources
🎓 Assignment Demonstration

The project is designed for a 3–5 minute demonstration.

Part 1 — Pipeline

Demonstrate:

Selected book
Bengali Wikisource
Text extraction
Text preprocessing
Chunking
BGE-M3 embeddings
ChromaDB
Retriever
Gemini
RAG pipeline
Part 2 — Question Answering

Ask at least five questions related to:

কপালকুণ্ডলা

Part 3 — No-Answer Case

Ask:

২০২৬ সালের বাংলাদেশ ক্রিকেট দলের অধিনায়ক কে?

Then demonstrate that the chatbot does not generate an unsupported book answer when the information is outside the knowledge base.

✅ Final Project Validation

The notebook performs final validation for major components including:

✓ Book URL
✓ Book Pages
✓ Documents
✓ Chunks
✓ Embeddings
✓ Vector Store
✓ Retriever
✓ LLM
✓ Evaluation
✓ Gradio Interface

When the required components are available, the project reports a successful project check.

🚀 Future Improvements

Potential future improvements include:

Bengali-specific reranking
Hybrid keyword + semantic retrieval
Improved citation formatting
Retrieval evaluation using Hit Rate / Recall@K
Embedding model comparison
Reranker integration
Persistent production vector database
FastAPI backend
Authentication
Conversation memory
Multi-book Bengali Knowledge Base
Bengali voice interaction
Cloud deployment
👨‍💻 Developer
Md. Ferdaus Hossen

AI/ML Engineer • Researcher • RAG Developer

Areas of Interest
Artificial Intelligence
Machine Learning
Deep Learning
Natural Language Processing
Large Language Models
Retrieval-Augmented Generation
Computer Vision
Intelligent Automation
📜 Disclaimer

This project is developed for educational and research purposes.

The chatbot is designed to answer questions using the selected book content available through the Bengali Wikisource source used in this project.

Generated answers should be interpreted together with the retrieved source context.

🙏 Acknowledgements

Special thanks to:

Bengali Wikisource for providing the book source
LangChain for RAG orchestration
Hugging Face for the embedding ecosystem
ChromaDB for vector storage and retrieval
Google Gemini for language generation
Gradio for the interactive interface
📌 Project Summary

Bengali Book Knowledge Base Chatbot — কপালকুণ্ডলা demonstrates a complete Bengali RAG workflow:

Web Source
    ↓
Text Extraction
    ↓
Preprocessing
    ↓
Chunking
    ↓
BGE-M3 Embeddings
    ↓
ChromaDB
    ↓
Semantic Retrieval
    ↓
Gemini
    ↓
Grounded Bengali Answer
    ↓
Chapter / Section Source

The project demonstrates how Bengali literary content can be transformed into a searchable Knowledge Base and used to build a focused RAG-powered chatbot.

⭐ GitHub Repository

Repository Name:

bengali-book-rag-chatbot

Repository Title:

Bengali Book Knowledge Base Chatbot — কপালকুণ্ডলা

Short Description:

A Bengali RAG chatbot for কপালকুণ্ডলা using BGE-M3, ChromaDB, LangChain, Gemini, and Gradio.
Suggested GitHub Topics
rag
rag-chatbot
bengali
bengali-nlp
nlp
llm
langchain
chromadb
bge-m3
gemini
gradio
question-answering
knowledge-base
generative-ai
retrieval-augmented-generation

Developed by Md. Ferdaus Hossen
