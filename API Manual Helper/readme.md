# API Manual Helper - n8n Workflow

This n8n workflow automates the process of ingesting API documentation PDFs into a Pinecone vector store for use in a Retrieval-Augmented Generation (RAG) AI assistant.  
It extracts **complete API method blocks** (method name, endpoint, parameters, examples) from PDF files, generates embeddings with OpenAI, and stores them in Pinecone for semantic search.

---

## 📌 Features
- Automatically triggers when a new API manual PDF is added to Google Drive.
- Extracts **full logical API sections** without splitting parameters, examples, or code blocks.
- Generates embeddings using **OpenAI**.
- Stores vector data in **Pinecone** for semantic retrieval.

---

## 🛠 Workflow Steps

### 1. **Google Drive Trigger**
- **Purpose**: Watches a specified Google Drive folder for new files.
- **Event**: `fileCreated`
- **Setup**:
  - Configure Google Drive credentials in n8n.
  - Select the folder where API manuals will be uploaded.

---

### 2. **If PDF**
- **Purpose**: Filters files to process only PDFs.
- **Condition**: Checks if file MIME type is `application/pdf`.

---

### 3. **Download File**
- **Purpose**: Downloads the PDF file from Google Drive.
- **Output**: Binary file for further processing.

---

### 4. **Extract from File**
- **Purpose**: Extracts text from the PDF.
- **Node Type**: PDF Extract node.
- **Output**: Raw text of the API manual.

---

### 5. **Filter API Methods, Parameters, Examples**
- **Purpose**: Code node that uses a **universal regex** to:
  - Identify API method start lines.
  - Capture the entire section (method name, endpoint, parameters, examples) until the next method.
  - Output one JSON item per API method block.
- **Output**: Each item is a *complete chunk* of API documentation, ready for embedding.

---

### 6. **Embeddings OpenAI**
- **Purpose**: Generates embeddings for each API chunk.
- **Model**: `text-embedding-3-small` (configurable).
- **Setup**: Requires OpenAI API credentials in n8n.

---

### 7. **Default Data Loader**
- **Purpose**: Prepares document chunks for Pinecone.
- **Includes**:
  - Chunk text.
  - Optional metadata (method name, endpoint, etc.).

---

### 8. **Recursive Character Text Splitter**
- **Purpose**: (Optional) Splits very large API method chunks while keeping logical boundaries.
- **Chunk Size**: Configurable (e.g., 1000–1500 characters with overlap).

---

### 9. **Pinecone Vector Store**
- **Purpose**: Stores embeddings + metadata in Pinecone.
- **Setup**:
  - Pinecone API credentials in n8n.
  - Index name: `apimanuals` (configurable).

---

## 🔧 Setup Instructions
1. Clone or download this workflow JSON into your n8n instance.
2. Set up the following credentials in n8n:
   - **Google Drive API** (for file trigger and download).
   - **OpenAI API** (for embeddings).
   - **Pinecone API** (for vector storage).
3. Configure:
   - Google Drive folder ID in the trigger node.
   - Pinecone index name & namespace.
   - OpenAI embedding model (default: `text-embedding-3-small`).
4. Test the workflow by uploading a sample API manual PDF to the Google Drive folder.

---

## 📜 License
MIT License – feel free to use and modify for your projects.
