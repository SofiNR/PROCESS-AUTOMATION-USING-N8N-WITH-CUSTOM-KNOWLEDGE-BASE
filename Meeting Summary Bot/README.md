📑 Meeting Summary Vector Workflow (n8n)

This repository contains an n8n workflow designed to process meeting transcript PDFs from Google Drive, extract only the speaker-dialogue sections, and store them in a Pinecone vector database for efficient semantic search and retrieval.

🚀 Features

    📂 Google Drive Trigger – Detects newly uploaded PDF meeting transcripts.

    📥 File Download – Downloads the detected PDF file from Google Drive.

    📑 Extract from PDF – Converts the PDF content into raw text.

    🎙 Extract Speaker Dialogue Section – Filters the raw text to retain only dialogues.

    🧩 Text Splitting – Uses Recursive Character Text Splitter to break dialogues into manageable chunks.

    🧠 Embeddings with OpenAI – Generates vector embeddings for dialogue chunks.

    📦 Pinecone Vector Store – Stores embeddings for semantic search and retrieval.

📊 Workflow Overview

    Google Drive Trigger

      Starts the workflow when a new file is added.

    If PDF

      Ensures only PDF files are processed.

    Download File

      Downloads the uploaded PDF file.

    Extract from PDF

      Extracts raw text content from the PDF.

    Extract Speaker Dialogue Section

      Applies regex-based cleaning to isolate speaker dialogues.

    Default Data Loader + Recursive Character Text Splitter

      Splits long transcripts into smaller chunks for embedding.

    Embeddings (OpenAI)

      Converts text chunks into vector embeddings.

    Pinecone Vector Store

      Saves embeddings for later retrieval and semantic query answering.


🛠 Tech Stack

    n8n – Workflow automation

    OpenAI – Embedding generation

    Pinecone – Vector storage and retrieval
