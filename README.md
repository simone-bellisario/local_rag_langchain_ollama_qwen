This is an example of a simple question-answering assistant using LangChain. In this case I chose to use the EU AI Act to test it out, adapting the system prompt accordingly.

In a time where frontier models have massive context windows, there is till space for a RAG approach like this, beyond the cost. For example, in regulatory work it is important to have an overall transparency of how and what is retrieved, besides having the possibility of running everything locally. On top of that, a RAG pipeline allows to reduce or avoid the "lost in the middle" problem with have with long contexts in frontier models.

The pipeline loads the document with PyMuPDF4LLM and splits it with a semantic chunker so that chunks follow the document's meaning rather than a fixed character count. It embeds the chunks with all-MiniLM-L6-v2 and stores them in Chroma (for the sake of this example, I used it locally). At query time, it retrieves the top 15 candidates by vector similarity, then uses a bge-reranker-base cross-encoder to keep the best 3.
Those chunks go to Qwen2.5 1.5B, run fully locally through Ollama, with a prompt that nudges it towards answering only from the given context, cite articles etc.

Finally, the pipeline is evaluated with RAGAS on context precision, context recall, and faithfulness, to give an example of how to assess both retrieval quality and hallucination are measured rather than assumed.
