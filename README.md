# Adaptive-RAG-with-Yahoo-Finance-and-Web-Search-and-Tracing

This project defines an adaptive Retrieval Augmented Generation (RAG) system using LangChain, LangSmith for tracing,  yfinance for stock information, and Tavily for web search.

# Flow:
![project_1](https://github.com/user-attachments/assets/a074ca20-4df1-45f6-8e95-c8bbbc16d517)


*1. Setup and Dependencies:*

* *Imports:* Imports necessary libraries like os, yfinance, components from langchain, langgraph, pydantic, IPython, and tavily.
* *Environment Variables:* Sets environment variables for LangSmith tracing (project name, API key), and the Tavily API key.  Crucially, you must replace the placeholder API keys with your own.
* *Pip Installs:* Installs required Python packages.  This is useful for ensuring a Colab environment has all the necessary libraries.
* *Tavily Client Initialization:* Initializes the Tavily client using the API key from the environment.

*2. Graph State Definition:*

* **GraphState Class:** Uses Pydantic's BaseModel to define the state of the RAG workflow.  It holds information like the user's query, retrieved documents, transformed query (if any), graded documents, and the final response.  This structured approach is essential for managing the flow of information through the graph.

*3. Node Definitions (Functions):*

This section defines functions that act as nodes in the execution graph. Each function represents a step in the RAG process:

* **web_search:** Uses the Tavily client to perform a web search based on the current query. It extracts title and URL and handles cases where no results are found or an error occurs.
* **yahoo_finance_search:**  Fetches stock information from Yahoo Finance.  It extracts the ticker symbol from the user's query, retrieves the latest closing price using yfinance, and returns the information or an error message.
* **retrieve:** Simulates retrieving documents from a vector database.  In a real application, this would interact with a vector store like ChromaDB.
* **grade_documents:** Filters the retrieved documents (at least 2).  In a real system, this would be more sophisticated, using relevance scoring or other criteria.
* **generate:** Generates the final response based on the graded documents.
* **transform_query:**  Transforms the query (e.g., adding keywords). This is currently a placeholder; a more sophisticated implementation would rephrase or augment the original query.


*4. Routing Logic:*

* **route_question:**  Decides which tool (web search, vector store, or Yahoo Finance) to use based on keywords in the user's query.
* **decide_to_generate:** Determines whether to generate a response immediately or to transform the query further based on the number of graded documents.
* **grade_generation_v_documents_and_question:** Evaluates the generated response to decide whether the workflow should terminate or continue.  Currently, it forces the workflow to end.

*5. Workflow Definition:*

* **workflow:** Creates a StateGraph object to manage the execution flow.
* *Adding Nodes:* Adds the functions defined earlier as nodes in the graph.
* *Adding Edges (Conditional and Unconditional):* Defines the connections between nodes using the add_edge and add_conditional_edges methods. The conditional edges dynamically determine the next step in the graph based on the routing logic functions.
* **graph_4_adaptiverag:** Compiles the workflow graph.


*6. Graph Visualization:*

* Attempts to display a visualization of the compiled workflow graph using Mermaid.js.

*7. Interactive Query Execution:*

* **run_adaptive_rag:** Executes the workflow graph with a given user query.
* *Main Loop:* Prompts the user for queries and prints the responses.



*8. Tracing with LangSmith:*

This part uses LangSmith to trace the execution of the RAG pipeline for several queries. It helps in monitoring and debugging the pipeline's behavior and performance. Remember to set up your LangSmith API key correctly.

*Key Improvements and Considerations:*


* *Robust Error Handling:* The code includes basic error handling, but it could be improved. More comprehensive error handling would gracefully handle unexpected situations, provide more informative error messages, and possibly provide fallback mechanisms.
* *Vector Database Integration:* Replace the placeholder retrieve function with actual vector database interaction.
* *Sophisticated Query Transformation:* Implement a better query transformation technique instead of just appending text.
* *Document Grading:* Develop a more advanced document grading mechanism using relevance scores, embeddings, or other metrics.
* *Response Generation:* Improve the response generation based on the graded documents.  Instead of just concatenating them, use a language model to create a concise and informative summary.


This explanation provides a detailed breakdown of the code. Remember to adapt the API keys and potentially other parts to fit your environment and needs.
