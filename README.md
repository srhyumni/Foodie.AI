# Foodie.AI 🍜

**Find Manhattan restaurants by describing what you're craving.**

Foodie.AI is a semantic restaurant recommender for Manhattan. Instead of filtering by cuisine tags, you type a craving in plain language, such as *"spicy Thai noodles"* or *"authentic handmade pasta"*. Foodie.AI then matches it against real restaurant reviews to surface the places that best fit what you want.

> Team project · Columbia University · Original repository: [albertchiyh/Foodie.AI](https://github.com/albertchiyh/Foodie.AI)

---

## The problem

Restaurant apps make you search by category ("Thai", "Italian") or scroll through star ratings. But people usually decide what to eat based on a *craving*, a specific dish, flavor or experience. That information lives in reviews, not in category filters.

Foodie.AI closes that gap by searching **what diners actually wrote about the food**.

## Features

- **Natural-language search**: describe a dish, flavor or vibe in your own words
- **Neighborhood filter**: narrow results to areas such as Chinatown, SoHo or Harlem
- **Ranked by relevance**: each result shows a match score, along with name, cuisine, address, rating and the review snippet that matched your craving
- **Fast responses**: precomputed review embeddings with FAISS similarity search

## How it works

1. Restaurant reviews are converted into vector embeddings with SentenceTransformer (`all-MiniLM-L6-v2`) and stored in a FAISS index.
2. The user's craving is embedded with the same model.
3. FAISS finds the reviews with the closest meaning (cosine similarity), optionally filtered by neighborhood.
4. Matching restaurants are ranked and returned with the most relevant review snippet.

## Tech stack

| Layer | Tools |
|---|---|
| Backend | FastAPI, Pydantic |
| Search | FAISS, SentenceTransformers (`all-MiniLM-L6-v2`) |
| Data | Pandas, NumPy |
| Frontend | HTML, CSS, JavaScript |

## Getting started

```bash
pip install -r requirements.txt
```

Make sure `manhattan_FoodieAI.csv` and `text_FoodieAI_index.faiss` are in the `static/` folder, then run:

```bash
python app.py
```

Open http://localhost:8000 in your browser.

### API

`POST /search-restaurants`

```json
{
  "query": "spicy Thai noodles",
  "neighborhood": "Chinatown",
  "top_k": 5
}
```

Returns ranked restaurants with name, address, cuisine, rating, match score, zip code and a matching review snippet.

## Project structure

```
app.py          FastAPI application
models.py       Pydantic request/response models
utils.py        Embedding utilities
services/       CSV/FAISS loading and search logic
static/         Web frontend, restaurant dataset and FAISS index
Doc/            Project presentation
```

## Team and contributions

This was a four-person team project.

**Sarah Yumni Pradita** ([@srhyumni](https://github.com/srhyumni)): Product lead
- Came up with the product idea and defined the problem: searching restaurants by craving instead of by category
- Sourced and selected the Manhattan restaurant and review data used to build the search index
- Designed the product features, including natural-language search, neighborhood filtering and review-snippet results
- Designed and coded the web interface (HTML/CSS)
- Built and delivered the final project presentation

**Albert Chi** ([@albertchiyh](https://github.com/albertchiyh)) and **Prisca Chang** ([@priscachang](https://github.com/priscachang)): backend, embeddings and search implementation
