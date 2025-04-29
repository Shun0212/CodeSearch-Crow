# 🔍 CodeSearch-Crow

**🇯🇵 日本語でGitHubコードを検索できる自然言語インターフェース**  
**🇺🇸 Natural Language Code Search for GitHub Repositories in Japanese**

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Shun0212/CodeSearch-Crow/blob/main/CodeCrow_RAG.ipynb)

---

## 🧩 概要 / Overview

**CodeSearch-Crow** は、日本語の自然言語クエリを用いて、指定した GitHub リポジトリ内のコードスニペットを検索するツールです。  
This tool allows you to search for relevant code snippets in a GitHub repository using **natural Japanese language queries**.

主な技術構成 / Powered by:

- [`Qwen3-8B-FP8`](https://huggingface.co/Qwen/Qwen3-8B-FP8) – 日本語→英語翻訳（LLM）
- [`CodeSearch-ModernBERT-Crow-Plus`](https://huggingface.co/Shuu12121/CodeSearch-ModernBERT-Crow-Plus) – コードベクトル埋め込み
- FAISS – 類似コードの高速検索
- Google Colab – 手軽に実行可能な実験環境

---

## ⚙️ 特徴 / Key Features

| 🇯🇵 日本語 | 🇺🇸 English |
|----------|-----------|
| 高精度な日本語クエリ対応（LLM翻訳） | High-precision Japanese query translation |
| GitHubリポジトリから関数/コードセル抽出 | Function & cell extraction from GitHub repos |
| `.py`, `.ipynb` 対応の柔軟なパーサー | Supports `.py` and `.ipynb` parsing |
| 事前埋め込み＋FAISSによる高速検索 | Fast retrieval via prebuilt FAISS index |
| モデル再利用可能、低リソース対応 | Model reuse, low memory footprint (with L4 GPU) |

---

## 🚀 クイックスタート / Quick Start (Google Colab)

1. Google Colab でノートブックを開く  
   → [CodeCrow_RAG.ipynb](https://colab.research.google.com/github/Shun0212/CodeSearch-Crow/blob/main/CodeCrow_RAG.ipynb)

2. ランタイムを **GPU（L4以上推奨）** に変更  
   > Runtime > Change runtime type > GPU

3. 必要ライブラリのインストール：

    ```bash
    !pip install lizard faiss-cpu torch numpy sentence-transformers transformers huggingface-hub
    ```

4. 設定を変更（対象リポジトリなど）：

    ```python
    GITHUB_REPO_URL = "https://github.com/Shun0212/CodeBERTPretrained.git"  # ← 対象リポジトリURLに変更
    SAVE_DIR = "./cloned_repos"
    MODEL_NAME = "Shuu12121/CodeSearch-ModernBERT-Crow-Plus"
    QWEN_MODEL = "Qwen/Qwen3-8B-FP8"
    MIN_FUNCTION_LENGTH = 3
    ```

5. セルを順に実行して、インデックス構築 or ロード → 検索開始！

---

## 💬 日本語での検索例 / Example Queries in Japanese

```
- ファイルを1行ずつ読み込むには？
- Web API を作成する関数は？
- データフレームをフィルターするコード
```

➡ 自動で英語に翻訳され、ベクトル検索が行われ、類似するコードが表示されます。

---

## 🔁 処理フロー / System Flow

```text
[日本語クエリ / Japanese Query]
     ↓ Qwen3-8B-FP8（翻訳 / Translation）
[英語クエリ / English Query]
     ↓ CodeSearch-ModernBERT-Crow-Plus（ベクトル化）
[クエリ埋め込み / Embedding Vector]
     ↓ FAISS（高速ベクトル検索）
[関連コードスニペット / Top-K Snippets]
     ↓ 表示（関数名、パス、コード）
```

---

## 📦 モデル構成 / Model Components

| Role | Model |
|------|-------|
| Code Embedding | [`CodeSearch-ModernBERT-Crow-Plus`](https://huggingface.co/Shuu12121/CodeSearch-ModernBERT-Crow-Plus) |
| JA→EN Translation | [`Qwen/Qwen3-8B-FP8`](https://huggingface.co/Qwen/Qwen3-8B-FP8) |

---

## 📁 リポジトリ構成 / Repository Structure

```
CodeSearch-Crow/
├── CodeCrow_RAG.ipynb   # メインのColabノートブック / Main Notebook
├── README.md            # このファイル / This README
├── LICENSE              # Apache-2.0 License
└── .gitignore
```

---

## ⚠️ 注意事項 / Notes

- `.py` の関数、`.ipynb` のコードセルのみが抽出対象です  
  Only `.py` functions and `.ipynb` cells are supported
- `MIN_FUNCTION_LENGTH` 以下の短い関数は無視されます  
  Short snippets below the threshold are ignored
- Qwenモデルは非常に大きいため、**L4 GPU以上を推奨**  
  Qwen model is large — **L4 GPU or better strongly recommended**
- 処理済みデータ (`functions.json`, `faiss_index.bin`) は再利用可能で、2回目以降は高速起動可能  
  Processed data is cached and reused on subsequent runs

---

## 📝 ライセンス / License

This project is licensed under the [Apache 2.0 License](./LICENSE).

---

## 👤 開発者 / Author

Developed by [@Shuu12121](https://huggingface.co/Shuu12121)  
