# 🎓 EduGraph — AI College Assistant

EduGraph is a **LangGraph-powered college assistant** that answers student questions using **Retrieval-Augmented Generation (RAG)** over the academic handbook and fee structure PDFs. It includes a Streamlit chat interface and a command-line version.

## 🚀 Features

* 🤖 **LangGraph Workflow** — Processes queries through a graph with conditional routing.
* 🧠 **LLM-Powered Query Classification** — Automatically classifies queries as academic, fee, or general.
* 📚 **PDF-Based RAG** — Retrieves relevant passages from the academic handbook or fee structure.
* 🔎 **FAISS Vector Search** — Finds relevant document chunks using semantic similarity.
* 🧩 **Hugging Face Embeddings** — Uses `all-MiniLM-L6-v2` to embed document and query text.
* ⚡ **Groq + Llama 3.3 70B** — Generates contextual responses.
* 🎓 **Programme-Aware Responses** — Tailors answers for BCA, BBA, or B.Com (H) students.
* 💬 **Streamlit Chat UI** — Provides an interactive chat interface, with a CLI also available.

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
| Groq          | LLM inference provider              |
| Llama 3.3 70B | Language model                      |
| FAISS         | Vector similarity search            |
| Hugging Face  | Text embeddings                     |
| PyPDF         | PDF document loading                |
| dotenv        | Environment variable management     |
| Streamlit     | Chat user interface                 |

## 📂 Project Structure

```text
EduGraph/
├── README.md
└── Edu-graph-main/
    ├── app.py                  # Streamlit application
    ├── conditional_RAG.py      # Command-line application
    ├── academics_handbook.pdf
    ├── fee_structure.pdf
    ├── requirements.txt
    ├── env.example
    └── .gitignore
```

## ⚙️ Installation

### 1. Clone the repository

```bash
git clone https://github.com/kunalmathiyan05/EduGraph.git
cd EduGraph/Edu-graph-main
```

### 2. Create a virtual environment

Using Python:

```bash
python -m venv .venv
```

Activate it on Windows:

```powershell
.\.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure the API key

Create a `.env` file from the provided example:

```powershell
Copy-Item env.example .env
```

Then open `.env` and add your Groq API key:

```env
GROQ_API_KEY=your_groq_api_key_here
```

**Never commit your `.env` file or API keys to GitHub.**

### 5. Run the Streamlit app

```bash
streamlit run app.py
```

### Command-line version

To use the terminal interface instead:

```bash
python conditional_RAG.py
```

Select your programme when prompted:

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

Type `exit` or `quit` in the terminal to close the command-line application.

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

## 🔮 Possible Future Improvements

Ideas for future development include:

* [ ] Conversation memory
* [ ] Source/page citations
* [ ] Answer verification node
* [ ] Self-correction and retrieval retry loops
* [ ] Additional admission and examination agents
* [ ] Web search integration
* [ ] Persistent user sessions
* [ ] Human-in-the-loop workflows

## 👨‍💻 Author

**KUNAL MATHIYAN**

Computer Science Engineering Student

---

⭐ If you find this project useful, consider giving the repository a star.
