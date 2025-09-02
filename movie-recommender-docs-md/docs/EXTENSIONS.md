# EXTENSIONS & NEXT STEPS

A menu of ideas to extend this project depending on your goals.

## UI & UX
- **Add posters and links**:
  - Use TMDb configuration + image endpoints to fetch poster URLs.
  - Show posters next to recommendations in the Gradio UI.
- **Live search suggestions**:
  - Hit TMDb `/search/movie` as the user types; blend catalog and live results.
- **Filters**:
  - Year range slider, minimum vote count, or language filter.
  - Toggle to exclude adult content.

## Modeling
- **Field weighting**:
  - Build separate TF-IDF vectors per field; combine with weighted similarity.
- **BM25** (rank_bm25, Elasticsearch/OpenSearch):
  - Better lexical retrieval on long “overview” fields.
- **ANN / Vector DB**:
  - FAISS, hnswlib, or a hosted vector DB for speed at scale.
- **Semantic embeddings**:
  - Sentence-transformers for overviews; combine with lexical retrieval (hybrid).
- **Re-ranking**:
  - Cross-encoder (e.g., MiniLM) on top-50 to refine final top-10.

## Data & Features
- **Multi-lingual**:
  - Pass language to TMDb API; create separate indices per language.
- **Freshness**:
  - Periodic re-fetch and re-fit; schedule a simple pipeline/notebook.
- **Cold-start user personalization**:
  - Let users “like” a few movies; average their embeddings for a profile.

## MLOps & Prod
- **Artifacts**:
  - `joblib` dump for vectorizer + matrix + NN model.
- **Packaging**:
  - Convert notebook to a small FastAPI service or package.
- **CI checks**:
  - Lint (`ruff`/`black`) and smoke tests for basic regressions.
