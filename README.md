# **README: Retrieval-Augmented Generation (RAG) System**

## **Project Overview**
This project implements a Retrieval-Augmented Generation (RAG) system that assists users by answering questions related to a device’s user manual. The system retrieves relevant sections from the manual and generates answers using a pre-trained large language model (LLM). It evaluates the system's performance based on metrics like **latency**, **precision**, **recall**.

---

## **Approach**

1. **Text Extraction**:  
   The system extracts text from a user manual PDF using the **PyPDF2**  library. The text is then divided into manageable chunks for retrieval.

2. **Text Chunking**:  
   The extracted text is chunked into smaller sections using Recursive Character Splitting. This chunking method helps manage retrieval accuracy by breaking the manual into more accessible parts.

3. **Sentence Transformer**  
   Sentence Transformer of HuggingFace vectorization to convert both the query and text chunks into numerical vectors. This allows us to compute cosine similarity between the user's query and the text chunks to identify relevant sections of the manual.

4. **Retrieval of Relevant Sections**:  
   Based on similarity scores, the system retrieves the top 6 most relevant sections from the manual that match the user's query.

5. **Response Generation**:  
   The system then combines the relevant sections into a context and sends it, along with the query, to the **Gemini 1.5-Flash API** (or any other LLM). The LLM generates the final answer based on the context retrieved from the manual.

6. **Performance Evaluation**:  
   The system measures performance using:
   - **Latency**: The time taken to retrieve the answer.
   - **Precision and Recall**: For evaluating the relevance of retrieved sections.

---

## **How to Run the Code**

### **Requirements**
Make sure to have the required Python libraries installed.
Make sure to upload PDF document.
Make sure to have Gemini API key.

**Run Code**

```bash
python rag_system.py
```

## **Assumptions**

- **Text Chunking**: The text from the manual is split by paragraphs using `\n\n` for simplicity. This method assumes that paragraphs are a meaningful division for chunking. For more complex documents, a more sophisticated chunking mechanism might be necessary to capture context and improve retrieval accuracy.

 - **PDF Content**: It is assumed that the input PDF i.e knowledge_doc.pdf contains text that is primarily informational and relevant to the types of questionn asked by the users.
   
 - **Embedding Size**: To create vector embeddings, HuggingFace's all-MiniLM-L6-v2 model is employed. Although this lightweight model can be used in a variety of situations, it might not be able to capture particularly complicated texts.


- **Precision & Recall**: In the provided code, dummy values are used for true and predicted labels for precision and recall calculations. This approach assumes that these dummy values provide a basic illustration of performance metrics. In practice, you would need real labeled data to accurately compute precision and recall.

## **Challenges Encountered**

1. **Latency Optimization**:  
   The system’s latency can increase with the size of the user manual and complexity of the query. Optimizing text retrieval and vectorization processes is crucial for reducing latency. The current implementation uses straightforward methods, which may need optimization for handling larger manuals and improving response times.

2. **API Rate Limits**:  
   Models like Open API , Cohere enforces rate limits and usage quotas. During performance testing or deployment, hitting these limits can impact evaluation and usage. It is important to manage API usage and monitor quotas to avoid disruptions, hence a model with lower efficency is used to avoid it.Morever, current model also has 15 request per minute rate limit and thus require exception handling.

3. **Handling Complex Queries**:  
   The system is designed to retrieve information based solely on the manual. Complex queries that require synthesizing information from multiple sections or additional knowledge may not be handled effectively. Enhancements might be needed to address such complex queries, including incorporating external knowledge sources or advanced retrieval techniques.



