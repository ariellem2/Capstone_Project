# Capstone Project - Predicting Garment Fit

**Arielle Miro** 

### EXECUTIVE SUMMARY

---

Data Science Problem: 

Consumer preference-based return reasons (e.g., size, fit, style, etc.) tend to drive around 72% of all returns in fashion product categories. Incorrect sizing accounts for over 50% of returns in the e-commerce retail space. (https://www.shopify.com/enterprise/ecommerce-returns).

How can we combat the mass "returns" culture to retain business and minimize loss? 

1. Battle the plague of return rates through smart sizing predictions.
2. Increase revenue and enhance user experience with smart product recommendations. 

Current retail e-commerce sites provide sizing charts, however many consumers do not know their exact measurements and therefore do not make purchase decisions based off the sizing charts. 


### Introduction

**Objective**: The goal is to accurately predict whether a garment fits (true to size) in order to optimize inventory, train recommender systems, and ensure client retention. 

---

### Overview
 
**Tools**
- Python 
- Tableau

**Data**
Source: The original JSON file scraped from the website was found here. http://cseweb.ucsd.edu/~jmcauley/datasets.html#clothing_fit.

The data was scraped from Rent the Runway. https://www.renttherunway.com/. Note that Rent the Runway's business model allows consumers to rent clothes rather than purchase them. They allow 1 additional backup size of a specific garment. Specific to Rent the Runway, if fit predictions were accurate enough, the back up garments could potentially be rented to other consumers and bring in more revenue. 

For most other retail sites that sell clothes (rather than rent), the issue of mass returns plagues them even more. 

Data Size:
- 192,544 Observations 
- Originally 12 features: age, body_type, bust_size, category, fit, height, item_id, rating, rented_for, review_date, review_summary, review_text, size, user_id, weight  
- For computational power purposes, a 20% sample of the data set was used to train the various models. 

Data Attributes:
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
- Adjusted datatypes to change size measurements to integers and/or floats
- Adjusted the predictions column "fit" to be numeric for modeling

**For Numeric**
- Dummied the categorical columns (rented_for, body_type, category)

**For NLP**
- Combined review summary and review text into one column 
- Made all text lowercase
- Removed all puntuation and numbers 
- Lemmatized (this was eventually removed for better performance)

At this point, there are two separate clean data frames ready for modeling: 
1. User & Sizing (categorical/numeric)
2. Rewviews (NLP)

---

### EDA

Link to Tableau Dashboards for Visualizations:

https://public.tableau.com/profile/arielle7797#!/vizhome/Capstone_Rent_the_Runway/Understandingthedata
https://public.tableau.com/profile/arielle7797#!/vizhome/Capstone_EDA_2/AdditionalEDA

Key insights from the visualizations:
- A majority of the garments fit true to size (about 73%). 
- The most popular category amongst users is dresses.
- Dresses were most often rented for weddings or formal parties. 
- The company's user base remains steady over time. 

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
- Random Forests has the highest Train accuracy score of 98%, however when applied to the Test data the model was very overfit (71%). Polynomial features and Gridsearch were used in an attempt to increase the Test score and reduce overfitness. 

How did I address the unbalanced classes? 
- Smote was used for oversampleing/undersampling. 
- Once the classes were balanced I ran a Gradient Boosting model. The model with the best score and minimal overfitting was Gradient Boosting with 74%. After gridsearching, the score did not improve much. 

Because the numeric model did not perform much stronger than the baseline, I proceeded with testing an NLP model. 

2. NLP
- The second model included the text data from user reviews of the garments. 
- Similar to the first model, Random Forests performed best with both CountVectorizer and TfidfVectorizer, however the model remained highly overfit with Train scores of ~98% and Test score of ~72%.

- The best performing NLP model was the Gradient Boosting model using Count Vectorizer. The Train score was 82% and the Test score of 80%. 

3. Combined Data 
- The third model will include the binary data from model 1 and the vectorized text data from model 2.
- Due to the computational issues, the third model has not been run yet. The cleaned and combined dataframe can be found in the repo. 
- As a next step, I intend to run the model using cloud computing in order to compare the scores. 

---

### Recommenders

As the second step to solving the business problem, I created two basic collaborative recommenders. Both recommenders were created using parawise distance to calculate the cosine similarity between users and/or items. 

1. User Based - In this particilar case, user profiles are not based on preferences/style. Rather it aggregates data on physical attributes of the user. With respect to retail/garments, retailers will often carry multiple sizes of the same garment, therefore the user based recommender here may not work. 

2. Item Based - Given that the data contains little detail regarding the item (i.e., description, fabric, images, etc.), the item-based recommender is working off the categorical data such as "rented for" and "category". 

Due to the above shortcomings of the recommenders, I built a content based recommender that incorporates the item reviews.

3. Content Based - This recommender is built off a matrix of the vectorized text data from the item reviews. Because it is based off text data, the item recommendations for a single item range across size, category, measurements, etc. 

---

### Conclusions 

Based on the model and recommender exploration, the NLP results performed "better". In what way and what does this mean?

- Fit Predictions: The NLP model reached a higher accuracy score. Given that many retail sites currently have sizing charts and still suffer from mass returns, NLP might be an interesting angle to approach size predictions. 

- Product Recommender: 
 - Using NLP captured user preferences that general size and fit data could not. 
 - Underlying item similarities were rooted in text reviews. 

**Next Steps**
- Use AWS Run a combined model and compare performace 
- Use AWS to run the various models on all the data rather than just a 20% sample
- Sentiment Analysis on garment reviews 
- Content based recommender on garment reviews

**Additional Useful Data to Collect**
- Purchase Amount - to predict revenue and/or loss
- User Location - to build up user profiles more 
- Item description - to retrieve additional detail for recommendations 


