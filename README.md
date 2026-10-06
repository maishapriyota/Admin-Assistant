# Admin Assistant: a privacy-first assistant for public-administration documents

A small retrieval-augmented question-answering app, built to explore how language models can be used responsibly in public administration, where data protection and trustworthy answers matter more than fluent ones.

**Live demo:** _add your GitHub Pages link here_

## What it does

- **Answers only from the documents, and cites them.** Every answer shows the passage it came from.
- **Refuses instead of guessing.** If the documents don't cover a question, it says so, and nothing is sent to any model.
- **Removes personal data first.** Emails, phone numbers, IBANs, dates and self-introduced names are replaced by placeholders before retrieval or any model call. A "Model input" tab shows exactly what a model would receive.
- **Works with or without an LLM.** Default is an offline extractive mode. It can also call a **local model through Ollama** (data never leaves the machine) or any **OpenAI-compatible API**. If the model call fails, it falls back to the offline answer.
- **Measures itself.** The Evaluation tab runs a test set in the browser and reports retrieval accuracy, correct refusals, wrongly refused questions and personal-data leaks.

## How it works

1. `redact()` removes personal data from the question.
2. `search()` ranks passages with BM25 and checks answerability: the best passage must cover at least 60% of the question's key terms.
3. If answerable, a prompt is built that restricts the model to the numbered context and requires citations. Otherwise the app refuses.
4. `evaluate()` runs 16 test questions (12 answerable, 3 unanswerable, 1 containing personal data) plus 2 redaction tests.

## Run it

Open `index.html` in a browser, or host the folder with GitHub Pages (Settings, Pages, deploy from the main branch). No build step and no dependencies.

To use a local model: install Ollama, run `ollama pull llama3.2`, start it with `OLLAMA_ORIGINS=*`, then choose "Local LLM via Ollama" in Settings.

## Use your own documents

The sample documents are **fictional** (the city of Musterstadt is invented). Replace the `DOCS` array in `index.html` with real public documents, add matching questions to `TESTS`, and re-run the evaluation.

## Results

_Run the Evaluation tab and paste your numbers here. Add harder questions first, since a small self-written test set is easy to pass._

## Limitations and next steps

- Retrieval is keyword-based (BM25). Embeddings with a vector store, such as pgvector on Supabase, would handle paraphrased questions better.
- Redaction uses simple patterns and will miss some names and addresses. A trained named-entity model would be stronger.
- Answerability uses a term-coverage threshold chosen by hand. It should be tuned on a larger test set.
- LLM answers are not yet scored automatically. A next step is checking that every claim is supported by a cited passage.
