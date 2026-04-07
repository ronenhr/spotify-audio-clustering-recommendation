# 🎵 Alfred's Brain — Music Clustering & Recommendation Engine

This project explores the Spotify Tracks Dataset (~114K tracks) to discover natural mood groupings in music using audio features, then builds a similarity engine to recommend songs based on audio DNA.

## 🎯 Project Overview

The core work involves:

1. **Thorough EDA** — Understanding the dataset structure, audio feature distributions, correlations, and data quality issues
2. **Feature Engineering** — Selecting, scaling, and testing feature combinations for optimal clustering
3. **Clustering** — Grouping ~88K tracks into 5 mood categories using K-Means on 3 audio features (energy, valence, acousticness)
4. **Similarity Engine** — Finding similar songs using cosine similarity across 10 audio features, with popularity filtering

This serves as the foundation for **Alfred**, a future music recommendation chatbot that will use NLP and personalization to suggest tracks based on natural language input.

## 📊 Key Results

### Mood Clusters (K=5, Silhouette=0.352)

| Cluster | Energy | Valence | Acousticness | Mood Label |
|---------|--------|---------|-------------|------------|
| 0 | 0.79 | 0.73 | 0.12 | Electronic Party |
| 1 | 0.25 | 0.25 | 0.84 | Acoustic & Atmospheric |
| 2 | 0.53 | 0.31 | 0.23 | Moody & Electronic |
| 3 | 0.87 | 0.28 | 0.04 | Intense & Aggressive |
| 4 | 0.55 | 0.71 | 0.64 | Acoustic & Feel-Good |

Clusters were created purely from audio features with no genre information, yet align with recognizable musical moods — validated against genre distributions.

### Example Recommendations

**"Creep" by Radiohead** → Brandi Carlile, Lil Peep, Five For Fighting, The Verve Pipe — moody, mid-tempo, emotional tracks

**"River Flows In You" by Yiruma** → Satie, Chopin, Einaudi, Philip Glass — classical/neoclassical piano with 0.99+ similarity

**"One Kiss" by Calvin Harris & Dua Lipa** → Imagine Dragons, BLACKPINK, Dua Lipa "Don't Start Now" — high-energy dance tracks

## 🔬 Methodology

### EDA Highlights
- Discovered dataset structure: 114K rows are song-genre pairs, not unique songs (~89K unique after deduplication)
- 114 genres with exactly 1,000 tracks each (by design) — after deduplication, counts range from 74 (reggaeton) to 1,000 (acoustic)
- Strong correlations: energy/loudness (0.76), energy/acousticness (-0.73)
- 18% of tracks have zero popularity but normal audio profiles — kept for clustering, used as filter for recommendations

### Clustering Journey (0.171 → 0.352)

| Approach | Silhouette |
|----------|-----------|
| All 10 features | 0.171 |
| Mood features (5) | 0.218 |
| Emotion core: V+D+E (3) | 0.270 |
| V+D+A (3) | 0.310 |
| **E+V+A (3)** | **0.352** |

- Tested K-Means, GMM, and DBSCAN — K-Means produced the most practical results
- Exhaustively tested all 84 three-feature combinations
- Discovered that **highest silhouette ≠ best clustering** — the top-scoring combo (0.56) produced unbalanced, uninterpretable clusters dominated by skewed features
- Final model: **Energy + Valence + Acousticness** — captures intensity, emotion, and texture

### Similarity Engine
- Cosine similarity across 10 audio features for fine-grained song matching
- Popularity filter transforms obscure but mathematically correct results into practical recommendations
- Validated across all 5 clusters with musically coherent results

## 🛠️ Tech Stack

- **Python** — pandas, numpy, scikit-learn, matplotlib, seaborn, plotly
- **Clustering** — K-Means (sklearn)
- **Similarity** — Cosine similarity (sklearn)
- **Scaling** — StandardScaler (sklearn)
- **Visualization** — PCA for 2D/3D cluster visualization

## 📁 Dataset

**Source:** [Spotify Tracks Dataset](https://www.kaggle.com/datasets/maharshipandya/-spotify-tracks-dataset) on Kaggle

To reproduce this project:
1. Download the dataset from Kaggle
2. Place the CSV file in the project root directory
3. Rename it to `dataset.csv` (or update the path in the notebook)

## 🚀 Getting Started

```bash
# Clone the repository
git clone https://github.com/ronenhr/spotify-audio-clustering-recommendation.git
cd spotify-audio-clustering-recommendation

# Install dependencies
pip install -r requirements.txt

# Download the dataset from Kaggle and place it in the project root

# Open the notebook
jupyter notebook spotify_audio_clustering_recommendation.ipynb
```

## 📌 Key Lessons

- **Feature selection > algorithm selection** — 3 carefully chosen features outperformed 10 across every algorithm
- **Domain validation matters** — silhouette scores alone can be misleading; clusters must make musical sense
- **Music is a continuous spectrum** — moderate clustering scores are normal for audio data
- **Popularity filtering is essential** — bridges the gap between mathematical similarity and practical usefulness

## ⚠️ Known Limitations

- **Cluster 3 (Intense & Aggressive) is the weakest cluster** — Spotify's audio features measure energy and loudness but not timbre, distortion, or vocal style
- **Audio features alone can't capture artistic uniqueness** — artists with distinctive sounds (Gorillaz, Radiohead) aren't fully represented by standard features
- **No personalization** — recommendations are based purely on audio similarity, not user history

## 🔮 Future Work — Alfred v2

- **NLP Layer:** Sentence embeddings for free-text mood input ("something chill like Bon Iver")
- **Spectral Features:** MFCCs and spectral centroid to better capture timbre and aggression
- **Chatbot Interface:** Conversational AI for interactive music discovery
- **Spotify API Integration:** Real-time recommendations from the live catalog
- **Collaborative Filtering:** Combine audio similarity with user listening patterns

## 📄 License

This project is for educational and portfolio purposes.
