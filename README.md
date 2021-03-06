# Disaster News Identification via NLP

The following is a quick project I completed with Gabriel Dusing for General Assembly's Data Science Intensive program. Data gathering and building of the LDA topic model were done by me. The deployed app was created as a collaboration between us, and was the first deployment of a Flask/Gunicorn stack allowing user input for either of us.

https://ga-project5.herokuapp.com/

Disclaimer: This model doesn't work that well. Good learning experience, though.

**The remainder of the readme below is left unchanged from project submission.**

## Contents

- [Introduction](#introduction)
  - [Problem Statement](#problem-statement)
  - [Solution](#solution)
- [Project Description](#project-description)
  - [Data Collection](#data-collection)
  - [Preprocessing and Modelling](#preprocessing-and-modelling)
- [Structure and Data Dictionary](#structure-and-data-dictionary)
- [Conclusions](#conclusions)
- [References](#references)

## Introduction

### Problem Statement

Taken directly from Problem 2 on <https://git.generalassemb.ly/DSI-TOR-9/project-5>:

"During a major disaster, it is essential to provide the public and responders with relevant local news updates in order to gain situational awareness during the event.
During a disaster, news updates are coming from tens to hundreds of different sources, all in different formats, available from different websites, news channels etc., and it is often difficult to find what would be most helpful amid the chaos of other non-disaster related news and media.
There is currently no forum for rounding up and archiving relevant news for a live disaster event.
This project will leverage news feeds relevant to specific disasters, gathered from multiple sources, to create a webpage that presents these live feeds under one umbrella (on one page). This is similar to the Google News feature."

### Solution

We created a news article aggregator that serves up news articles related to disasters using search terms.
This solution was deployed as a Heroku application which can be found here:
<https://ga-tor-9-project-5.herokuapp.com/>

- Our team members are:
  - Patrick Dornian
  - Gabriel John Dusing

- We worked with the following tech stack:
  - Basic python data science tools
    - Pandas
    - Numpy
  - Scikit-Learn
    - Tfidf-Vectorizer
    - RandomForestClassifier
  - NLP Tools
    - Gensim
    - NLTK
  - Web Deployment
    - Flask
    - Heroku

## Project Description

### Data Collection

News articles were collected using Rapid API's News Search API. Additional articles were sourced from [Aylien](https://aylien.com/resources/datasets/natural-disasters-dataset-download), a registration gated news data source.

### Preprocessing and Modelling

There are two stages to our article selection process:

1. Search results are filtered based on whethery they are classified to be a news article about a disaster or not.
2. The filtered articles are then marked with a topic assigned by a trained LDA (Latent Dirichlet Allocation) model.

#### Disaster Classification

In the first stage, we preprocessed our text using TF-IDF to vectorize our text, and used a random forest model (with default settings) to determine if the article(s) pulled by the API is actually talking about a disaster.
The TF-IDF parameters are as follows:

| Parameter | Value |
| :-------: | :----: |
| `stop_words` | `'english'` |
| `max_features` | `5000` |
| `ngram_range` | `(1,3)` |

This model was trained using the articles pulled from the news search api,
with our training dataset consisting of 52\% general articles and 48\% disaster-related articles.
We obtained a train-test accuracy of 100\% - 99\%.

#### Topic Modelling

LDA models are used in NLP to assign topics to unlabelled text data from a corpus. The model was trained on the body text of \~ 32,000 news articles about natural disasters in 2019 sourced from Aylien. 

The dictionary used inclued about 26,000 stemmed words sourced from the total of the articles. It comprised words that appeared in at least 5 articles, but less than 50%.

The most intuitive results were achieved when the model was limited to six topics. The six disaster topics loosely correspond to:

- Drought/Global warming
- Fires
- Earthquakes and seismic events
- Urban/aviation disasters
- Hurricanes/Storms
- Flooding

## Data Dictionary and Folder Structure

### Data Dictionary

Each successful search input returns a data frame with the following dictionary:

| Column | Datatype | Description |
| :----: | :------: | :--------- |
| `title`| text | Title and short description of article |
| `url` | text | Article's URL |
| `datePublished` | datetime | Article's publication date |

The returned result also includes graphics that display what possible disaster types are being included in the search results.

### Folder Structure

Our folders have been organized according to the structure below:

- `general_assembly_jabberwocky_project_5`
  - `__pycache__`

    - `app.cpython-38`

  - `app`

    - `models`

      - `topic_model`

        - `dictionary.tmp`

        - `trained_model.tmp`

        - `trained_model.tmp.expElogbeta.npy`

        - `trained_model.tmp.id2word`

        - `trained_model.tmp.state`

      - `random_forest.pkl`

      - `tfidf_vectorizer.pkl`

    - `static`

      - `icons`

        - `civic`

        - `earthquake`

        - `fire`

        - `sun`

        - `tornado`

        - `water`

    - `templates`

      - `index.html`

    - `app.py`

  - `code`

    - `topic_modelling`

      - `code`

        - `Ida_deploy_functions.ipnyb`

        - `Ida_model_training.ipynb`

      - `gensim_data`

        - `dictionary.tmp`

        - `trained_model.tmp`

        - `trained_model.tmp.expElogbeta.npy`

        - `trained_model.tmp.id2word`

        - `trained_model.tmp.state`

  - `datasets`

    - `disaster_news.csv`

  - `Procfile`

  - `README.md`

## Conclusions and Opportunities for Improvement

The app is mostly a proof of concept. It provides plausible results, but examining the underlying disaster filtering and the topic classification reveals frequent errors.

Model performance of classifying if an article is about a disaster or not could be significantly improved given access to more labelled data and/or a professional grade news API. Topic modelling and disaster classification were trained on different datasets, resulting in inconsistent labelling and results. Due to the catastrophic impact of ignoring a disaster, the model should probably be optimized for specificity (minimizing false negatives). 

To better serve as a disaster news feed, the app could be further developed to make more explicit use of geographical data and filtering, allowing for better browsing of regionally important issues.

Currently, user input is required for the application to return results. Future improvements may include adding a feature that allows the application to automatically update display without needing the user to input search terms each time.
This initial application specifically pulls news articles. A future improvement would allow the application to extract information from other sources, e.g. Twitter, Instagram, YouTube, etc.

## References

Our work would not be possible without the assistance of the following:

- Deploying and Hosting a Machine Learning Model Using Flask, Heroku and Gunicorn
  - Gilbert Adjei
  - <https://heartbeat.fritz.ai/deploying-and-hosting-a-machine-learning-model-using-flask-heroku-and-gunicorn-4b1f748b2ea6>
- Tutorial 2- Deployment of ML models in Heroku using Flask
  - [Krish Naik](https://www.youtube.com/user/krishnaik06/about)
  - <https://youtu.be/mrExsjcvF4o>
