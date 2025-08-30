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

**3.	Positive Correlation Between Rating and Recommendation**

In general, higher product ratings are associated with a stronger likelihood of recommendation.
-	Products rated 4 or 5 stars are highly recommended.
-	Products rated 3 stars receive mixed opinions: the number of customers recommending versus not recommending is nearly balanced.

This suggests that a 3-star rating often reflects a neutral stance, highlighting an opportunity for Sephora to investigate and improve customer satisfaction for “average” products

<img width="550" height="552" alt="image" src="https://github.com/user-attachments/assets/e7b3c640-448c-47fa-a9cf-5b7c532d16c7" />

_Fig 5 Positive Correlation between Recommend and Rating_

**4.	Price Tier Analysis**

Based on the price distribution, most reviewed products fall within the lower price tier, primarily under $100. Higher-priced (i.e., prestige products) receive fewer reviews, possibly due to a smaller customer base for these premium items

<img width="550" height="532" alt="image" src="https://github.com/user-attachments/assets/4b244b38-13a9-4a61-b74c-9f2c5d46922b" />

_Fig 6 Most reviewed products fall within the lower price tier_

#### 5.3.	Product Dataset

The Product Dataset contains details on the product offering by Sephora. The features are outlined in Fig 7: Data Dictionary of Product Dataset.

<img width="550" height="638" alt="image" src="https://github.com/user-attachments/assets/3aaf9a69-e727-4814-bc57-074ae147918d" />
<img width="550" height="638" alt="image" src="https://github.com/user-attachments/assets/362dfc71-3be5-4fa0-9155-229ea311316c" />

_Fig 7 Data Dictionary of Product Dataset_

The key exploratory findings are summarized as follows:

**1.	Product Distribution**

Most products fall into major categories such as Skincare, Makeup, Hair, Fragrance, and Bath & Body, which primarily target women. Smaller but notable groups include Mini Size, Men, Tools & Brushes, and Gifts, which represent more niche markets.

<img width="550" height="573" alt="image" src="https://github.com/user-attachments/assets/6cff5931-08b5-4bc9-abe1-fc5698e2f1a4" />

_Fig 8 Product Distribution_

**2.	Product Popularity and Customer Engagement**

Products with high loves counts tend to cluster around an average rating of 4.5, suggesting they are both popular and well-liked. Products with low loves counts but high ratings (>4.5) may represent new, niche, or underexposed items with untapped growth potential.

Ratings between 4.0–5.0 are generally associated with higher review volumes, indicating a strong correlation between perceived quality and customer engagement. Sparse data (low ratings and few reviews) likely corresponds to unpopular or poorly marketed products.

<img width="550" height="309" alt="image" src="https://github.com/user-attachments/assets/8163cac0-2655-4822-939d-eebd2fb0e2f7" />

_Fig 9 Product Popularity and Customer Engagement_

**3.	Price and Customer Engagement**

Ratings between 4.0–4.5 are most common in the price band just below USD 250, suggesting this range reflects a balance between affordability and high quality.

Customer popularity (measured by loves count) peaks in the USD 20–125 range, highlighting strong engagement with mass-market products. Higher-priced items in the USD 250–500 range tend to receive lower loves counts, indicating they cater to niche or luxury segments with limited customer bases.

A significant outlier was identified: a product priced above USD 1,750, which warrants further investigation.

<img width="550" height="367" alt="image" src="https://github.com/user-attachments/assets/47adc1ff-cb92-4973-becf-b322be82a601" />

_Fig 10 Price and Customer Engagement_

## 6. Data Preparation

For my analysis, the Review Dataset was used as the primary dataset, while the Product Dataset was integrated as a secondary source to enrich the customer-level information

#### 6.1.	Merging of Dataset

A left join was performed on the two datasets using the unique product_id as the key. This ensured that all customer reviews were retained while incorporating additional product attributes such as product category, price, and size. Duplicate columns that appeared in both datasets (e.g., brand_name, brand_id) were removed to avoid redundancy.

#### 6.2.	Removal of Unnecessary Features

Since the focus of this study is on customer demographics and product preferences, several features were removed to streamline the dataset

Feature                                                                        | Reason for Removal             
---------------------                                                          | --------------------- 
Index column (unnamed:0)                                                       | Not relevant for analysis      
total_feedback_count, total_neg_feedback_count, and total_pos_feedback_count   | Replaced with the more concise helpfulness feature.
review_text, review_title, reviews, and avg_rating                             | Dropped to retain only the rating from individual reviewers.
Time-related columns (submission_time, submission_year, submission_month, submission_quarter) | Excluded as temporal analysis is beyond the scope of this project

<img width="450" height="650" alt="image" src="https://github.com/user-attachments/assets/5fb0406c-1e1d-4a21-b3c2-a542309ad8e0" />

_Fig 11 Features of Merged Dataset_

#### 6.3.	Missing Value

Several steps were taken to address missing values in the dataset to ensure data quality and reliability for segmentation.

<img width="150" height="901" alt="image" src="https://github.com/user-attachments/assets/775394fa-28de-4b5f-919c-c5a63381ef82" />

Fig 12 List of Features with Missing Values

##### 6.3.1.	Helpfulness 

For the helpfulness feature, defined as the ratio of total_pos_feedback_count to total_feedback_count, NaN values arose when no feedback was provided. These were replaced with 0 to indicate the absence of feedback.

##### 6.3.2.	Skin Tone and Skin Type

Missing values were also found in skin type (~1.6%) and skin tone (~4.0%). Since these are critical demographic characteristics for skincare analysis, reviews lacking this information were removed to preserve the integrity of the clustering results.

##### 6.3.3.	Eye Color and Hair Color

In contrast, eye color and hair color were considered less relevant for skincare-focused analysis and were therefore dropped entirely.

##### 6.3.4.	Size, Variation Type, Variation Value, Variation Desc, Child count
For product-related attributes, missing size values were replaced with “Not Found” to indicate unavailable information, while missing variation type values were filled with “Not Applicable” in cases where no product variations existed (i.e., child_count = 0). Finally, redundant fields such as variation value and variation description were removed to avoid duplication.

#### 6.3.5.	Ingredients and Highlights
Missing values were observed across various product categories for the ingredients and highlights features. Due to the inconsistent coverage of these fields, it was not possible to identify common patterns or association rules. To handle these missing entries, all null values were replaced with “Not Found” to indicate unavailable information, ensuring that the dataset remained complete and consistent for further analysis.

#### 6.4.	Feature Engineering

##### 6.4.1.	Product Category

During analysis of product categories, it was observed that the tertiary category was often insufficiently detailed. For example, Self Tanner products had vague tertiary labels such as “For Body” or “For Face,” which provided little distinction between product types.

<img width="540" height="88" alt="image" src="https://github.com/user-attachments/assets/4d7a9808-265f-42b9-a661-ef56a2d43263" />

_Fig 13 Vague tertiary labels_

To address this, the secondary and tertiary categories were combined to create more specific distinctions between products. 

Additionally, products with unclear or underpopulated categories were reclassified when appropriate; for instance, a product listed under “Shop by Concern” was reclassified under “Masks – Face Masks” to better reflect its function. 

After these adjustments, the original primary, secondary, and tertiary category columns were dropped from the dataset, as the refined categorization provided a more meaningful representation for analysis.

##### 6.4.2.	Skin Tone

The original skin tone feature contained a large number of categories, which could reduce the effectiveness of clustering. To simplify the data and improve segmentation, similar skin tone labels were reclassified into broader groups. Specifically, "fairLight", "light", and "porcelain" were grouped as "fair"; "lightMedium" and "mediumTan" were grouped as "medium"; and "deep", "olive", "rich", and "dark" were grouped as "tan". This consolidation reduced sparsity across categories, making the feature more suitable for K-Means clustering while still preserving meaningful demographic distinctions.

<img width="250" height="250" alt="image" src="https://github.com/user-attachments/assets/2fba1db1-500d-4bc3-b076-1b79ae641d9e" />








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
