# 🎵 Music Recommender Simulation

![Screenshot of recommender output](../image.png)
![Top 5 reccommendation for Conflicted Pop](image-1.png)

## Project Summary

This version loads songs from `data/songs.csv` and uses a simple content-based recommender to match songs to a user's taste profile. It compares genre, mood, energy, valence, danceability, and acousticness to produce a ranked list of the best songs. The app also explains why each recommendation was chosen so the output is easy to understand.

Your goal is to:

- Represent songs and a user "taste profile" as data
- Design a scoring rule that turns that data into recommendations
- Evaluate what your system gets right and wrong
- Reflect on how this mirrors real world AI recommenders

---

## How The System Works

### The Concept
Real-world recommenders (Spotify, TikTok) work by comparing what users have liked in the past with songs they haven't heard yet. This system finds songs that are **similar to past favorites** based on audio features. Our version uses **content-based filtering**: instead of comparing user behavior, we compare the actual audio properties of songs. We prioritize **genre and mood matches** (because users typically like the same type of music) and then fine-tune recommendations with **energy levels and emotional tone** to find songs that feel just right.

### Song Features (What We Analyze)
Each `Song` object contains:
- **genre** - Type of music (pop, rock, lofi, jazz, etc.)
- **mood** - Emotional feel (happy, chill, intense, relaxed, etc.)
- **energy** - How intense/exciting (0.0-1.0 scale)
- **valence** - How positive/cheerful (0.0-1.0 scale)
- **danceability** - How suitable for dancing (0.0-1.0 scale)
- **acousticness** - Acoustic vs. electronic (0.0-1.0 scale)
- Plus metadata: id, title, artist, tempo_bpm

### User Profile (What We Know About Users)
Each `UserProfile` stores:
- **favorite_genre** - Their preferred music type
- **favorite_mood** - Their preferred emotional tone
- **target_energy** - How intense they like songs
- **likes_acoustic** - Whether they prefer acoustic or electronic sounds

### Scoring Algorithm
For each song, we calculate a **similarity score** (0-1):
1. **Genre match**: Exact match = 1.0, no match = 0.0
2. **Mood match**: Exact match = 1.0, no match = 0.0  
3. **Energy match**: `1 - |song_energy - user_target_energy|` (rewards closeness)
4. **Valence match**: `1 - |song_valence - user_target_valence|`
5. **Danceability match**: `1 - |song_danceability - user_target_danceability|`
6. **Acousticness preference**: reward high acousticness if the user likes acoustic music, otherwise reward low acousticness
7. **Combine with weights**: Genre (25%) + Mood (20%) + Energy (20%) + Valence (15%) + Danceability (12%) + Acousticness (8%)

### Algorithm Recipe
- Load all songs from `data/songs.csv`
- Convert each song into a feature dictionary
- Compare each song against the user taste profile
- Produce a numeric score for every song
- Remove songs below a confidence threshold
- Sort remaining songs by score in descending order
- Return the top 5 matched songs

### Potential Biases
- This system may **over-prioritize genre**, so a great song in a different genre could be ignored even if it fits the user's mood and energy.
- It also treats mood and genre as exact categories, which can be too strict for users who like mixed or evolving tastes.
- Because weights favor genre and mood, the system may under-value subtler matches like tempo or acoustic feel.

### Ranking & Selection
1. Score all songs
2. Filter out low-confidence matches (score < 0.40)
3. Sort by score (highest first)
4. Return top 5 recommendations

---

## Getting Started

### Setup

1. Create a virtual environment (optional but recommended):

   ```bash
   python -m venv .venv
   source .venv/bin/activate      # Mac or Linux
   .venv\Scripts\activate         # Windows

2. Install dependencies

```bash
pip install -r requirements.txt
```

3. Run the app:

```bash
python -m src.main
```

### Running Tests

Run the starter tests with:

```bash
pytest
```

You can add more tests in `tests/test_recommender.py`.

---

## Experiments You Tried

I tested the system with a workout-style profile that prefers pop, happy mood, and high energy. The top recommendations were pop and upbeat songs, which matched expectations. I also added new songs in genres like classical, reggae, and metal to check whether the ranking could still surface good matches based on energy and mood.

I noticed non-pop songs could still appear if they had the right mood and energy, but genre-match songs stayed at the top.

---

## Limitations and Risks

This recommender uses a small catalog and does not consider lyrics, artist popularity, or real user listening history. It treats genre and mood as exact categories, so it can miss songs that feel right but are labeled in a different genre. The model also assumes one fixed taste profile per user, which is too narrow for people who like multiple styles.

You will go deeper on this in your model card.

---

## Reflection

Read and complete `model_card.md`:

[**Model Card**](model_card.md)

I learned that simple recommenders can still behave clearly if the scoring is transparent. It was interesting to see how much genre and mood dominate the result. This project showed me that real music recommenders need more than feature matching to feel truly personal.

---

## 7. `model_card_template.md`

Combines reflection and model card framing from the Module 3 guidance. :contentReference[oaicite:2]{index=2}  

```markdown
# 🎧 Model Card - Music Recommender Simulation

## 1. Model Name

Give your recommender a name, for example:

> VibeFinder 1.0

---

## 2. Intended Use

- What is this system trying to do
- Who is it for

Example:

> This model suggests 3 to 5 songs from a small catalog based on a user's preferred genre, mood, and energy level. It is for classroom exploration only, not for real users.

---

## 3. How It Works (Short Explanation)

Describe your scoring logic in plain language.

- What features of each song does it consider
- What information about the user does it use
- How does it turn those into a number

Try to avoid code in this section, treat it like an explanation to a non programmer.

---

## 4. Data

Describe your dataset.

- How many songs are in `data/songs.csv`
- Did you add or remove any songs
- What kinds of genres or moods are represented
- Whose taste does this data mostly reflect

---

## 5. Strengths

Where does your recommender work well

You can think about:
- Situations where the top results "felt right"
- Particular user profiles it served well
- Simplicity or transparency benefits

---

## 6. Limitations and Bias

Where does your recommender struggle

Some prompts:
- Does it ignore some genres or moods
- Does it treat all users as if they have the same taste shape
- Is it biased toward high energy or one genre by default
- How could this be unfair if used in a real product

---

## 7. Evaluation

How did you check your system

Examples:
- You tried multiple user profiles and wrote down whether the results matched your expectations
- You compared your simulation to what a real app like Spotify or YouTube tends to recommend
- You wrote tests for your scoring logic

You do not need a numeric metric, but if you used one, explain what it measures.

---

## 8. Future Work

If you had more time, how would you improve this recommender

Examples:

- Add support for multiple users and "group vibe" recommendations
- Balance diversity of songs instead of always picking the closest match
- Use more features, like tempo ranges or lyric themes

---

## 9. Personal Reflection

A few sentences about what you learned:

- What surprised you about how your system behaved
- How did building this change how you think about real music recommenders
- Where do you think human judgment still matters, even if the model seems "smart"