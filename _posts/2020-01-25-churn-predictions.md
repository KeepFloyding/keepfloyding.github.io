---
layout: post
title: Using Clickstream Data to Predict User Churn
date: 2020-01-25 00:00:00 +0100
categories: [Machine Learning, Supervised Algorithms, Classification]
tags: [Machine Learning, Random Forest, Supervised Learning]
seo:
  date_modified: 2020-01-25 17:13:24 +0000
---

Happy 2020! When thinking about my next blog post, I thought that it would be good to talk about my time as a data scientist in Isaac Physics. When I was there, we did some cool stuff in trying to define and solve open-ended problems, and found that with simple machine learning algorithms, we were able to do a lot! Having a structured and clean data source like they had can really take you far. 

# Introduction 

To those of you who don't know, Isaac Physics (IP) is an online educational platform that aims to improve STEM education throughout the country by providing students with the free resources needed to consolidate their understanding of Physics and Mathematics. Check out their slick website at www.isaacphysics.org.

![Alt Text](https://keepfloyding.github.io/images/IP_website.png)

# Data-driven Research: Problem of User Churn

Over the last few years, IP has received over 60,000 registered users and has recorded over 8 million question attempts. What I was most impressed with, is that all of this clickstream data was captured in a structured and easy to read manner in a SQL table. This made data easy to query and explore. Now one of the challenges that we faced was to define a problem that we could attempt to solve with our data science tools. We decided to try to tackle a problem that is endemic to many online platforms, the issue of user retention. After registration, 30% of users never returned to the platform after one month of activity. 

![Alt Text](https://keepfloyding.github.io/images/IP_user_retention.png)

Now I initially thought that with the clickstream data that we had at hand, that we would be able to easily determine who were the users likely to stop using the platform. However, it is much harder than I thought. For instance, you can see that those users who do a lot of question attempts aren't necessarily going to stay on the platform. 

![Alt Text](https://keepfloyding.github.io/images/IP_QA.png)

# Importance of Teachers

An important additional feature of the IP platform is the ability of teachers to enroll their pupils and to set homework problems for them to complete. The teachers can track the progress of their pupils and identify if there are any problems in understanding any of the content matter. On IP, this means that we can do more feature engineering for each user by not only determining their own clickstream data, but also incorporating the clickstream data of their teachers. 

![Alt Text](https://keepfloyding.github.io/images/IP_teacher_features.png)

Using these newly identified features, we were capable of determining with an 87% accuracy whether a user would stay on the platform or leave using a Random Forest Classifier. A strong advantage in using this algorithm lies in its ability to determine the strength of each feature in calculating user churn. It was clear, that the key features involved were those affiliated with the teacher rather than the user. This means that the teachers are the stars of the show and are instrumental in ensuring that students keep on using the IP platform.

![Alt Text](https://keepfloyding.github.io/images/IP_ML_results.png)

# Feeding Insight into Action

So what can we do with this information? It is clear that by ensuring the co-operation of the teachers, that we can encourage the pupils to actively use the platform. With the teacher features that we determined, and using unsupervised learning algorithms, we can identify which teachers are likely to be inactive. These teachers can then be offered help in learning how best to use the platform to help teach their pupils basic scientific knowledge. 

![Alt Text](https://keepfloyding.github.io/images/IP_unsuper.png)

IP runs teacher workshops to help teachers better use the platforms. Now with data science and machine learning, they can help users to better benefit from their platform and increase interest in STEM subjects nationwide. 




