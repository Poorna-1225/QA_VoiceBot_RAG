## QA Voice Bot RAG-Assistant

This application demonstrates how to build a voice assistant using LiveKit and OpenAI. It leverages Retrieval Augmented Generation (RAG) to provide contextually relevant answers to user questions. The assistant listens to user voice input, transcribes it to text, uses RAG to find relevant information from a knowledge base, and then generates a spoken response using OpenAI's language models.

### Setup

1. **Create a virtual environment:**
   ```bash
   conda create -m venv python>=3.9 -y
   ```

2. **Activate the virtual environment:**
   ```bash
   conda activate venv
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

### Workflow

1. **Data Preparation (`build_data.py`)**

   - This script processes your `raw_data.txt` file.
   - It breaks the text into smaller chunks (paragraphs).
   - It generates embeddings for each paragraph using OpenAI's embedding model. These embeddings are numerical representations that capture the meaning of the text.
   - It builds an Annoy index. This is a data structure that allows for efficient searching of similar embeddings (and therefore similar text chunks).
   - It saves the paragraphs and their corresponding identifiers in a file called `my_data.pkl`.

2. **Voice Assistant (`assistant.py`)**

   - This script starts your LiveKit-powered voice assistant.
   - It connects to a LiveKit room (you'll need a LiveKit server running).
   - It uses the following components:
     - **Silero VAD:**  Detects when you're speaking.
     - **Deepgram:** Transcribes your speech into text.
     - **OpenAI:**  Powers the language understanding and generation.
   - When you speak, the assistant:
     - Transcribes your speech to text.
     - Creates an embedding of your question/statement.
     - Uses the Annoy index (built by `build_data.py`) to find the most relevant paragraph from your `raw_data.txt`.
     - Sends your question and the relevant paragraph to OpenAI's language model.
     - Receives a response from OpenAI.
     - Converts the response to speech and plays it back to you.


**In essence, `build_data.py` prepares your knowledge base, and `assistant.py` uses that knowledge base to answer questions conversationally.**

