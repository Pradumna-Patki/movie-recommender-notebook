# DESIGN DECISIONS

This document explains the “why” behind key choices to help reviewers and teammates reason about tradeoffs.

## Problem Framing
We want a simple, explainable recommender that:
- Runs in a single notebook (great for interviews/portfolios).
- Has no user history requirement.
- Returns results instantly with a tiny code surface area.

## Modeling Approach
Content-based similarity over textual metadata:
- Inputs: `title`, `genres`, `year`, `overview`
- Vectorizer: **TF-IDF** (unigrams + bigrams, English stopwords)
- Similarity: **cosine** distance via `NearestNeighbors`

**Rationale**
- Strong baseline for text-only catalogs.
- Transparent: easy to debug and present at whiteboard.
- Fast enough for thousands of items; switches to ANN later if needed.

## Fuzzy Matching
`difflib.get_close_matches` provides robust UX:
- Typos (e.g., "The Matrxi") still map to the intended title.
- Small, dependency-free; for more control, use RapidFuzz.

## Library Choices
- **scikit-learn**: mature, reliable, widely used.
- **pandas/numpy**: ergonomic data munging.
- **Gradio**: ~3 lines to launch a polished UI in the notebook.
- **requests**: minimal overhead for API calls.

## Text Combination Strategy
Concatenate `title + genres + year + overview` lowercased.
- Genres: replace `|` with spaces to tokenize per-genre.
- Title & year: often highly informative; bigrams capture meaningful phrases.

**Alternatives**
- Weighted concatenation (duplicate tokens to up-weight fields).
- Separate vector spaces per field and late fusion (weighted sum of similarities).

## Parameters & Defaults
- `ngram_range=(1,2)`: good phrase coverage without exploding vocabulary.
- `min_df=1`: keep features in small demos; raise to `3–5` for bigger data.
- `k` (top-K): UI slider (3–15).
- Fuzzy cutoff: `0.6` (tune for recall vs. precision).

## Scaling Path
- **BM25** or **sparse retrievers**: better for long descriptions.
- **ANN (FAISS, hnswlib, Annoy)**: sub-linear neighbor search at scale.
- **Hybrid**: lexical (BM25) + semantic (sentence embeddings).
- **Re-ranking**: cross-encoder on the top-N for quality boosts.

## Caching & Persistence
- Notebook demo keeps everything in RAM.
- For production:
  - Persist `vectorizer`, `matrix`, and `model` (`joblib`).
  - Cache TMDb responses to limit API calls and speed restarts.
