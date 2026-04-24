# Spotify Song Analysis — What Makes a Track Go Popular?

Spotify hosts over 100 million songs. Every day, labels release new tracks hoping they break through — but most don't. What separates the songs that climb the charts from the ones that sit at zero plays?

This project analyzes a dataset of Spotify tracks to find out. Using audio features that Spotify measures for every song (like energy, danceability, and loudness), it groups songs into meaningful categories, builds models to predict popularity, and identifies which musical qualities actually drive streaming success — and which ones don't matter as much as you'd think.

---

## Why This Matters

For music labels, streaming platforms, and artists, understanding what predicts popularity isn't just academic. It's a business question with real dollar value:

- **Labels** deciding which tracks to invest promotion budgets in could use a model like this to flag high-potential songs before committing resources
- **Streaming platforms** could use song clusters to improve playlist recommendations and keep listeners engaged longer — which directly drives subscription retention
- **Artists and producers** could use audio feature benchmarks to understand how their sound compares to what's performing well in their genre

Data can't replace taste or culture. But it can sharpen the decisions that surround them.

---

## What the Analysis Found

After testing 11 different models, the clearest signal in the data comes down to this:

> **Popular songs tend to be energetic, vocal, loud, and upbeat.**
> **Unpopular songs tend to be instrumental, acoustic, quiet, or niche.**

But — and this is important — **genre changes everything**. The same level of energy that drives popularity in EDM hurts it in folk. A good model has to account for that interaction, not just look at audio features in isolation. That's what the winning model does.

The overall predictive accuracy is modest (the model explains about 11–12% of popularity variance), which is actually the honest and expected result. Popularity is shaped by timing, promotion, artist following, and cultural context — things no audio fingerprint can capture. What the model does reliably identify is the direction: which sonic qualities help and which hurt, within what the data can measure.

---

## How the Analysis Works

### Step 1 — Exploring the Data

Before building anything, the data is examined to understand its shape. A few things stand out:

- Many songs have a popularity score of zero — likely unpublished or unlisted tracks, not genuine failures
- No single audio feature is strongly correlated with popularity on its own. Everything interacts.
- Genre and subgenre carry meaningful signal that pure audio measurements miss

### Step 2 — Clustering Songs into Groups

Using a technique called **hierarchical clustering** (which works by grouping songs that sound similar, then progressively merging groups until natural clusters emerge), the dataset is divided into 5 distinct song profiles:

| Cluster | Sound Profile | Where They Appear |
|---|---|---|
| 1 | High energy, highly danceable, moderately acoustic | Mixed genres |
| 2 | Very high energy, mid danceability | EDM, pop |
| 3 | Danceable, mostly vocal (low instrumentalness) | Pop, R&B |
| 4 | High speechiness — more talking than singing | Hip-hop, rap |
| 5 | Acoustic and instrumental | Folk, singer-songwriter |

These clusters are musically meaningful — a label executive or playlist curator would recognize them immediately. That's a sign the model is capturing something real.

### Step 3 — Predicting Popularity

Eight regression models are built (regression = a mathematical formula that predicts a number, in this case a popularity score). They range from simple — just using genre — to complex, with interactions between audio features and genre categories.

The strongest model is one that lets **genre modify the effect of audio features**. It recognizes that energy, loudness, and danceability don't affect all genres equally — and that nuance is what makes it outperform the simpler versions.

### Step 4 — Regularization: Keeping the Model Honest

Complex models can overfit — they memorize the training data instead of learning generalizable patterns. To prevent this, three **regularization** techniques are applied (methods that deliberately simplify a model to make it more reliable on new data):

- **Ridge** — keeps all features but shrinks their influence, preventing any one variable from dominating
- **LASSO** — goes further by cutting weaker features out entirely, leaving only the variables that genuinely matter. 20 out of 67 features were eliminated.
- **Elastic Net** — a blend of both approaches

**LASSO wins.** It matches the best unregularized model's accuracy while using 30% fewer variables — a cleaner, more trustworthy result.

### Step 5 — Testing with Random Forest

A **Random Forest** (an ensemble method that builds hundreds of decision trees and averages their predictions, capturing complex nonlinear patterns that simple formulas miss) is also tested. Interestingly, it doesn't outperform LASSO on new data — which tells us that the relationship between audio features and popularity is mostly linear. There's no hidden complexity that a more powerful model unlocks.

---

## Final Model Performance

| Model | Accuracy (R²) | Error (RMSE) |
|---|---|---|
| LASSO — Categorical × Continuous | 0.115 | 2.08 |
| All Inputs (unregularized) | 0.118 | 2.08 |
| Continuous features only | lower | higher |
| Random Forest | 0.115 | 2.17 |

R² of 0.115 means the model explains about 11–12% of what makes a song popular. That sounds low — but it's the honest ceiling for audio-feature-only data. Popularity is partly algorithmic, partly social, partly luck. This model captures the part that's measurable.

---

## Tech Stack

| Tool | Role |
|---|---|
| Python 3 | Core language |
| Pandas & NumPy | Data preparation and manipulation |
| Scikit-learn | All models: regression, clustering, regularization, Random Forest, cross-validation |
| Matplotlib & Seaborn | Charts and visualizations |
| Jupyter Notebook | Interactive analysis environment |

---

## How to Run

```bash
git clone https://github.com/ChanelWu/spotify-song-analysis.git
cd spotify-song-analysis
pip install pandas numpy scipy scikit-learn matplotlib seaborn jupyter
jupyter notebook Portfolio_Spotify.ipynb
```

---

## Built By

**Xueying Wu (Chanel)** — Data Science student, CompTIA Data+ certified.

---

*This project is for educational and portfolio purposes only.*
