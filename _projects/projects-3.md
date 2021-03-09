---
title: "Random Rocket stuff"
excerpt: "High-level summaries of my work done at an online travel agency"
collection: projects
---

I've worked on a bunch of stuff in my time at Rocket and it's really easy for me to forget it all. This document is just some notes for me to keep track of what I've done, although (as I've said elsewhere) the notes are easier for me to understand when I force myself to write it in a way that anyone can read. 

## Rocketmiles Search Result Ranking Model (August 2018 - May 2019)
Due to data volume and problem complexity, search result ranking is one of the most ambitious machine learning projects an e-commerce company can take on. However, good search result rankings are critical to the customer experience, and upgraded algorithms have been shown at companies such as [Airbnb](https://arxiv.org/abs/1810.09591) to create tremendous lift in customer conversion rates. I prototyped a gradient boosting tree model for ranking called **LambdaMART**, which required me to come up with custom data sampling strategies to serve the use case. To productionize the model, I used big data infrastructure and a modern realtime model deployment stack built on Amazon Web Services. We are currently working on deploying the search result model for live A/B testing.

### Concepts
As ranking is a less-discussed data science problem, I self-learned the specialized training procedure underpinning the LambdaMART models. This involved detailed analysis of the behavior of the standard ranking metric, **normalized discounted cumulative gain**, and understanding the philosophy of **preference pairs** behind **listwise ranking models**. Because many standard sampling strategies would bias the model (e.g. against gross margin), I came up with a custom **sampling strategy** to define which search results to keep in the training dataset. To validate the model, I used a **game-theoretic** framework called **SHAP values** to decompose model scores into the contributions given by each training feature.

### Tools and Languages
I used the **Python xgboost package** to build the LambdaMART model. The **shap** library was used to audit the model. To process the training dataset, I rewrote all queries to run using **Spark** on **AWS EMR**. I am currently working on a real-time deployment using **AWS Sagemaker** for model endpoint hosting, **Redis** for feature caching, and **AWS Lambda** for event-driven API responses.

## Rocketmiles Fraud Model (January 2018 - June 2018)
Like the Rocketmiles search result ranking models, I designed, deployed, and own the Rocketmiles fraud detection models. This was a huge project stretching over 6 months (January 2018 - June 2018) from the exploratory stage to stable deployment. Models (currently gradient boosted forests) predict on all incoming transactions to flag possibly fraudulent charges for the Rocket Revenue Protection team to check, attaining predictive performances comparable to that of human analysts. My first deployment was a batch prediction process, but since then, our team has packaged the models into a realtime API so model-generated predictions can be smoothly integrated into the revenue protection team's workflow. 

### Concepts
In fraud modeling, I had to deal with **class imbalance, miscalibrated probabilities, nonstationarity of the response, and the cold-start problem**. Models were **gradient boosted forests**. Model performance was evaluated using **Area under the Precision-Recall Curve** and other metrics. For each transaction, we calculate **SHAP values** which allows revenue protection analysts to understand the drivers of model predictions on a transactional level. 

### Tools and Languages
Tools used in this project include **Airflow** to automate the running of pipelines, **AWS S3** to store pipeline output, **AWS Redshift** for data ingestion and replication for analytics, and **Chartio** for dashboarding.

The project is written in **Python** with the data queries in **PostgreSQL**. Database commands were executed using an internal database class built on the **psycopg2** package. **boto3** was used to access files held in AWS S3 and credentials held in the AWS ParameterStore. Data processing was done using custom classes built on **numpy** and **pandas**. Models were built using the **sklearn** and **imblearn** libraries, and packaged into Pipeline objects for deployment. Interpretability scores were calculated using the **shap** package.

The real-time deployment (which I do not take credit for!) uses **AWS Sagemaker** for model endpoint hosting, **Redis** for feature caching, and **AWS Lambda** for event-driven API responses.

## Rocketmiles Pricing Model (June 2018)
I was brought in to validate my coworker's work on Rocket's first crack at a pricing algorithm for our hotel rooms. The algorithm is built off of a logistic regression conversion model which takes in search result and user features and predicts an expected probability of conversion on that result. In doing so, I was able to fully derive the equation which uses the logistic regression coefficients and non-price features to find the price which maximizes expected profit, giving us the capability to quickly price all incoming search results. The profit-maximizing price can be decomposed nicely into a cost factor and a markup factor which the product owners can adjust.

### Concepts
This project involved a lot of mathematical and statistical concepts. I had to incorporate the derivative (with respect to price) of a **logistic regression** with **interaction effects** into the standard profit-maximizing equation. The solution involved solving a **product log** or **Lambert W** equation; I was able to decompose the profit-maximizing equation into a cost factor and a markup factor based on **customer price elasticities** given by the logistic regression factors. Because we incorporated **simple random undersampling** to correct for an extreme **class imbalance** problem, I had to incorporate a **probability calibration adjustment** into the logistic regression intercept.

### Tools and Languages
Coding made up only a small part of my work for this project; that said, I reproduced the project using **Python** using the **pandas**, **numpy**, and **sklearn**. I wrote a function to make **vectorised predictions** of the profit maximizing price as an alternative to the existing solution, a slow database lookup.

## Rocketmiles Error Backtester (July 2018 - August 2018)
Rocket does not organically source hotel rooms; it purchases room-nights wholesale from organizations called "providers". The pipelines facilitating this data flow are owned by many intermediaries and can break upstream of Rocket, causing errors to surface at the consumer level, resulting in failed page loads or charge attempts that severely impact the customer experience. In these situations, it was hypothesized that temporarily blocking providers or specific provider-hotel pairs until the upstream issue was solved could improve error rates.

To investigate this, I built a backtester which would simulate a rules-based approach to blocking problematic providers or provider-hotel pairs. Using it, I found that provider-hotel level blocking strategies could reduce error rates by 26% by giving up 2% of potential revenue, versus the provider hotel level strategies that were in deployment at the time which reduced error rates by 12.2% by giving up 7% of potential revenue.

### Concepts
Although not a statistical project and primarily a software engineering problem, backtester reported performance relied heavily on **precision** (error rate in blocked transactions) and **recall** (proportion of total errors that were blocked) metrics. Another important metric was the percent of non-errored transactions lost, or the **false discovery rate**. 

The backtester follows an **object-oriented** design with classes descending from an abstract BlockingRule class. **Lazy evaluation** was implemented in many functions where assignments would be delayed until necessary so that they could be done in batches; this improved runtime by over 100x. As usual, operations were vectorized wherever possible.

### Tools and Languages
The backtester was coded entirely in Python using the **numpy** and **pandas** libraries. 
