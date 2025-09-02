# TROUBLESHOOTING

## Installation / Imports
**ModuleNotFoundError: No module named 'gradio'**  
- Re-run the first Setup cell — it auto-installs missing packages into the current kernel.
- Some kernels need an import cache refresh; the cell already calls `importlib.invalidate_caches()`.

**IPython magic not found / not recognized**  
- The notebook uses either `%pip` magic (when IPython is available) or `python -m pip` programmatically.
- If in doubt, restart the kernel and re-run the first cell.

## API / Data
**No TMDb credentials**  
- Set `USE_SAMPLE_IF_NO_KEY=True` to use the tiny built-in dataset for demo/testing.

**TMDb “Unauthorized”**  
- Double-check you pasted the correct Bearer token or v3 key.
- Rate limits: keep `PAGES` low (1–5) and avoid repeated rapid runs.

**Empty results / weird recommendations**  
- Try more pages to expand the catalog (`PAGES=5` or `10`).
- Ensure the input title exists (use suggestions) or tune fuzzy cutoff from `0.6` → `0.5`.

## Similarity & Quality
**Too similar / too generic**  
- Increase `ngram_range` to `(1,3)`; add field weights (boost titles); filter by genres.

**Slow lookups**  
- For large catalogs, adopt an ANN index (FAISS/hnswlib) and persist artifacts.

## Notebook UI
**Gradio doesn’t show inline**  
- Ensure `demo.launch(inline=True)` is used.
- Some hosted notebook environments need a public URL; use `share=True` temporarily.
