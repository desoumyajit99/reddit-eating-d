# Reddit Eating Disorder Project — PRAW Scraper for r/EDanonymemes

A minimal starter to collect posts (and optionally comments) from **r/EDanonymemes** using [PRAW](https://praw.readthedocs.io/). Keep it simple: set up the environment with Poetry, run the notebook, and start scraping.

---

## Quickstart

### 1) Set up the environment (Poetry)

```bash
# open a terminal
cd reddit-eating-d

# initialize poetry
poetry init

# use Python 3.13 for the virtualenv
poetry env use 3.13

# create a lockfile (okay if this says nothing to lock yet)
poetry lock
```

Open `pyproject.toml` and set the Python line to:

```toml
requires-python = ">=3.11,<4"
```

Install packages from `requirements.txt`:

```bash
poetry add $(cat requirements.txt)
```

Install Jupyter kernel for the project:

```bash
poetry add ipykernel
poetry run python -m ipykernel install --user --name reddit-eating-d
```

### 2) Add Reddit API credentials (if running on bash)

Create environment variables (recommended) or use a `.env` file:

```bash
export REDDIT_CLIENT_ID="YOUR_CLIENT_ID"
export REDDIT_CLIENT_SECRET="YOUR_CLIENT_SECRET"
export REDDIT_USER_AGENT="script:ed-scraper:0.1 (by u/YOUR_USERNAME)"
```

### 3) Run the notebook

```bash
poetry run jupyter notebook
```

Open **`etd.ipynb`** and run the cells.

---

## What this does (in plain words)

* Connects to Reddit with PRAW
* Pulls recent posts from **r/EDanonymemes** (titles, authors, timestamps, etc.)
* (Optional) Grabs comments per post
* Saves simple outputs (e.g., JSON/CSV like `authors.json`, `posts.json`) for analysis

---

## Minimal example (used inside the notebook)

```python
import os, json
import praw

reddit = praw.Reddit(
    client_id=os.environ["REDDIT_CLIENT_ID"],
    client_secret=os.environ["REDDIT_CLIENT_SECRET"],
    user_agent=os.environ["REDDIT_USER_AGENT"],
)

authors = []
for s in reddit.subreddit("EDanonymemes").new(limit=40):
    authors.append(str(s.author))

with open("authors.json", "w", encoding="utf-8") as f:
    json.dump(authors, f, ensure_ascii=False, indent=2)

print("Saved authors.json")
```

---

## Tips

* Keep credentials out of your code; use env vars.
* Be respectful of subreddit rules and rate limits.
* This repo is intentionally simple—start with `etd.ipynb`, and expand as needed.

---

