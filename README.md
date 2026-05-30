# CENG493 Turkish Legal RAG Project

This repository contains the implementation notebooks and evaluation results for the CENG493 term project: **Improving Turkish Legal Question Answering with an Optimized RAG Pipeline**.

## Project Objective

The goal is to build a Turkish legal Retrieval-Augmented Generation (RAG) system that produces grounded, source-supported, citation-consistent answers while minimizing hallucination.

## Final Pipeline

The final system uses:

1. Dense retrieval with `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`
2. BM25 sparse retrieval
3. Hybrid candidate fusion
4. Fine-tuned legal cross-encoder reranker
5. Sentence-level answer-oriented context selection
6. Qwen2.5-7B-Instruct / Qwen2.5-7B LoRA generation
7. Citation verification and grounded fallback

## Main Notebooks

- `notebooks/19_finetune_legal_reranker_clean.ipynb`  
  Fine-tunes the legal cross-encoder reranker using `reranker.jsonl`.

- `notebooks/20_final_grounded_rag_pipeline.ipynb`  
  Builds the final grounded RAG pipeline, runs demo queries, and evaluates retrieval and QA metrics.

## Final Retrieval Results

| System | Recall@5 | Recall@10 | MRR | nDCG@10 |
|---|---:|---:|---:|---:|
| Base Hybrid Retrieval | 0.735 | 0.765 | 0.658 | 0.671 |
| Hybrid + Base Cross-Encoder Reranker | 0.760 | 0.770 | 0.683 | 0.690 |
| Hybrid + Fine-tuned Legal Reranker | 0.760 | 0.790 | 0.724 | 0.753 |

## Final QA / Grounding Results

| System | F1 | BLEU | ROUGE-L | Faithfulness | Citation Accuracy | Hallucination Rate |
|---|---:|---:|---:|---:|---:|---:|
| Base 7B RAG + Old Retrieval | 0.099 | 0.004 | 0.130 | 0.109 | 0.033 | 0.891 |
| Fine-tuned 7B RAG + Old Retrieval | 0.126 | 0.009 | 0.129 | 0.111 | 0.033 | 0.889 |
| Base 7B + Final Grounded RAG Pipeline | 0.283 | 0.074 | 0.269 | 0.984 | 0.990 | 0.016 |
| Fine-tuned 7B + Final Grounded RAG Pipeline | 0.288 | 0.078 | 0.273 | 0.994 | 1.000 | 0.006 |

## Dataset and Model Paths

Full datasets and fine-tuned model weights are not included in the repository due to size and access restrictions.

Expected dataset path in Colab:

```text
/content/Datasets_Ceng493_legal_rag/
