# CENG493 Turkish Legal RAG Project — Instructor Testing Guide

This README explains how the instructor can test and use the Turkish Legal RAG system from GitHub.

## 1. Main Testing Notebook

The main notebook for testing the final system is:

```text
notebooks/20_final_grounded_rag_pipeline.ipynb
```

This notebook contains:

```text
- Final Grounded RAG Pipeline
- Question answering demo
- Gradio custom corpus upload interface
- Custom benchmark evaluation interface
```

The instructor does not need to copy code cells manually. The notebook should be opened directly in Google Colab and the cells should be run in order.

---

## 2. Recommended Environment

The project is designed to run on Google Colab.

Recommended runtime:

```text
Google Colab + GPU
```

Recommended GPU:

```text
A100 is recommended.
T4 can also be used, but model loading and inference may be slower.
```

To enable GPU in Colab:

```text
Runtime → Change runtime type → GPU
```

---

## 3. Required Model Files

The full model weights are not stored directly in GitHub because of file size limitations.

The required fine-tuned model files are provided through Google Drive links in:

```text
models/README.md
```

The instructor should download or add the model folders to Google Drive.

Expected model paths in Colab:

```text
/content/drive/MyDrive/CENG493_models/finetuned_legal_reranker
/content/drive/MyDrive/qwen2_5_7b_legal_lora_adapter
```

If the model folders are placed somewhere else, update these variables in the notebook:

```python
FT_RERANKER_PATH = "/content/drive/MyDrive/CENG493_models/finetuned_legal_reranker"
FT_LLM_ADAPTER_PATH = "/content/drive/MyDrive/qwen2_5_7b_legal_lora_adapter"
```

The base model:

```text
Qwen/Qwen2.5-7B-Instruct
```

is downloaded automatically from Hugging Face when the notebook is executed.

---

## 4. Required Dataset Files

For testing the original project dataset, the expected dataset folder is:

```text
/content/Datasets_Ceng493_legal_rag/
```

Expected structure:

```text
Datasets_Ceng493_legal_rag/
├── corpus.jsonl
├── gold_benchmark.json
├── reranker.jsonl
└── llm.jsonl
```

If the dataset is uploaded to another path, update the dataset path variables in the notebook.

---

## 5. Step-by-Step Testing Instructions

### Step 1 — Open the final notebook

Open this file from GitHub:

```text
notebooks/20_final_grounded_rag_pipeline.ipynb
```

Then open it in Google Colab.

---

### Step 2 — Enable GPU runtime

In Colab:

```text
Runtime → Change runtime type → GPU
```

---

### Step 3 — Run setup cells

Run the setup/import cells at the beginning of the notebook.

These cells install and import the required libraries.

---

### Step 4 — Mount Google Drive

Run:

```python
from google.colab import drive
drive.mount("/content/drive")
```

This is required for accessing the fine-tuned model folders.

---

### Step 5 — Check model paths

Make sure the fine-tuned model folders exist at:

```text
/content/drive/MyDrive/CENG493_models/finetuned_legal_reranker
/content/drive/MyDrive/qwen2_5_7b_legal_lora_adapter
```

If not, update the notebook paths accordingly.

---

### Step 6 — Prepare dataset

Upload or extract the dataset folder to:

```text
/content/Datasets_Ceng493_legal_rag/
```

The folder should contain:

```text
corpus.jsonl
gold_benchmark.json
reranker.jsonl
llm.jsonl
```

---

### Step 7 — Run the notebook cells in order

Run the cells for:

```text
1. Imports and setup
2. Path configuration
3. Corpus loading
4. Embedding model loading
5. FAISS index creation
6. BM25 index creation
7. Fine-tuned reranker loading
8. Retrieval functions
9. Sentence-level context selection
10. LLM loading
11. Grounded answer generation
12. Citation verification and fallback
13. Demo functions
14. Gradio interface
```

---

## 6. Testing the Default RAG System

After the required cells are executed, the instructor can test the default RAG system using:

```python
ask_legal_rag("Kıdem tazminatı nedir?")
```

Other example questions:

```python
ask_legal_rag("Anayasa madde 1'e göre Türkiye'nin devlet şekli nedir?")
```

```python
ask_legal_rag("İdari dava türleri nelerdir?")
```

The system output includes:

```text
- Generated answer
- Citation
- Selected source sentences
- Document ID
- Document title
- Relevance score
```

---

## 7. Testing with a Custom Corpus

The notebook also includes a Gradio interface for testing with a custom uploaded corpus.

To start the Gradio interface, run the final Gradio cell:

```python
app.launch(share=True)
```

A temporary Gradio link will be generated.

The Gradio interface contains:

```text
1. Upload Custom Corpus
2. Ask Question
3. Evaluate Custom Benchmark
```

---

## 8. Custom Corpus Format

The instructor can upload a custom corpus file in one of the following formats:

```text
.jsonl
.json
.txt
```

Recommended `.jsonl` format:

```json
{"id": "doc1", "title": "Custom Legal Document", "text": "Madde 1 ..."}
{"id": "doc2", "title": "Custom Legal Document", "text": "Madde 2 ..."}
```

After uploading the corpus file:

```text
1. Go to the "Upload Custom Corpus" tab.
2. Upload the corpus file.
3. Click "Build Index".
4. Wait until the system builds the FAISS and BM25 indexes.
5. Go to the "Ask Question" tab.
6. Ask a Turkish legal question.
```

The answer will be generated only from the uploaded custom corpus.

---

## 9. Custom Benchmark Evaluation

If the instructor wants to compute metrics on a custom dataset, a benchmark file can also be uploaded.

Supported benchmark formats:

```text
.json
.jsonl
```

Required benchmark fields:

```text
question
verified_answer
gold_sources
```

Example benchmark format:

```json
[
  {
    "question": "Kıdem tazminatı nedir?",
    "verified_answer": "Kıdem tazminatı, işçinin çalıştığı süreye bağlı olarak işverenden aldığı toplu paradır.",
    "gold_sources": [
      {"doc_id": "doc1"}
    ]
  }
]
```

To evaluate a benchmark:

```text
1. Upload and index a custom corpus first.
2. Go to the "Evaluate Custom Benchmark" tab.
3. Upload the benchmark file.
4. Click "Evaluate Benchmark".
```

The system computes:

```text
- Recall@5
- Recall@10
- MRR
- nDCG@10
- Exact Match
- F1
- BLEU
- ROUGE-L
- Faithfulness
- Citation Present
- Citation Accuracy
- Hallucination Rate
```

---

## 10. Important Notes

The Gradio `share=True` link is temporary. It works only while the Colab session is active.

For later testing, the instructor should reopen the notebook from GitHub, enable GPU runtime, mount Google Drive, load the model files, and run the cells again.

Notebook outputs were cleared for readability and GitHub rendering. The instructor should run the cells in order to reproduce the outputs.

The fine-tuned model files are required for full testing. Without the model folders, the final RAG system and Gradio demo may not run correctly.

---

## 11. Quick Test Summary

For a quick test:

```text
1. Open notebooks/20_final_grounded_rag_pipeline.ipynb in Colab.
2. Enable GPU.
3. Mount Google Drive.
4. Make sure model folders are available.
5. Upload/extract the dataset folder.
6. Run the notebook cells in order.
7. Test with ask_legal_rag().
8. Optionally launch Gradio with app.launch(share=True).
```

Example quick test command:

```python
ask_legal_rag("Kıdem tazminatı nedir?")
```
