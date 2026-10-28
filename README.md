# Arabic Semantic Search (ChromaDB)

Type a question, get the news articles closest to it **by meaning**, not by matching words. Data: 3,081 Arabic news articles. Tools: `sentence-transformers` and ChromaDB, in one notebook.

## What I did

1. Cleaned the data: removed Word/XML lines pasted into the text, and removed duplicate articles.
2. Found that the model reads only 128 tokens, while most articles are much longer.
3. Split each article into short chunks so the model reads all of it.
4. Turned the chunks into vectors and stored them in ChromaDB with the cosine metric.
5. Tested the search with broad and specific questions.

## Measurements

| What I measured | Result |
|---|---|
| Data size | 3,786 rows before cleaning, 3,081 unique articles after |
| Articles longer than the model's limit (128 tokens) | 95% (median 343 tokens) |
| Chunks after splitting | 14,431 (about 4.7 per article) |
| Chunks still longer than the limit | 1.3% |

## What I learned

- The model's input limit was the most important decision. Without measuring it, the model would have read only the start of each article and I would not have known.
- Broad questions worked. Specific ones (agreement between Qatar and Germany, discovery of a new planet) showed a weakness: the model catches the topic but not the details (Germany vs Austria, planet vs asteroid).
- Cleaning the data changed the results.

## Run it

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Put `Train.csv` in `data/`, open `arabic_search.ipynb`, and Run All.
