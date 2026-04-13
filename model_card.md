# 🎧 Model Card: Music Recommender Simulation

## 1. Model Name  

**VibeFinder 1.0**

---

## 2. Intended Use  

This recommender suggests songs from a small catalog based on a user's preferred genre, mood, and energy level. It is designed for classroom exploration and simulation, not for real production use. It assumes the user has a single favorite genre and mood.

---

## 3. How the Model Works  

The model compares each song to the user's taste profile. It checks if the song genre and mood match the user's preferences. It also compares energy, valence, danceability, and acousticness to target values. Each feature gets a score, and the model adds them up with weights. Songs with higher total scores are ranked higher.

---

## 4. Data  

The catalog uses 18 songs from `data/songs.csv`. The dataset includes pop, lofi, rock, ambient, jazz, synthwave, indie pop, hip-hop, electronic, classical, country, rnb, metal, reggae, and folk. It includes moods like happy, chill, intense, relaxed, moody, focused, melancholic, peaceful, romantic, energetic, and angry. The dataset does not cover lyrics, artist popularity, or real user listening history.

---

## 5. Strengths  

The system works well for users with clear, focused tastes. It can recommend songs that match a strong pop/happy/energetic profile. It also captures the difference between acoustic and electronic sound. The top results tend to match the expected vibe for the test profile.

---

## 6. Limitations and Bias 

The model over-prioritizes genre and mood as exact matches. It can miss good songs in other genres that have a similar feel. It does not consider tempo ranges, lyrics, or user listening history. It also treats a user as having only one fixed taste profile, which is too narrow for people with mixed preferences.

---

## 7. Evaluation  

I tested the system with a pop/happy profile and confirmed the top songs were upbeat pop tracks. I also checked the ranking output from the CLI to make sure the score details matched the expected features. The system was compared to intuition about which songs should appear for a workout-like profile.

---

## 8. Future Work  

- Add support for multiple favorite genres and moods.
- Use tempo and release style as extra features.
- Add a diversity rule so recommendations do not all come from the same genre.

---

## 9. Personal Reflection  

This section is filled in README.md to keep the user-facing reflection more visible.
