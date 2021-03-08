---
title: "Ranking at Rocket"
excerpt: "[Slide deck](../../files/rocket_ranking_slides.pdf) of the first ranking model deployed at Rocketmiles"
collection: ranking
---

[Slide deck link](../../files/rocket_ranking_slides.pdf)

I made this slide deck for an internal presentation at Rocket on how we approached the ranking problem and how we evaluated the performance of our ranking model both offline and on an online AB test. Luckily, not much editing had to be done to make this available to the public.

Some figures are outdated as of 2021:
1. We moved our hosting stack from AWS Sagemaker to Kubernetes. This allowed us to deploy the ranking model both on our own AWS stack and on our sister company's servers simultaneously. 
2. We swapped from Spark, which built the training set from scratch each time, to a dbt-based data pipeline in Redshift. This allowed us to use the same data for both training as well as populating the production cache features.

After testing on multiple products, we declared the ranking model a great success! It continues to predict on all hotel searches today; feel free to [make one yourself](https://www.rocketmiles.com)!