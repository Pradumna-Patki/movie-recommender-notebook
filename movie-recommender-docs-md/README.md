# 🎬 Movie Recommender — One-Notebook (TMDb API + scikit-learn + Gradio)

**What it is:** A simple, transparent, and production-readable **content-based movie recommender** you can run end-to-end in **one Jupyter notebook**.  
**How it works:** We fetch movie metadata from **TMDb** (or a tiny built-in sample), turn text into vectors with **TF-IDF**, and find similar movies using **cosine nearest neighbors**. A **Gradio** UI runs **inside the notebook** for instant, interactive recommendations.

> “Simple > clever.” This project is built to be readable, teachable, and easily extended.

---

## ✨ Key Features
- **One-file experience:** Everything in a single `.ipynb` — no separate server.
- **Live UI in notebook:** Gradio app launches inline. Type a title, filter by genres, see results instantly.
- **Content-based approach:** No user histories needed. Uses titles, genres, year, and overview.
- **Explainable:** TF-IDF + cosine NN are easy to reason about and debug.
- **API-backed:** Pulls fresh movies via TMDb *discover* + *genres* endpoints (with a small sample fallback).

---

## 🧠 Why this project?
- **Great for portfolios & interviews:** Shows full-stack thinking (data → features → model → UI) without heavy infra.
- **Teachable baseline:** Demonstrates classic information retrieval before you jump to deep models.
- **Extensible:** Swap TF-IDF for BM25, add posters from TMDb images, or move to FAISS/ANN when the catalog grows.

---

## 🏗️ Architecture (high level)

```
TMDb API (discover + genres)
           │
           ▼
    Pandas DataFrame
(title | genres | overview | year)
           │
           ▼
  Combine text fields (lowercased)
           │
           ▼
   TF-IDF (1–2 grams, stopwords='english')
           │
           ▼
  NearestNeighbors(cosine)
           │
           ▼
 Top-K similar movies (optional genre filter)
           │
           ▼
       Gradio UI in notebook
```

---

## 📦 Tech Choices (and why)

- **Python + Pandas/Numpy** — fast iteration, readable transforms.  
- **scikit-learn TF-IDF + NearestNeighbors (cosine)**  
  - TF-IDF balances frequent/informative terms for text similarity.  
  - Cosine distance is a natural fit for TF-IDF vectors.  
  - NearestNeighbors is simple, reliable for small/medium catalogs.  
- **difflib** — forgiving fuzzy matching for user-typed titles.  
- **Gradio** — 3–5 lines to launch a clean UI right in the notebook.  
- **TMDb API** — rich, standardized metadata; great for demos and prototypes.

> Deep models are powerful, but a **well-tuned classic baseline** is simpler to maintain, explain, and deploy.

---

## 🚀 Quickstart

1) Open the notebook: `Movie_Recommender_API_One_Notebook.ipynb`  
2) Paste your **TMDb Bearer** token or **v3 API key** (or rely on the built-in tiny sample).  
3) Run all cells.  
4) At the bottom, use the **Gradio** block: type a title, (optional) set genres, click **Recommend**.  
5) Increase `PAGES` to ingest more TMDb pages (~20 movies/page).

> The first notebook cell auto-installs any missing packages (`pandas`, `numpy`, `scikit-learn`, `requests`, `gradio`) in the current kernel.

---

## ⚙️ Configuration

- **TMDB_BEARER** (preferred) — v4 Read Access Token  
- **TMDB_API_KEY** — v3 API key (fallback)  
- **USE_SAMPLE_IF_NO_KEY=True** — run without credentials using a tiny sample  
- **PAGES** — number of discover pages to fetch (1–10 good for demos)

**Security:** Don’t commit real tokens. Keep credentials out of version control (env vars/secrets or paste at runtime).

---

## 🔬 Methodology

- **Features:** Lowercased concatenation of `title + genres + year + overview`  
  - Replace `|` with spaces so each genre becomes a token  
- **Vectorization:** `TfidfVectorizer(stop_words='english', ngram_range=(1,2), min_df=1)`  
- **Similarity:** `NearestNeighbors(metric='cosine')`, convert distance → similarity via `1 - d`  
- **Fuzzy matching:** `difflib.get_close_matches` for forgiving user input  
- **Optional genre filter:** Intersection on pipe-separated genres

---

## 📈 Performance & Scaling

- **Small–Medium catalogs:** current approach is fine.  
- **Larger:**  
  - Persist vectors; use **Approximate Nearest Neighbors** (FAISS/Annoy/HNSW).  
  - Consider **BM25** for long overviews.  
  - Add **field weights** (e.g., boost title vs. overview).

---

## 🧪 Quality & Validation

- **Smoke tests:** titles exist, no empty vectors, fuzzy matches return suggestions.  
- **Relevance checks:** spot-check by title/genre; tune n-grams or weights.  
- **A/B or offline eval:** with interactions/ratings, compare CTR/NDCG@K to baselines.

---

## 🗂️ Suggested Repo Layout

```
movie-recommender-notebook/
├── Movie_Recommender_API_One_Notebook.ipynb
├── README.md
└── docs/
    ├── DESIGN_DECISIONS.md
    ├── EXTENSIONS.md
    └── TROUBLESHOOTING.md
```

---

## ❓ FAQ

- **Why not collaborative filtering?** No user–item interactions here; content-based works on day one.  
- **Why not deep embeddings?** Overkill for a teaching demo; start with a strong classical baseline, then iterate.  
- **Duplicates/missing fields?** We drop duplicate titles and handle missing gracefully; TMDb quality is high.

---

## 📜 License & Attribution

- Code: **MIT** (adjust to your preference).  
- TMDb: “This product uses the TMDb API but is not endorsed or certified by TMDb.”  
  Please respect TMDb’s [Terms of Use](https://www.themoviedb.org/terms-of-use) and attribution guidelines.
