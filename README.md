**Syllabus Vector Creation Workflow — n8n**

**Overview**

This n8n workflow automates the process of:

1. Listening for new syllabus-related documents uploaded to a Google Drive folder.

2. Extracting and chunking text from PDF, TXT, or DOCX formats.

3. Converting the text into embeddings using OpenAI.

4. Storing the embeddings in a Pinecone vector store for later Retrieval-Augmented Generation (RAG) queries.

**Workflow Steps**

1. Google Drive Trigger Node: Google Drive Trigger

    Purpose: Watches a specific Google Drive folder for newly created files.

    Trigger Event: fileCreated

2. Download FileNode: Download File

    Purpose: Downloads the newly uploaded document from Google Drive for processing.

3. File Type Switch Node: Switch

    Purpose: Routes the file to the appropriate extraction path based on its format.

    Routes:

        PDF → Extract from PDF

        TXT → Extract from TXT

        DOCX → Convert to DOC → Get Document

4. File Extraction and Chunking

   PDF Files
    - Extract from PDF → Extracts raw text.

    - Paragraph Chunking → Splits text into paragraph-level chunks.

    TXT Files
    - Extract from TXT → Extracts raw text from .txt file.

    - Chunking → Manual text chunking.

    DOCX Files
    - Convert To DOC → Converts DOCX to DOC format to extract text which is one of the simple way to process DOCX files

    - Get Document → Retrieves the document text.

    - Chunking by Paragraph → Splits into chunks by paragraph.

5. Split Out

    - Node: Split Out

    - Purpose: Takes the list of chunked text segments and splits them into individual items.

    - Why: This ensures each text chunk is processed independently in the embedding creation step, allowing OpenAI and Pinecone to handle them one by one.

6. Generate Embeddings

    - Node: Embeddings OpenAI

    - Purpose: Converts each text chunk into a vector embedding using OpenAI’s embedding model.

7. Store in Pinecone Vector Store

    - Node: Pinecone Vector Store

    - Purpose: Saves embeddings into Pinecone for future RAG queries.

8. Cleanup

    - IF DOC Node: Checks whether the document exists after processing.

    - Delete DOC Node: Removes the file from Google Drive to prevent reprocessing.
  
    - Purpose: The DOC file created from the DOCX file to be removed via workflow to prevent cluttering and reprocessing.
