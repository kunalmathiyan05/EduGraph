# 🎓 EduGraph — Agentic College Assistant

EduGraph is a **LangGraph-powered AI college assistant** that intelligently routes student queries and uses **Retrieval-Augmented Generation (RAG)** to answer academic and fee-related questions from official college documents.

## 🚀 Features

* 🤖 **LangGraph Workflow** — Stateful graph-based query processing and conditional routing.
* 🧠 **LLM-Powered Query Classification** — Automatically classifies queries as academic, fee, or general.
* 📚 **PDF-Based RAG** — Retrieves relevant information from college academic and fee documents.
* 🔎 **FAISS Vector Search** — Performs efficient semantic similarity search over document embeddings.
* 🧩 **HuggingFace Embeddings** — Uses `all-MiniLM-L6-v2` for document and query embeddings.
* ⚡ **Groq + Llama 3.3 70B** — Generates fast and contextual responses.
* 🎓 **Programme-Aware Responses** — Personalizes answers according to the student's programme.
* 🔀 **Conditional Graph Routing** — Sends each query to the appropriate retrieval or general-response node.

## 🏗️ Architecture

```text
                         Student Query
                              │
                              ▼
                    ┌──────────────────┐
                    │  Query Classifier │
                    └────────┬─────────┘
                             │
             ┌───────────────┼───────────────┐
             ▼               ▼               ▼
        Academic RAG      Fee RAG       General
             │               │               │
             ▼               ▼               │
        Academic PDF      Fee PDF             │
             │               │               │
             └───────────────┼───────────────┘
                             ▼
                    ┌──────────────────┐
                    │ Response Generator│
                    └────────┬─────────┘
                             ▼
                       Final Answer
```

## 🛠️ Tech Stack

| Technology    | Purpose                             |
| ------------- | ----------------------------------- |
| Python        | Core development                    |
| LangGraph     | Agent workflow and state management |
| LangChain     | LLM and RAG integration             |
| Groq          | LLM inference                       |
| Llama 3.3 70B | Language model                      |
| FAISS         | Vector similarity search            |
| HuggingFace   | Text embeddings                     |
| PyPDF         | PDF document loading                |
| dotenv        | Environment variable management     |

## 📂 Project Structure

```text
EduGraph/
│
├── main.py
├── academics_handbook.pdf
├── fee_structure.pdf
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md
```

## ⚙️ Installation

### 1. Clone the repository

```bash
git clone https://github.com/udaydhaila1926/Edu-graph.git
cd Edu-graph
```

### 2. Create a virtual environment

Using Python:

```bash
python -m venv .venv
```

Activate it on Windows:

```powershell
.venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure the API key

Create a `.env` file:

```env
GROQ_API_KEY=your_groq_api_key_here
```

**Never commit your `.env` file or API keys to GitHub.**

### 5. Run EduGraph

```bash
python main.py
```

Select your programme:

```text
1. BCA
2. BBA
3. B.Com (H)
```

You can then ask questions such as:

```text
What is the minimum attendance requirement?

What are the examination rules?

How much is the tuition fee?

What happens if I miss the fee payment deadline?
```

Type `exit` or `quit` to close the application.

## 🔄 How It Works

### 1. Document Processing

The academic handbook and fee structure PDFs are loaded using `PyPDFLoader`.

### 2. Text Chunking

Documents are split into smaller chunks using `RecursiveCharacterTextSplitter`.

```text
Chunk Size: 800
Overlap: 100
```

### 3. Embedding Generation

Each chunk is converted into a vector using:

```text
sentence-transformers/all-MiniLM-L6-v2
```

### 4. Vector Storage

The embeddings are stored in separate **FAISS vector stores** for academic and fee documents.

### 5. Query Classification

LangGraph sends the student's query to a classifier node that determines whether the query is:

```text
academic
fee
general
```

### 6. Conditional Routing

LangGraph dynamically routes the query to the appropriate node.

### 7. Response Generation

The retrieved context and student's programme are provided to the Llama 3.3 70B model to generate a contextual response.

## 🧠 LangGraph State

EduGraph maintains a shared state containing:

```python
class State(TypedDict):
    programme: str
    messages: Annotated[list, add_messages]
    query_type: str
    retrieved_context: str
```

This allows different nodes in the graph to communicate through a shared state.

## 📌 Example

**Student:**

```text
What are the attendance requirements for my programme?
```

**EduGraph:**

```text
Query → Classifier
          ↓
       Academic
          ↓
     Academic RAG
          ↓
    Relevant PDF chunks
          ↓
     Groq / Llama 3.3
          ↓
      Final Answer
```

## 🔮 Future Improvements

Planned improvements include:

* [ ] Conversation memory
* [ ] Source/page citations
* [ ] Answer verification node
* [ ] Self-correction and retrieval retry loops
* [ ] Additional admission and examination agents
* [ ] Web search integration
* [ ] Streamlit web interface
* [ ] Persistent user sessions
* [ ] Human-in-the-loop workflows

## 👨‍💻 Author

**KUNAL MATHIYAN**

Computer Science Engineering Student

---

⭐ If you find this project useful, consider giving the repository a star.
