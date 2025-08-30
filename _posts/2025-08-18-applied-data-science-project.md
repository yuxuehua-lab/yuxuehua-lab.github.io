---
layout: post
author: Hua Yu Xue
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## 1. Project Background
Sephora is a global leader in the beauty retail industry, offering thousands of products across categories such as skincare, haircare, and cosmetics. With over 2,700 stores in 35 countries and more than 500 stores across the Americas, alongside a world-class e-commerce platform, Sephora serves millions of customers worldwide.

Sephora was selected for this study for several reasons:
-	Real-world business relevance: As a major global retailer, Sephora continuously seeks to improve customer experience and maintain competitive advantage.
-	Diverse customer base: Sephora caters to a wide demographic range in terms of age, gender, location, skin types, and preferences.
-	Rich product ecosystem: Customers can choose from an extensive variety of products across multiple categories.
-	Engagement ecosystem: Sephora promotes interaction through customer reviews, ratings, and social features such as “loves” and helpful votes, creating a wealth of data for analysis.

## 2. Application Used
Python and various libraries were used for this analysis.

## 3.	Business Objective
Our group aims to enhance Sephora’s customer satisfaction, uncover customer preferences, and drive product development decisions through data-driven analysis through four areas:
-	**Product Optimization through Sentiment Analysis** – understanding how customers feel about products to guide development.
-	**Identifying Bottom 20 Products via XGBoost** – spotting underperforming products for potential improvement.
-	**Identifying Factors for High Engagement (loves_count) via XGBoost + SHAP** – uncovering drivers of product popularity.
-	**Customer Segmentation via K-Means** – enabling personalized and targeted marketing strategies.Individual 

## 4.	Individual Objective
While Sephora possesses a vast customer base and rich datasets, effectively translating this data into actionable insights remains a challenge. The problem is to identify patterns and behaviors within the customer data that can inform targeted marketing strategies, personalized recommendations, and strategic product development.

My individual goal is to identify distinct customer segments based on demographic characteristics, enabling more targeted marketing strategies that align with customer needs and support Sephora’s broader business goals

## 5.	Data Exploratory
#### 5.1.	Datasets
We have selected two comprehensive datasets from Kaggle:
-	**Products dataset** which contains product details such as brand name, size, stock availability etc for more than 8000 products offered by Sephora
-	**Reviews dataset** which contains more than 1 million reviews on 2000 products from year 2008 to year 2023

The original reviews dataset spans from 2008 to 2023. Since the 2023 dataset is incomplete and Sephora’s customer preferences evolve quickly, we focus on the 2021–2022 period, which better represents current trends. The refined dataset is still substantial and sufficient (i.e., total 394k transactions) for our analysis


Dataset               | Content               | Size                  | Time Period Used      | Reason for Selection  
--------------------- | --------------------- | --------------------- | --------------------- | ---------------------
Products              | Product details (brand, size, stock availability, etc.) for 8,000+ items         | More than 8000 products     | N/A | Provides product-level context for analysis
Reviews | 1M+ customer reviews on 2,000 products | 1M+ rows refined to 394k rows | 2008–2023 (filtered to 2021–2022) | Focus on recent trends; 2023 incomplete

<img width="250" height="250" alt="image" src="https://github.com/user-attachments/assets/e214270c-6a26-4ded-bf4f-e25722b99fdf" />

_Fig 1: Number of reviews by year and quarter. The data from 2023 is excluded as it is incomplete._

#### 5.2.	Review Dataset
The Review Dataset contains customer feedback on Sephora skincare products. The features are outlined in Fig 2: Data Dictionary of Review Dataset.

<img width="550" height="360" alt="image" src="https://github.com/user-attachments/assets/d4948ab4-2cfb-40f7-9110-5a45615c0d1d" />

_Fig 2 Data Dictionary of Review Dataset_

The key exploratory findings are summarized as follows:

**1.	Focus on Skincare Category**

The dataset contains reviews exclusively from the skincare category, with treatments, moisturizers, and cleansers being the most frequently reviewed product types.

<img width="550" height="286" alt="image" src="https://github.com/user-attachments/assets/00660d87-cc28-4974-91eb-3031739d102f" />

_Fig 3 Reviews count for each product category_

**2.	Predominantly Asian Customer Base**

Most reviewers have light to fair skin, brown eyes, and brown or black hair, with a majority reporting combination skin type. These characteristics suggest that the dataset largely represents Asian customers. This insight is valuable for designing region-specific marketing campaigns and product promotions that align with this demographic profile.

<img width="550" height="600" alt="image" src="https://github.com/user-attachments/assets/7c60e35b-2668-4523-961d-a7170cc2fb18" />

_Fig 4 Customer Demographic_



### Data Preparation
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

### Modelling
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

### Evaluation
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## Recommendation and Analysis
Explain the analysis and recommendations

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## AI Ethics
Discuss the potential data science ethics issues (privacy, fairness, accuracy, accountability, transparency) in your project. 

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce bibendum neque eget nunc mattis eu sollicitudin enim tincidunt. Vestibulum lacus tortor, ultricies id dignissim ac, bibendum in velit. Proin convallis mi ac felis pharetra aliquam. Curabitur dignissim accumsan rutrum. In arcu magna, aliquet vel pretium et, molestie et arcu. Mauris lobortis nulla et felis ullamcorper bibendum. Phasellus et hendrerit mauris. Proin eget nibh a massa vestibulum pretium. Suspendisse eu nisl a ante aliquet bibendum quis a nunc. Praesent varius interdum vehicula. Aenean risus libero, placerat at vestibulum eget, ultricies eu enim. Praesent nulla tortor, malesuada adipiscing adipiscing sollicitudin, adipiscing eget est.

## Source Codes and Datasets
Upload your model files and dataset into a GitHub repo and add the link here. 
