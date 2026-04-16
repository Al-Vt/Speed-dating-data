# Speed Dating Analysis

Speed dating experiment data from Columbia University (2002-2004). 21 waves, ~8,000 dates, one question: **what actually makes two people match?**

---

## The Dataset

Participants met a series of potential partners in 4-minute dates. Before, during, and after the event, they answered surveys about their demographics, lifestyle, activities, preferences, and self-perception. A match occurs when both people say yes.

The data spans three time points:
- **Sign-up** - stated preferences, demographics, activity interests
- **During the event** - scorecards filled after each date
- **Follow-up** (next day, then 3-4 weeks later) - revised preferences, actual dates

Full variable documentation: Speed+Dating+Data+Key.doc

---

## What We Found

Predicting a match from individual profiles alone is surprisingly hard (ROC-AUC ~0.68). The real signal comes from **compatibility between the two people** - not who they are, but how similar they are.

Switching from individual features to pairwise compatibility features (age gap, activity differences, preference alignment) pushed the ROC-AUC from 0.68 to 0.92.

| Model | ROC-AUC | Match recall |
|-------|---------|--------------|
| Logistic Regression (baseline) | 0.681 | 0.51 |
| XGBoost - individual profiles | 0.683 | 0.57 |
| XGBoost - compatibility features | 0.904 | 0.61 |
| LightGBM | 0.918 | 0.58 |
| KNN | 0.893 | 0.65 |
| Stacking (LightGBM + XGBoost + KNN) | 0.924 | 0.83 |

The most predictive features: reading interest gap, age difference, TV sports gap, hiking gap, and alignment between what one person looks for and how the other rates themselves.

---

## Structure

    01-Speed_Dating.ipynb
    Speed+Dating+Data.csv
    requirements.txt
    Dockerfile
    .env.example

---

## Run with Docker

    docker build -t speed-dating .
    docker run -p 8888:8888 -v /c/Users/axelv/Documents/Jedha/DATA_SCIENCE/FULLSTACK/Projet/02_Data_Analysis/models:/app/models speed-dating

Open http://localhost:8888.

## Run locally

    pip install -r requirements.txt
    jupyter notebook

---

## Environment

Copy .env.example to .env and fill in your token:

    HF_TOKEN=your_token

Used only for the Ollama city classification call - result is cached after the first run.
