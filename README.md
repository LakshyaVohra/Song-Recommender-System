# Song Recommender System

## Overview

This project presents a hybrid content-based song recommender system that utilizes multiple sources of information, including metadata, lyrics, genre, and song/artist/album names. The goal is to generate meaningful and personalized song recommendations by combining various embedding and similarity strategies.

## Feature Categories

The features are categorized into four major groups:

1. **Metadata**  
   Extracted via the Spotify API, these include:
   - Track Popularity
   - Album Type
   - Danceability
   - Energy
   - Key, Mode, Loudness
   - Speechiness, Acousticness, Instrumentalness
   - Liveness, Valence, Tempo

2. **Lyrics**  
   Lyrics were scraped from Genius.com and Musixmatch using the `lyricsgenius` library, with fallback logic. Extracted features include:
   - Pre-trained BERT-based embeddings (`bert-base-multilingual-cased`)
   - Language detection
   - Sentiment analysis
   - Topic modeling

3. **Genre**  
   Artist genres were extracted from Spotify and further processed by:
   - Generating genre embeddings (`distilbert-base-nli-stsb-mean-tokens`)
   - Applying K-Means clustering to form genre clusters

4. **Names (Song, Artist, Album)**  
   Embeddings and descriptive attributes were derived from song/artist/album names:
   - Sentence embeddings (`LaBSE` and `all-MiniLM-L6-v2`)
   - Artist details: follower count, popularity, genre, album count, top track count
   - Album details: release year/month, popularity, track count

## Similarity Computation

Different similarity measures were applied based on feature type:

| Feature Type           | Similarity Metric         |
|------------------------|---------------------------|
| Embeddings (Lyrics, Genre, Names) | Cosine Similarity   |
| Numerical features (Metadata, Sentiment) | Euclidean Distance |
| Popularity             | Absolute Difference       |

A weighted combination of cosine and Euclidean distances was used for some features:

## Final Similarity = α × Cosine Similarity + (1 - α) × Euclidean Distance

All distances were normalized to ensure scale uniformity.

## Sample Results

| Input Songs                                          | Recommended Song        |
|------------------------------------------------------|--------------------------|
| Jeena Jeena, Humdard, Thodi Jagah                    | Darasal (Raabta)         |
| No Love, Lover, One Love                             | Malang Sajna             |
| Believer, Venom, Rise                                | LET ME SEE YA MOVE       |
| Dil Kyun Yeh Mera, Main Agar Kahoon, Chand Sifarish | Phir Aur Kya Chahiye     |
| Lak 28 Kudi Da, High Heels, Hookah Bar               | Kudi Tu Butter           |

## Evaluation

- **Average Similarity Score**: 0.7366
- **Accuracy (Threshold 0.7)**: 68.15%
- Accuracy was evaluated using human judgment and by thresholding average similarity.

## Challenges Faced

- Slow and partial lyric scraping due to API limitations
- High latency in generating BERT embeddings for long texts
- Failed attempts at audio scraping due to access restrictions
- Manual batching and multi-member task division to handle large datasets

## Future Work

1. Integrate audio features by developing a stable audio scraping pipeline.
2. Incorporate external sources such as YouTube metrics and critic reviews.
3. Model more advanced similarity functions using deep metric learning.
4. Combine collaborative filtering techniques for a hybrid recommendation engine.

## Authors

- Rahul  
- Sparsh  
- Lakshya

