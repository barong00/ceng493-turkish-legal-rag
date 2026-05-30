# Dataset Files

The full dataset files are not included in this repository due to size and access restrictions.

The notebooks expect the dataset folder to be available in Colab at:

[/content/Datasets_Ceng493_legal_rag/](https://drive.google.com/drive/folders/1ak6PC0TV4IbXMwqjZMv73qeiuDb-FLKp?usp=sharing)

Required files:
- corpus.jsonl
- gold_benchmark.json
- reranker.jsonl
- llm.jsonl
- embedding.jsonl
- rag_eval.json

The system supports custom document collections by replacing corpus.jsonl and rebuilding the FAISS and BM25 indexes.
