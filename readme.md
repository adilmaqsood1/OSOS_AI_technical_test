# Dr. X's Publications Analysis System

## Project Overview

This project aims to analyze the publications and documents left behind by Dr. X, a researcher who mysteriously disappeared. The system aims to uncover insights into Dr. X's research by providing a comprehensive analysis of their work. This involves processing various file formats (PDF, DOCX, Markdown), chunking the text into smaller, manageable units, creating vector embeddings to represent the semantic meaning of these chunks, and ultimately providing a Retrieval Augmented Generation (RAG)-based Q&A system to help users explore and understand Dr. X's research. 

## System Architecture and Workflow

The system follows a multi-stage workflow to achieve its goals:

1. **Data Ingestion and Preprocessing:**
   - **Input:** Various document formats, including PDF, DOCX, Markdown, and Excel files containing Dr. X's publications and notes.
   - **Preprocessing:**
     - Tabular data (e.g., Excel) is converted into Markdown for better handling by LLMs, as they excel at processing plain text.
     - Text extracted from different file formats undergoes cleaning and formatting adjustments.
     - The choice of Markdown for tabular data is strategic, as it preserves the relationships between rows and columns during subsequent chunking and embedding phases.
2. **Chunking:**
   - **Purpose:** Divide the preprocessed text into smaller, semantically meaningful chunks.
   - **Implementation:**
     - Utilizes libraries like `tiktoken`, `pdfplumber`, and `python-docx` to extract text from different file formats.
     - Splits the text based on logical boundaries like sections, paragraphs, and tables.
     - Chunks are typically limited to a specific token size (e.g., 512) to ensure efficient processing by the embedding model.
3. **Embedding Generation:**
   - **Purpose:** Create vector representations (embeddings) of each text chunk to capture their semantic meaning.
   - **Implementation:**
     - Employs a pre-trained embedding model, such as `nomic-ai/nomic-embed-text-v1`, which is readily available through the Hugging Face Transformers library.
     - Each chunk is fed into the embedding model, and the resulting vector representation is stored.
4. **Vector Database Creation:**
   - **Purpose:** Store and index the generated embeddings for efficient retrieval.
   - **Implementation:**
     - Utilizes FAISS (Facebook AI Similarity Search), a library designed for efficient similarity search in high-dimensional spaces.
     - A FAISS index is built to store the embeddings, enabling fast retrieval of relevant chunks based on query similarity.
5. **RAG Q&A System:**
   - **Purpose:** Provide a user interface for querying Dr. X's research and receiving answers grounded in the original documents.
   - **Implementation:**
     - Employs a local Large Language Model (LLM) for natural language understanding and response generation. Two options are explored:
       - `llama-cpp`: A Python library for running LLaMA models locally. This option offers offline capabilities but might have limitations in terms of model size and performance.
       - `Groq`: An alternative implementation using the Groq platform and the `llama-3.3-70b-versatile` model, offering faster and more comprehensive responses while maintaining offline capabilities.
     - When a user asks a question, the system:
       - Embeds the question using the same model used for document chunking.
       - Queries the FAISS index to retrieve the most relevant chunks based on similarity to the question embedding.
       - Constructs a prompt that includes the question and the retrieved context.
       - Feeds the prompt to the LLM to generate an answer.
       - Presents the answer to the user, along with citations indicating the source documents and specific chunks used for generating the response.
6. **Translations:**
   - **Purpose:** Facilitate access to Dr. X's work for a wider audience by providing translations into different languages, specifically Arabic.
   - **Implementation:**
     - Utilizes the Groq platform's OpenAI API and the `llama3-70b-8192` model for translation.
     - Extracts text from PDF or DOCX files, translates it into Arabic, and renders the translated text into a new PDF file with proper formatting for Arabic script.
     - Addresses challenges of right-to-left languages and ensures readability using libraries like `arabic-reshaper` and `python-bidi`.
7. **Summarization:**
   - **Purpose:** Provide concise summaries of Dr. X's publications, capturing the main ideas and findings.
   - **Implementation:**
     - Utilizes the Groq platform's OpenAI API and the `llama3-70b-8192` model for summarization.
     - Offers three summarization strategies:
       - Default: Produces a bullet-point summary highlighting key takeaways.
       - Detailed: Provides a more comprehensive summary with in-depth analysis.
       - Headings: Creates a summary organized by section headings, offering concise overviews of each topic.
     - Evaluation: Employs ROUGE metrics to assess the quality of generated summaries against reference summaries if available.
     - Output: Generates summary PDFs for convenient access.


## Installation and Setup

1. **Clone the Repository:** `git clone https://github.com/adilmaqsood1/OSOS_AI_technical_test.git`
2. **Install Dependencies:** `pip install -r requirements.txt`
3. **Install Fonts:** `apt-get install -y fonts-dejavu` (for Arabic support)
4. **Configure API Key:** Store your Groq API key as an environment variable or in a user data file (`google.colab import userdata`).
5. **Organize Data:** Place your input files (Dr. X's documents) in the designated data directory.
6. **Run Notebook:** Execute the cells in the Colab notebook to process the data, generate embeddings, build the vector database, and start the Q&A system.

## Usage

1. **Start the Q&A System:** Run the main script or notebook section that initiates the RAG Q&A system.
2. **Ask Questions:** Enter your questions in natural language, and the system will retrieve relevant information from Dr. X's documents and generate answers.
3. **Translation:** Use the translation functionality to translate Dr. X's documents into Arabic and generate translated PDFs.
4. **Summarization:** Use the summarization functionality to obtain concise summaries of publications and generate summary PDFs.

## Limitations

- **Model Performance:** The accuracy and performance of the Q&A system depend on the chosen LLM and the quality of the input documents.
- **Computational Resources:** Local LLMs might require significant computational resources, especially for larger models. Consider using cloud-based options for improved performance.
- **Data Preprocessing:** The quality of data preprocessing directly affects the accuracy of the system. Inaccurate or incomplete text extraction can lead to incorrect or irrelevant responses.
- **Language Support:** The current system focuses on English and Arabic. Support for other languages might require additional translation and language processing components.
- **Model Bias:** LLMs can inherit biases present in their training data. Be aware of potential biases in generated responses and critically evaluate the information provided.

## Future Work

- **Improved Data Handling:** Implement more advanced text cleaning and formatting techniques to enhance data quality.
- **Enhanced Retrieval:** Explore alternative embedding models and retrieval algorithms to improve the accuracy and efficiency of information retrieval.
- **LLM Fine-tuning:** Fine-tune LLMs on Dr. X's research domain to enhance their understanding of the specific terminology and concepts relevant to the documents.
- **Multilingual Support:** Add support for more languages through integration with translation services or models.
- **User Interface Development:** Create a more interactive and user-friendly interface for exploring the documents, asking questions, and visualizing the results.
- **Knowledge Graph Integration:** Explore the possibility of building a knowledge graph from Dr. X's research to enable more structured and interconnected insights.

