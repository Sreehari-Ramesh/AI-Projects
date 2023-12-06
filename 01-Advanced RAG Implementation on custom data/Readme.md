What is RAG?
Retrieval Augmented Generation (RAG) is an approach that integrates elements from information retrieval and natural language generation to enhance the quality and relevance of generated text. This is particularly beneficial in the realm of intricate language tasks such as question-answering, summarization, and text completion.

The primary objective of RAG is to improve the text generation process by capitalizing on the advantages of retrieval. This empowers the Natural Language Generation (NLG) model to produce responses that are more informed and contextually appropriate. By integrating pertinent information retrieved during the generation process, RAG aims to enhance the accuracy, coherence, and informativeness of the generated content.

Implementation of Advanced RAG Concepts:
We will now implement an advanced RAG pipeline, incorporating the following concepts:

Caching Embeddings:
Embeddings can be cached to avoid recomputing them, utilizing a CacheBackedEmbeddings. This wrapper caches embeddings in a key-value store, with the text being hashed and the hash serving as the key. The cache is implemented using the local file system for storing embeddings and the FAISS vector store for retrieval.

Example:

python
Copy code
store = LocalFileStore("./cache/")
Additionally, an in-memory cache for embeddings can be set up:

python
Copy code
store = InMemoryStore()
Note: In-memory caches are primarily useful for unit tests or prototyping and should not be used for actual storage of embeddings.

Hybrid Vector Search:
Hybrid Search combines keyword-style search and vector-style search. It utilizes the BM25 algorithm for keyword search, generating a sparse vector. BM25 is an information retrieval algorithm that ranks and scores document relevance based on a search query. It is an extension of the TF-IDF (Term Frequency-Inverse Document Frequency) approach.

Key points about BM25:

Term Frequency (TF): Measures the frequency of a term in a document.
Inverse Document Frequency (IDF): Measures the importance of a term based on its frequency across the entire document collection.
BM25 Weighting: Combines TF and IDF to calculate the relevance score of a document for a given query.
Query Terms: Considers the occurrence of query terms in the document and adjusts their scores accordingly.
Parameter Tuning: Involves tuning parameters (k1, b) to optimize ranking performance based on the dataset.
Semantic Search, on the other hand, aims to improve accuracy and relevance by understanding the context and meaning behind a search query. It uses FAISS for semantic search and BM25 for keyword search in a Hybrid Search implementation with langchainEnsembleRetriever.

The EnsembleRetriever takes a list of retrievers as input, combining their results using the Reciprocal Rank Fusion algorithm. By leveraging the strengths of different algorithms, the EnsembleRetriever achieves better performance than any single algorithm. This combination of a sparse retriever (BM25) and a dense retriever (embedding similarity) is commonly known as "hybrid search."

InMemoryCaching:
Caching occurs for every user query and response generated, provided the user query does not match a previously requested query.

Implementation Stack:

Embedder: BAAI general embedding
Retrieval: FAISS Vectorstore
Generation: Mistral-7B-Instruct GPTQ model
Infrastructure: Google Colab, A100 GPU
Data: Financial Documents





