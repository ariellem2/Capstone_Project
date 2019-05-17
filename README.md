# Capstone Project - Predicting Garment Fit

**Arielle Miro** 

### EXECUTIVE SUMMARY

---

Data Science Problem: 

Consumer preference-based return reasons (e.g., size, fit, style, etc.) tend to drive around 72% of all returns in fashion product categories. Incorrect sizing accounts for over 50% of returns in the e-commerce retail space. (https://www.shopify.com/enterprise/ecommerce-returns).

How can we combat the mass "returns" culture to retain business and minimize loss? 

1. Battle the plague of return rates through smart sizing predictions.
2. Increase revenue and enhance user experience with smart product recommendations. 


### Introduction

**Objective**: The goal is to accurately predict whether a garment fits (true to size) in order to optimize inventory, train recommender systems, and ensure client retention. 

---

### Overview
 
**Tools**
- Python 
- Tableau

**Data**
Source: The original JSON file scraped from the website was found here. http://cseweb.ucsd.edu/~jmcauley/datasets.html#clothing_fit.

The data was scraped from Rent the Runway. https://www.renttherunway.com/. 

Size:
- 192,544 Observations 
- Originally 12 features: age, body_type, bust_size, category, fit, height, item_id, rating, rented_for, review_date, review_summary, review_text, size, user_id, weight  
- For computational power purposes, a 20% sample of the data set was used to train the various models. 

Attributes:
- User
- Item 
- Category
- Occasion
- Size
- Body Type
- Weight
- Height 
- Age
- Review Summary 
- Full Review

---

### Data Cleaning 
    
**General**
- Due to the size of the dataset, I proceeded with dropping null values
- Adjusted column names to have underscores
- Adjusted datatypes for possible integers and floats
- Adjusted the predictions column "fit" to be numeric for modeling

**For Numeric**
- Dummied the categorical columns 

**For NLP**
- Combined review summary and review text into one column 
- Made all text lowercase
- Removed all puntuation and numbers 
- Lemmatized

At this point, there are two separate clean data frames ready for modeling: 
1. User & Sizing (categorical/numeric)
2. Rewviews (NLP)

---

### EDA

Linked to Tableau Dashboards for Visualizations:

https://public.tableau.com/profile/arielle7797#!/vizhome/Capstone_Rent_the_Runway/Understandingthedata
https://public.tableau.com/profile/arielle7797#!/vizhome/Capstone_EDA_2/AdditionalEDA

Key insights from the visualizations:
- A majority of the garments fit true to size (about 73%). 
- The most popular category amongst users is dresses.
- Dresses were most often rented for weddings or formal parties. 

---

### Models 

**This is a Multiclass Classification Prediction**

First, it's important to note that the classes we're highly unbalanced. The breakdown goes as follows:

0/Small : 13%
1/Fit : 73%
2/Large : 14%

**With the use of a modeling function, I ran several basic models.**

1. User & Sizing  
- The first model included the sizing and cateogrical data. 
- Random Forests performed best with an accuracy score of 98% and minimal overfitting.  
- The AUC ROC score was 96%. 
- Although the initial score was strong, I gridsearched through the random forests to find the best parameters.  

How did I address the unbalanced classes? 
- Smote was used for oversampleing/undersampling. 
- Once the classes were balanced I ran a Gradient Boosting model, resulting in AUC ROC score of 80%. 

Because the initial random forests model performed better in accuracy and AUC ROC, I proceeded with that. 


2. NLP
- The second model included the text data from user reviews of the garments. 
- Similar to the first model, Random Forests performed best with both CountVectorizer and TfidfVectorizer. 
- The accuracy score again was 98%. 

3. Combined Data 
- The third model will include the binary data from model 1 and the vectorized data from model 2.
- Due to the computational issues, the third model has not been run yet. The cleaned and combined dataframe can be found in the repo. 
- As a next step, I intend to run the model using cloud computing in order to compare the scores. 

---

### Recommenders

As the second step to solving the business problem, I created two basic collaborative recommenders. Both recommenders were created using parawise distanced to calculate the cosine similarity between users and/or items. 

1. User Based - In this particilar case, user profiles are not based on preferences/style. Rather it aggregates data on physical attributes of the user. With respect to retail/garments, retailers will often carry multiple sizes of the same garment, therefore the user based recommender here may not work. 

2. Item Based - Given that the data contains little detail regarding the item (i.e., description, fabric, images, etc.), the item-based recommender is working off the categorical data such as "rented for" and "category". 

Due to the above shortcomings of the recommenders, a next step is to build a content based recommender that incorporates the item reviews. 

---

### Conclusions 

**Next Steps**
- Use AWS Run a combined model and compare performace 
- Use AWS to run the various models on all the data rather than just a 20% sample
- Sentiment Analysis on garment reviews 
- Content based recommender on garment reviews

**Additional Useful Data to Collect**
- Purchase Amount - to predict revenue and/or loss
- User Location - to build up user profiles more 
- Item description - to retrieve additional detail for recommendations 


