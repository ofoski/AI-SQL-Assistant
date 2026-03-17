# 🎵 AI SQL Assistant (Chinook)

[![Python](https://img.shields.io/badge/python-blue.svg)](https://www.python.org/downloads/release/python-310/)
![Build](https://github.com/ofoski/RAG-SQL-assistant/actions/workflows/python-app.yml/badge.svg)

### 🔍 At a glance
- **Problem:** Translating natural-language questions into correct SQL queries  
- **Solution:** Schema-aware Retrieval-Augmented Generation (RAG) pipeline using SentenceTransformers embeddings and FAISS similarity search over SQLite database schemas  
- **Outcome:** 100% execution accuracy on a curated evaluation set of reference SQL queries  
- **Stack:**  
![RAG](https://img.shields.io/badge/RAG-0F766E)
![GenAI](https://img.shields.io/badge/GenAI-7C3AED)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?logo=openai&logoColor=white)
![SentenceTransformers](https://img.shields.io/badge/SentenceTransformers-3B82F6)
![FAISS](https://img.shields.io/badge/FAISS-005571)
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?logo=streamlit&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-003B57?logo=sqlite&logoColor=white)

A schema-aware **Retrieval-Augmented Generation (RAG)** SQL assistant that converts natural-language questions into executable SQL queries and executes them directly against a SQLite database.

## ✨ Features
- 🔎 **Natural Language → SQL** conversion  
- 🧠 **Schema-Aware Retrieval** via FAISS similarity search over SentenceTransformers embeddings  
- 📊 **Interactive UI** built with Streamlit  
- 🌍 **Dataset Flexibility** – supports SQLite datasets downloaded via KaggleHub  
- 🧪 **Evaluation Framework** with prompts and gold SQL pairs  


## 🛠️ Pipeline Overview
1. 📑 **Extract schema information** – read table names and column details from the SQLite database  
2. 🔡 **Embed schemas** – represent each table schema as a dense vector using a pretrained sentence embedding model  
3. ⚡ **Index schemas** – store schema embeddings in a vector index to enable efficient similarity search  
4. 🎯 **Retrieve relevant tables** – select the most relevant table schemas for each user question  
5. 🤖 **Generate SQL** – generate a SQLite-compatible SQL query using a language model constrained by the retrieved schemas  
6. 📊 **Execute and display results** – run the generated query and display results in an interactive interface  



## Installation (Windows)

### 1) Clone the repo
```powershell
git clone https://github.com/ofoski/RAG-SQL-assistant.git
cd RAG-SQL-assistant
```

### 2) Create and activate a virtual environment
```powershell
python -m venv venv
venv\Scripts\activate
```
### 3) Install dependencies
```powershell
pip install -r requirements.txt
```

## Setting the OpenAI API Key (Windows)
You can set the key in two ways:
### Method 1: .env file
```powershell
OPENAI_API_KEY=sk-your-key-here
```
Create a file named .env in the project root.
Add your key as shown above.
The app will automatically load this file.

### Method 2: Command Prompt
```powershell
setx OPENAI_API_KEY "sk-your-key-here"
```
Run the command in the terminal.

## Running the App
```powershell
streamlit run streamlit.py
```

## Dataset

The assistant is scoped to the Chinook dataset by default and is automatically downloaded via KaggleHub (no DB file committed).  

### Swap datasets
You can use other SQLite datasets by replacing the KaggleHub dataset identifier (formatted as `owner/dataset-name`) in the data-loading code.

This line is located in [`load_data.py`](./load_data.py):
```python
import kagglehub
path = kagglehub.dataset_download("ranasabrii/chinook")  
```

## 📊 Example Usage

**Question:**  
*"Which artist has the most albums?"*

**Generated SQL:**  
```sql
SELECT Artist.Name, COUNT(Album.AlbumId) AS AlbumCount
FROM Artist
JOIN Album ON Artist.ArtistId = Album.ArtistId
GROUP BY Artist.Name
ORDER BY AlbumCount DESC
LIMIT 1;
```

## 🖼️ Screenshot
<img width="2846" height="1528" alt="image" src="https://github.com/user-attachments/assets/1ee5a42e-b6b6-4e71-aaf5-4ae323d8dfaa" />

## 🎥 Demo Video
https://github.com/user-attachments/assets/a270f159-479f-4efe-a708-0e4fa615780d


## 📏 Evaluation

- **Metrics:** Execution Accuracy, ROUGE (SQL similarity)
- **Evaluation dataset** → [`sql_eval_dataset.json`](./sql_eval_dataset.json)  

**Difficulty levels:**  
- 🟢 **Easy** → simple queries  
- 🟡 **Medium** → joins & grouping  
- 🔴 **Difficult** → multi-joins, aggregations, revenue calculations

**Results:**  
✅ The assistant successfully generated correct SQL queries for **all examples** in the evaluation dataset,  
returning the same results as the gold SQL.
 
