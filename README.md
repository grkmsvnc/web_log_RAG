# RAG-Powered_LogQA
## Overview

This project develops a Question-Answering (Q&A) system that leverages web traffic logs to answer user queries. The system is built on the **Retrieval-Augmented Generation (RAG)** model, which retrieves relevant information from traffic logs and generates accurate responses to user questions.

You can use this project with either synthetic log data, generated by the script provided, or your own web traffic logs.

## Key Features

- **Synthetic Log Generation**: A script is provided to generate realistic, diverse web traffic logs using the `Faker` library. The generated logs simulate various user interactions, including geographic locations, IP addresses, HTTP methods, URLs, browser user agents, referrers, and session information.
- **Retrieval-Augmented Generation (RAG)**: The Q&A system retrieves relevant information from the log data and generates suitable answers to user queries based on those logs.

## Prerequisites

Before running the code, ensure you have the required dependencies installed. You can install them using the `requirements.txt` provided in the repo:

```bash

pip install -r requirements.txt

```

## Usage

### 1. Generating Synthetic Web Traffic Logs

If you do not have access to your own web traffic logs, you can generate synthetic logs using the provided script. This script simulates real-world user interactions on a website and generates logs in a format similar to those used by Apache or Nginx servers.

To generate synthetic log data:

```bash

python synthetic_data.py

```

By default, this will generate 10,000 log entries and save them to `diverse_synthetic_web_traffic.log`. You can modify the number of entries as needed by passing a different value to the `num_entries` parameter.

### 2. Using Your Own Log Data

If you already have your own web traffic logs, you can skip the synthetic log generation and proceed directly to using the logs with the Q&A system. Ensure that your logs are formatted correctly (e.g., Apache/Nginx log format) so that they can be properly parsed and analyzed by the system.

### Example Log Entry

Here’s an example of a synthetic log entry generated by the script:

```arduino

192.168.56.12 - 5a1fcf10-e89b-11eb-9a03-0242ac130003 [19/Aug/2023:10:15:32 +0000] "GET /product HTTP/1.1" 200 12345 "https://google.com" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36" Cookie="5f4dcc3b5aa765d61d8327deb882cf99"

```

This log entry includes details like:

- IP Address
- Session ID
- Timestamp
- HTTP Method and URL
- Status Code
- Referrer URL
- User Agent
- Cookie Information

## File Structure

- `synthetic_data.py`: Script for generating synthetic log data.
- `requirements.txt`: List of dependencies required for the project.
- `diverse_synthetic_web_traffic.log`: Example of synthetic log data (if generated).

## Log Data Cleaning and Preparation (cleaning_editing.py)

The `cleaning_editing.py` script is designed to clean and prepare the log data for further processing and analysis. After generating or importing the log data, this script ensures that the data is of high quality, free from errors, inconsistencies, and missing values. Here's an overview of what this script does:

### Key Features:

1. **Parsing Log Data**:
    - The script reads log entries from the file and parses each entry into individual components (IP, session ID, timestamp, method, URL, status code, etc.).
2. **Data Cleaning**:
    - Handles missing or invalid data, including IP addresses, timestamps, and HTTP methods.
    - Validates HTTP status codes, request sizes, and ensures the log data is consistent and correct.
3. **Data Enrichment**:
    - Extracts additional insights from the data, such as the hour and day from the timestamp.
    - Converts request sizes to megabytes for easier analysis.
4. **Outlier Detection**:
    - Identifies and removes outliers based on the request size (in MB) using Z-scores.
5. **Standardization**:
    - Ensures consistency by converting HTTP methods to uppercase.
6. **Reporting**:
    - Reports missing values and inconsistencies in the data after cleaning.

### How to Use

To use the `cleaning_editing.py` script, follow these steps:

1. **Prepare the Log Data**:
    - Ensure you have generated or imported log data in the proper format. You can use either the synthetic logs from `synthetic_data.py` or your own logs.
2. **Run the Script**:
    - The script reads the log data from a file, cleans and validates it, and then saves the cleaned data in both CSV and Excel formats.
    
    ```bash
    
    python cleaning_editing.py
    
    ```
    
3. **Check the Cleaned Data**:
    - After the script runs, you will find two new files in your project directory:
        - `high_quality_log.csv`
        - `high_quality_log.xlsx`
    - These files contain the cleaned and validated log data, ready for further analysis or use in the Q&A system.

### Example of the Cleaning Process:

- **IP Validation**: Ensures all IP addresses are valid and correctly formatted.
- **Timestamp Validation**: Converts timestamps into proper datetime format, removing invalid entries.
- **HTTP Method Validation**: Filters out invalid or non-standard HTTP methods, such as GET, POST, PUT, DELETE, etc.
- **Outlier Removal**: Request sizes are evaluated, and any outliers (based on Z-scores) are removed to ensure data quality.

### Additional Features:

- **Extracting Hour and Day**: The script enriches the data by extracting the hour of the day and the day of the week from each log entry, allowing for temporal analysis.
- **Missing Value Handling**: Missing values in critical fields (like IP address, timestamp, and URL) are removed, while missing values in non-critical fields (such as referrer and user agent) are replaced with placeholders.

### File Structure

- `cleaning_editing.py`: Script for parsing, cleaning, and validating the log data.
- `high_quality_log.csv`: Cleaned log data in CSV format.
- `high_quality_log.xlsx`: Cleaned log data in Excel format.

## Log Data Analysis and Visualization (data_analysis.py)

The `data_analysis.py` script is designed to provide deeper insights into the cleaned web traffic logs through data analysis and visualizations. This script allows you to better understand traffic patterns, user behavior, and system performance by analyzing various attributes of the log data, such as HTTP methods, visited URLs, IP addresses, and more.

### Key Features:

1. **HTTP Method Distribution**:
    - Visualizes the distribution of HTTP methods (GET, POST, etc.) used by users to interact with the website.
2. **Top 10 Most Visited URLs**:
    - Identifies and visualizes the most frequently visited pages on the website.
3. **Top 10 Most Frequent IP Addresses**:
    - Displays the top 10 IP addresses generating the most traffic, providing insights into potential high-traffic users or bots.
4. **Traffic Distribution by Hour**:
    - Analyzes website traffic by the hour of the day, helping identify peak traffic times.
5. **Traffic Distribution by Day**:
    - Shows the distribution of traffic over the days of the week to understand which days see the most activity.
6. **Top 10 Most Used Browsers (User-Agent)**:
    - Lists and visualizes the most common user agents (browsers and devices) used to access the website.
7. **Top 10 Referring Sources (Referrers)**:
    - Displays the top referrer URLs driving traffic to the website, helping you understand where users are coming from.

### How to Use

1. **Prepare the Cleaned Data**:
    - Ensure that you have run the `cleaning_editing.py` script to generate the cleaned log data. The cleaned data should be saved as an Excel file (`high_quality_log.xlsx`).
2. **Run the Data Analysis Script**:
    - The script reads the cleaned log data and performs various analyses. It generates visualizations that provide insights into user behavior and traffic patterns.
    
    ```bash
    
    python data_analysis.py
    
    ```
    
3. **Visual Output**:
    - The script will output several visualizations, including bar plots and histograms, that summarize the key metrics of your website's traffic.

### Detailed Analysis:

1. **HTTP Method Distribution**:
    - The script counts and plots the distribution of HTTP methods (GET, POST, etc.). This helps identify which types of requests are most commonly made by users.
      
      ![http](https://github.com/user-attachments/assets/7bdee407-13ab-47b3-9ad8-bf518eacee17)

2. **Top 10 Most Visited URLs**:
    - The most frequently visited URLs are identified and displayed in a bar plot, helping you understand which pages are most popular among users.
      
     ![url](https://github.com/user-attachments/assets/a0eb73b7-afab-4ec0-b0c2-91c389473589)
 
3. **Top 10 Most Frequent IP Addresses**:
    - A bar plot shows the IP addresses that generate the most traffic, allowing for easy identification of high-traffic users or potential malicious activity.
      
      ![ip](https://github.com/user-attachments/assets/eb7aeb71-fb2d-4440-b095-32845039bca2)

4. **Traffic Distribution by Hour**:
    - This bar chart visualizes how traffic is distributed throughout the day, helping to identify peak traffic times and plan for resource allocation.
      
      ![trafik_hour](https://github.com/user-attachments/assets/c502bcf4-eecd-47ce-afb0-d67ced452270)

5. **Traffic Distribution by Day**:
    - The traffic distribution by day is shown, revealing patterns about which days of the week see the most traffic.
      
      ![trafik_day](https://github.com/user-attachments/assets/d9cf5c22-af7a-4476-b6cf-fb0606ce626d)

6. **Top 10 Most Used Browsers (User-Agent)**:
    - The most common browsers and devices used by visitors are displayed, helping you optimize your site for the most popular platforms.
      
7. **Top 10 Referring Sources (Referrers)**:
    - The top referral sources driving traffic to your website are displayed in a horizontal bar chart. This can be useful for understanding your website's marketing effectiveness or social media impact.
      
      ![refer](https://github.com/user-attachments/assets/ae173238-2e4b-4c82-bcb7-1e50aa2ca15d)


### Example Output

Here are examples of the insights you can expect:

- **HTTP Method Distribution**:
    
    A bar plot showing the proportion of each HTTP method (e.g., GET, POST) used in the traffic logs.
    
- **Top 10 Visited URLs**:
    
    A bar plot showing the top 10 most visited pages on your website, helping you understand which content is most engaging.
    
- **Traffic Distribution by Hour**:
    
    A bar chart showing when users are most active on your site, allowing you to identify peak traffic times and optimize performance during those hours.
    

### File Structure

- `data_analysis.py`: Script for performing data analysis and generating visualizations from the cleaned log data.
- `high_quality_log.xlsx`: Cleaned log data (generated from the `cleaning_editing.py` script) used as input for analysis.

## Embedding Creation and Vector Storage (embedding_vectorstorage.py)

The `embedding_vectorstorage.py` script is designed to transform the cleaned log data into embeddings and store them efficiently for fast retrieval using **FAISS**. This step is crucial for the question-answering system, as it allows for efficient search and retrieval of relevant log entries based on user queries.

### Key Features:

1. **Textual Feature Embedding**:
    - Extracts textual features (URLs, referrers, user agents, HTTP methods, cookies, and timestamps) and converts them into dense vector representations using a **Sentence-Transformer** model.
2. **Numerical and Categorical Embedding**:
    - Normalizes numerical features (hour, status, request size) and one-hot encodes categorical features (day of the week) to create embeddings for these attributes.
3. **Combining Embeddings**:
    - Merges textual embeddings with numerical and categorical embeddings to form a final representation of each log entry.
4. **FAISS Vector Storage**:
    - Utilizes **FAISS** to store these embeddings, allowing for efficient vector-based similarity searches. This enables fast and scalable retrieval of the most relevant log entries based on user queries.
5. **Querying the FAISS Index**:
    - The script includes an example of how to query the FAISS index using an embedding (e.g., the first log entry) and retrieve the top-k nearest neighbors (log entries).

### How to Use

1. **Prepare the Cleaned Data**:
    - Ensure that you have cleaned log data available in the Excel format (`high_quality_log.xlsx`). This is the input for the embedding creation and storage process.
2. **Run the Embedding and Storage Script**:
    - The script processes the cleaned log data to generate embeddings and stores them in a FAISS index for efficient retrieval.
    
    ```bash
    
    python embedding_vectorstorage.py
    
    ```
    
3. **Check the Output**:
    - After running the script, you will find the following files in your project directory:
        - `final_embeddings.npy`: The final embeddings generated from the log data.
        - `faiss_index.bin`: The FAISS index containing the stored embeddings for efficient retrieval.

### Detailed Steps:

1. **Text Feature Extraction and Embedding**:
    - The script extracts key textual features such as URL, referrer, user agent, HTTP method, and timestamp. These features are combined into a single string for each log entry, which is then embedded using a **Sentence-Transformer** model (e.g., `all-mpnet-base-v2`).
2. **Numerical and Categorical Feature Preparation**:
    - Numerical features (hour, status code, request size) are normalized using `StandardScaler`, while the day of the week is one-hot encoded to represent categorical data.
3. **Embedding Combination**:
    - The textual embeddings are combined with the numerical and categorical embeddings to form the final log entry representation. These combined embeddings serve as the input for the FAISS index.
4. **FAISS Index Creation**:
    - The final embeddings are added to a FAISS index using **L2 (Euclidean) distance** for fast similarity search. The FAISS index allows for efficient querying of log entries based on their vector representations.
5. **Querying the FAISS Index**:
    - The script includes an example query using the first log entry's embedding to search for the top 5 nearest neighbors in the FAISS index. It returns the indices and distances of the most similar log entries.

### Example Output:

- **Final Embeddings Shape**: The shape of the final combined embeddings is printed to ensure the correct size of the vector representation.
    
    ```bash
    
    Final embeddings shape: (10000, 768)
    
    ```
    
- **FAISS Index Population**: The total number of vectors added to the FAISS index is printed.
    
    ```bash
    
    Total of 10000 vectors added to FAISS.
    
    ```
    
- **Nearest Neighbor Search**: After querying the FAISS index, the indices and distances of the nearest neighbors are displayed. The closest log entry is printed for reference.
    
    ```bash
    
    Indices of nearest vectors found: [[0 1 2 3 4]]
    Distances to the nearest vectors: [[0.000000 0.250123 0.300321 ...]]
    Closest log entry: (Details of the log entry)
    
    ```
    

### File Structure

- `embedding_vectorstorage.py`: Script for generating embeddings from log data and storing them in a FAISS index for efficient vector-based search.
- `final_embeddings.npy`: Saved embeddings from the log data.
- `faiss_index.bin`: FAISS index with stored embeddings for fast retrieval.


## Log Querying with Llama3-70B Model and Groq API (llama3_70b_groq.py)

In **Retrieval-Augmented Generation (RAG)** systems, security is a critical concern, especially when handling sensitive documents and logs related to companies and organizations. Due to these concerns, large language models such as **OpenAI's GPT models**, **Claude**, and **Gemini** have not been used in this system to avoid potential security risks associated with API calls to third-party services. Instead, by leveraging sufficient **GPU resources**, the **Llama3-70B** model can be run **locally**, ensuring full control over data security and eliminating the need to share sensitive information with external providers.

### Why Llama3-70B?

![Ekran Görüntüsü (441)](https://github.com/user-attachments/assets/076cdf20-3468-459e-8124-fcabcd9a6786)


The **Llama3-70B** model was selected as the best open-source model for this system because of its high performance, achieving a score of **0.95**. Although it tied with **Alibaba's qwen2-72b-instruct**, Llama3-70B was chosen due to qwen2-72b-instruct producing 60% longer responses, which could lead to increased costs and potentially slower response times. Hence, Llama3-70B provides an optimal balance between accuracy and efficiency.

### Key Features:

1. **FAISS Index Querying**:
    - The script queries a **FAISS** index to retrieve the closest log entries based on embeddings generated from user queries. The logs are retrieved in vector form, allowing for quick, scalable searches.
2. **Embedding Combination**:
    - The query embeddings are dynamically generated from the user's text input and combined with numerical features (e.g., hour, status, request size) and one-hot encoded categorical data (e.g., day of the week). This ensures that the search is highly contextual and accurate.
3. **Groq API Integration**:
    - The script utilizes the **Groq API** to generate detailed, human-readable responses based on the logs retrieved from FAISS. The response not only addresses the user query but also analyzes the log data to identify trends and issues.
4. **Advanced Model (Llama3-70B)**:
    - **Llama3-70B** is used to generate comprehensive answers, making use of the log data to provide insights and detect problems in the system.

### How to Use

1. **Prepare FAISS Index and Log Data**:
    - Ensure that the FAISS index and cleaned log data are available. The FAISS index should be stored in a file (e.g., `faiss_index.bin`), and the log data should be loaded from an Excel file (`high_quality_log.xlsx`).
2. **Run the Log Query Script**:
    - The script allows users to input a query (e.g., "Are there any errors in the system?"), which is then processed to search the FAISS index and generate a response using **Groq API** and **Llama3-70B**.
    
    ```bash
    
    python llama3_70b_groq.py
    
    ```
    
3. **Check the Output**:
    - After running the script, the system will output the nearest log entries and generate a detailed response to the user query, providing valuable insights into the system’s behavior.

### Detailed Steps:

1. **Loading the FAISS Index and Log Data**:
    - The FAISS index is loaded from a file (`faiss_index.bin`), and the log data is read from an Excel file (`high_quality_log.xlsx`).
2. **Query Embedding Creation**:
    - The user query is transformed into a dense vector representation using a **Sentence-Transformer** model. This embedding is combined with numeric and one-hot encoded features (hour, status code, request size, day of the week) to match the FAISS index.
3. **FAISS Search**:
    - The FAISS index is queried to retrieve the closest log entries based on the query embedding. This ensures that the most relevant logs are returned for further analysis.
4. **Groq API Response Generation**:
    - Once the relevant logs are retrieved, the **Groq API** is used to generate a detailed response using the **Llama3-70B** model. The response includes an analysis of the log entries and addresses the user's question directly, identifying potential issues or patterns.


## Enhanced Log Querying with Llama3-70B and Groq API (llama3_70b_groq_advanced.py)

The `llama3_70b_groq_advanced.py` script builds upon the previous implementation to enhance both performance and user experience. The main focus of this advanced version is to allow users to specify more detailed parameters for their queries, leading to more refined and relevant results in the **Retrieval-Augmented Generation (RAG)** system.

### Key Enhancements:

1. **User-Specific Query Refinement**:
    - Users are now prompted to provide more granular details for their queries, including:
        - The hour of the day to analyze traffic.
        - Specific HTTP status codes (e.g., 200, 404) to focus on certain types of responses.
        - The request size in megabytes (MB) for handling payload-related questions.
        - The day of the week to filter logs by temporal context.
        - The number of nearest log entries to retrieve from the FAISS index.
    
    These enhancements allow for more targeted searches, resulting in more precise and meaningful results.
    
2. **Improved User Interface**:
    - The script introduces a more user-friendly, interactive command-line interface (CLI). Users are guided through the process step-by-step, allowing them to refine their queries or exit the system when needed.
3. **Dynamic Query Processing**:
    - The query embedding is dynamically generated based on the user's input, ensuring that both textual and numerical features (such as hour, status, and size) are combined effectively to match the FAISS index and return the most relevant log entries.
4. **Enhanced Log Retrieval**:
    - Similar to the previous version, this script uses the **FAISS index** to search for the nearest log entries. However, users now have more control over how many entries to retrieve, making the analysis more flexible.

### How to Use

1. **Run the Log Query Script**:
    - The script prompts the user for a query and additional parameters, allowing for a more interactive and customized experience. Users can specify various query details or leave them blank to use default values.
    
    ```bash
    
    python llama3_70b_groq_advanced.py
    
    ```
    
2. **Follow the Interactive Prompts**:
    - The user-friendly system will guide you through specifying:
        - Query text (e.g., "Are there any errors in the system?")
        - Query hour (default: 12)
        - HTTP status code (default: 200)
        - Request size in MB (default: 0.01)
        - Day of the week (default: Monday)
        - Number of nearest log entries to retrieve (default: 5)
3. **Check the Output**:
    - After processing the query, the system retrieves the relevant log entries and uses the **Groq API** with **Llama3-70B** to generate a detailed, task-specific response.

### File Structure

- `llama3_70b_groq_advanced.py`: Advanced version of the script for querying the FAISS index and generating responses using **Groq API** with more user-specific query options.
- `faiss_index.bin`: FAISS index containing stored log embeddings.
- `high_quality_log.xlsx`: Log data used for FAISS search and analysis.

## Enhanced Local Log Querying with Quantized Meta-Llama Model (llama3_1_8b_local_gpu.py)

In this implementation, we enhance the **Retrieval-Augmented Generation (RAG)** system by running a **locally-hosted model**, specifically **Meta-Llama-3.1-8B-Instruct**. This setup provides strong data security, as no external API is needed, and ensures that sensitive information from logs is not shared with third-party services. The model is **quantized** to reduce memory usage while maintaining high performance, enabling it to run efficiently on local hardware.

### Why Meta-Llama-3.1-8B-Instruct?
![Ekran Görüntüsü (442)](https://github.com/user-attachments/assets/349db796-422e-44ca-a412-49a4de4d54cb)


The **Meta-Llama-3.1-8B-Instruct** model was selected based on experimental results, where it performed better than other versions in the 8B series. This model provided more accurate responses, making it ideal for the **log analysis** tasks involved in this system.

In addition, according to research conducted by [**Rungalileo.io**](https://www.rungalileo.io/), this model has surpassed several recent large models like **Snowflake's Arctic**, achieving a score of **0.89**. It is an ideal choice for smaller-scale applications that require strong inference performance without the heavy memory demands of larger models like Llama-70B.

This model allows us to leverage **quantized 4-bit precision**, which significantly reduces memory usage and computation while still providing high-quality responses. This makes it possible to run the model locally without requiring extensive GPU resources, ensuring full control over the data pipeline and enhancing system privacy.

### Key Enhancements in This Version:

1. **Local Model for Data Security**:
    - The system now operates entirely offline with the **Meta-Llama-3.1-8B-Instruct** model, ensuring that sensitive logs are never shared with external services. This approach maximizes data privacy and security.
2. **Quantized Model for Lower Memory Usage**:
    - The model is **quantized to 4-bit precision**, reducing memory and computational requirements without sacrificing much in terms of accuracy. This allows the system to be deployed on local hardware with more modest resources.
3. **Same FAISS Integration for Log Retrieval**:
    - **FAISS** is still used to retrieve the nearest log entries based on vector embeddings, ensuring efficient and fast search functionality.
4. **Improved Log Analysis**:
    - Using the **Meta-Llama-3.1-8B-Instruct** model, the system can generate detailed and accurate responses to user queries, analyzing logs and identifying key patterns or issues.

### How to Use

1. **Run the Log Query Script**:
    - The system allows users to input a query (e.g., "Why did the system return an error?"), and it processes the query to retrieve relevant log entries from the FAISS index. The retrieved logs are then analyzed by the **Meta-Llama-3.1-8B-Instruct** model to generate a detailed response.
    
    ```bash
    
    python llama3_1_8b_local_gpu.py
    
    ```
    
2. **Follow the Interactive Prompts**:
    - Users can specify details like the hour of the day, HTTP status codes, request size, and the day of the week, allowing for a more tailored query and analysis.
3. **Check the Output**:
    - After processing the query, the system retrieves the most relevant log entries and uses the **Meta-Llama-3.1-8B-Instruct** model to generate a detailed response, offering insights into system behavior, errors, or patterns detected in the logs.

### Model Selection and Performance:

- **Meta-Llama-3.1-8B-Instruct** was chosen based on internal experiments, where it outperformed other models in its class for log analysis tasks. The model offered better accuracy and more relevant answers.
- According to [**Rungalileo.io**](https://www.rungalileo.io/), this model has outperformed many larger models like **Snowflake's Arctic**, achieving a competitive score of **0.89**.
- **Quantized at 4-bit precision**, this model significantly reduces the memory footprint while maintaining high performance. This allows for local deployment without requiring extensive computational resources, making it suitable for smaller-scale use cases where data privacy and security are critical.

### File Structure

- `llama3_1_8b_local_gpu.py`: Script for querying the FAISS index with a user query, retrieving log entries, and generating a response using **Meta-Llama-3.1-8B-Instruct** running locally.
- `faiss_index.bin`: FAISS index containing stored log embeddings.
- `high_quality_log.xlsx`: Log data used for FAISS search and analysis.

## User-Friendly Advanced Log Analysis System with Meta-Llama (llama3_1_8b_local_gpu_advanced.py)

This version of the **Retrieval-Augmented Generation (RAG)** system enhances the user experience with a **command-line interface (CLI)** and introduces advanced query features. Using the **Meta-Llama-3.1-8B-Instruct** model, which is **quantized** for lower memory usage, the system runs locally, ensuring maximum data security and efficiency.

### Key Features:

1. **Advanced Query System**:
    - This version features an interactive CLI that allows users to input queries with additional parameters such as hour, HTTP status code, request size, day of the week, and the number of log entries to retrieve. If left blank, default values are used, making it easier for users to interact with the system.
2. **Local Model for Data Security**:
    - The system runs the **Meta-Llama-3.1-8B-Instruct** model locally, ensuring that no sensitive log data is shared with external services. The model is **quantized to 4-bit precision**, reducing memory requirements while maintaining strong performance.
3. **Efficient Log Retrieval with FAISS**:
    - The system uses **FAISS** to efficiently retrieve the nearest log entries based on vector embeddings generated from the user’s query. This ensures fast, scalable, and accurate log searches.
4. **Detailed Log Analysis**:
    - Once the relevant logs are retrieved, they are analyzed by the **Meta-Llama-3.1-8B-Instruct** model, which generates a detailed response based on the user’s query. This response includes insights into patterns, trends, and potential issues in the system logs.

### How to Use

1. **Run the Advanced Log Analysis System**:
    - The system accepts user queries and additional parameters through an interactive CLI. Users can customize their queries by specifying parameters such as hour, status code, request size, and day of the week.
    
    ```bash
    
    python llama3_1_8b_local_gpu_advanced.py
    
    ```
    
2. **Follow the Prompts**:
    - The CLI will prompt you to enter a query and, if desired, specify parameters. If a parameter is left blank, default values will be applied to keep the process simple and intuitive.
3. **Check the Results**:
    - After processing the query, the system retrieves the relevant log entries and generates a response using the **Meta-Llama-3.1-8B-Instruct** model. The output includes detailed insights based on the logs and user query.

### File Structure

- `llama3_1_8b_local_gpu_advanced.py`: Main script for the advanced log analysis system using the CLI.
- `faiss_index.bin`: FAISS index containing log embeddings.
- `high_quality_log.xlsx`: Log data used for FAISS search and analysis.

### Summary

The **llama3_1_8b_local_gpu_advanced.py** script enhances both user interaction and system performance. By combining a powerful, locally hosted **Meta-Llama-3.1-8B-Instruct** model with an advanced query system, this version provides secure, efficient, and detailed log analysis tailored to user input.
