---
layout: post
author: Hua Yu Xue
title: "Applied Data Science Project Documentation"
categories: ITD214
---
## **1. Project Background**
Sephora is a global leader in the beauty retail industry, offering thousands of products across categories such as skincare, haircare, and cosmetics. With over 2,700 stores in 35 countries and more than 500 stores across the Americas, alongside a world-class e-commerce platform, Sephora serves millions of customers worldwide.

Sephora was selected for this study for several reasons:
-	Real-world business relevance: As a major global retailer, Sephora continuously seeks to improve customer experience and maintain competitive advantage.
-	Diverse customer base: Sephora caters to a wide demographic range in terms of age, gender, location, skin types, and preferences.
-	Rich product ecosystem: Customers can choose from an extensive variety of products across multiple categories.
-	Engagement ecosystem: Sephora promotes interaction through customer reviews, ratings, and social features such as “loves” and helpful votes, creating a wealth of data for analysis.

## **2. Application Used**
Python and various libraries were used for this analysis.

## **3.	Business Objective**
Our group aims to enhance Sephora’s customer satisfaction, uncover customer preferences, and drive product development decisions through data-driven analysis from four areas:
-	**Product Optimization through Sentiment Analysis** – understanding how customers feel about products to guide development.
-	**Identifying Bottom 20 Products via XGBoost** – spotting underperforming products for potential improvement.
-	**Identifying Factors for High Engagement (loves_count) via XGBoost + SHAP** – uncovering drivers of product popularity.
-	**Customer Segmentation via K-Means** – enabling personalized and targeted marketing strategies.Individual 

## **4.	Individual Objective**
While Sephora possesses a vast customer base and rich datasets, effectively translating this data into actionable insights remains a challenge. The problem is to identify patterns and behaviors within the customer data that can inform targeted marketing strategies, personalized recommendations, and strategic product development.

My individual goal is to identify distinct customer segments based on demographic characteristics, enabling more targeted marketing strategies that align with customer needs and support Sephora’s broader business goals

## **5.	Data Exploratory**
#### **5.1.	Datasets**

We have selected two comprehensive datasets from Kaggle:
-	**Products dataset** which contains product details such as brand name, size, stock availability etc for more than 8000 products offered by Sephora
-	**Reviews dataset** which contains more than 1 million reviews on 2000 products from year 2008 to year 2023

The original reviews dataset spans from 2008 to 2023. Since the 2023 dataset is incomplete and Sephora’s customer preferences evolve quickly, we focus on the 2021–2022 period, which better represents current trends. The refined dataset is still substantial and sufficient (i.e., total 394k transactions) for our analysis


Dataset               | Content               | Size                  | Time Period Used      | Reason for Selection  
--------------------- | --------------------- | --------------------- | --------------------- | ---------------------
Products              | Product details (brand, size, stock availability, etc.) for 8,000+ items         | More than 8000 products     | N/A | Provides product-level context for analysis
Reviews | 1M+ customer reviews on 2,000 products | 1M+ rows refined to 394k rows | 2008–2023 (filtered to 2021–2022) | Focus on recent trends; 2023 incomplete

<img width="250" height="250" alt="image" src="https://github.com/user-attachments/assets/e214270c-6a26-4ded-bf4f-e25722b99fdf" />

###### _Fig 1: Number of reviews by year and quarter. The data from 2023 is excluded as it is incomplete._

#### **5.2.	Review Dataset**

The Review Dataset contains customer feedback on Sephora skincare products. The features are outlined in Fig 2: Data Dictionary of Review Dataset.

<img width="550" height="360" alt="image" src="https://github.com/user-attachments/assets/d4948ab4-2cfb-40f7-9110-5a45615c0d1d" />

###### _Fig 2 Data Dictionary of Review Dataset_

The key exploratory findings are summarized as follows:

##### **5.2.1.	Focus on Skincare Category**

The dataset contains reviews exclusively from the skincare category, with treatments, moisturizers, and cleansers being the most frequently reviewed product types.

<img width="550" height="286" alt="image" src="https://github.com/user-attachments/assets/00660d87-cc28-4974-91eb-3031739d102f" />

###### _Fig 3 Reviews count for each product category_

##### **5.2.2. Predominantly Asian Customer Base**

Most reviewers have light to fair skin, brown eyes, and brown or black hair, with a majority reporting combination skin type. These characteristics suggest that the dataset largely represents Asian customers. This insight is valuable for designing region-specific marketing campaigns and product promotions that align with this demographic profile.

<img width="700" height="600" alt="image" src="https://github.com/user-attachments/assets/7c60e35b-2668-4523-961d-a7170cc2fb18" />

###### _Fig 4 Customer Demographic_

##### **5.2.3. Positive Correlation Between Rating and Recommendation**

In general, higher product ratings are associated with a stronger likelihood of recommendation.
-	Products rated 4 or 5 stars are highly recommended.
-	Products rated 3 stars receive mixed opinions: the number of customers recommending versus not recommending is nearly balanced.

This suggests that a 3-star rating often reflects a neutral stance, highlighting an opportunity for Sephora to investigate and improve customer satisfaction for “average” products

<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/e7b3c640-448c-47fa-a9cf-5b7c532d16c7" />

###### _Fig 5 Positive Correlation between Recommend and Rating_

##### **5.2.4. Price Tier Analysis**

Based on the price distribution, most reviewed products fall within the lower price tier, primarily under $100. Higher-priced (i.e., prestige products) receive fewer reviews, possibly due to a smaller customer base for these premium items

<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/4b244b38-13a9-4a61-b74c-9f2c5d46922b" />

###### _Fig 6 Most reviewed products fall within the lower price tier_

#### **5.3.	Product Dataset**

The Product Dataset contains details on the product offering by Sephora. The features are outlined in Fig 7: Data Dictionary of Product Dataset.

<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/3aaf9a69-e727-4814-bc57-074ae147918d" />
<img width="700" height="500" alt="image" src="https://github.com/user-attachments/assets/362dfc71-3be5-4fa0-9155-229ea311316c" />

###### _Fig 7 Data Dictionary of Product Dataset_

The key exploratory findings are summarized as follows:

##### **5.3.1.	Product Distribution**

Most products fall into major categories such as Skincare, Makeup, Hair, Fragrance, and Bath & Body, which primarily target women. Smaller but notable groups include Mini Size, Men, Tools & Brushes, and Gifts, which represent more niche markets.

<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/6cff5931-08b5-4bc9-abe1-fc5698e2f1a4" />

###### _Fig 8 Product Distribution_

##### **5.3.2.	Product Popularity and Customer Engagement**

Products with high loves counts tend to cluster around an average rating of 4.5, suggesting they are both popular and well-liked. Products with low loves counts but high ratings (>4.5) may represent new, niche, or underexposed items with untapped growth potential.

Ratings between 4.0–5.0 are generally associated with higher review volumes, indicating a strong correlation between perceived quality and customer engagement. Sparse data (low ratings and few reviews) likely corresponds to unpopular or poorly marketed products.

<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/8163cac0-2655-4822-939d-eebd2fb0e2f7" />

###### _Fig 9 Product Popularity and Customer Engagement_

##### **5.3.3.	Price and Customer Engagement**

Ratings between 4.0–4.5 are most common in the price band just below USD 250, suggesting this range reflects a balance between affordability and high quality.

Customer popularity (measured by loves count) peaks in the USD 20–125 range, highlighting strong engagement with mass-market products. Higher-priced items in the USD 250–500 range tend to receive lower loves counts, indicating they cater to niche or luxury segments with limited customer bases.

A significant outlier was identified: a product priced above USD 1,750, which warrants further investigation.

<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/47adc1ff-cb92-4973-becf-b322be82a601" />

###### _Fig 10 Price and Customer Engagement_

## **6. Data Preparation**

For my analysis, the Review Dataset was used as the primary dataset, while the Product Dataset was integrated as a secondary source to enrich the customer-level information

#### **6.1.	Merging of Dataset**

A left join was performed on the two datasets using the unique product_id as the key. This ensured that all customer reviews were retained while incorporating additional product attributes such as product category, price, and size. Duplicate columns that appeared in both datasets (e.g., brand_name, brand_id) were removed to avoid redundancy.

#### **6.2.	Removal of Unnecessary Features**

Since the focus of this study is on customer demographics and product preferences, several features were removed to streamline the dataset

Feature                                                                        | Reason for Removal             
---------------------                                                          | --------------------- 
Index column (unnamed:0)                                                       | Not relevant for analysis      
total_feedback_count, total_neg_feedback_count, and total_pos_feedback_count   | Replaced with the more concise helpfulness feature.
review_text, review_title, reviews, and avg_rating                             | Dropped to retain only the rating from individual reviewers.
Time-related columns (submission_time, submission_year, submission_month, submission_quarter) | Excluded as temporal analysis is beyond the scope of this project

<img width="450" height="650" alt="image" src="https://github.com/user-attachments/assets/5fb0406c-1e1d-4a21-b3c2-a542309ad8e0" />

###### _Fig 11 Features of Merged Dataset_

#### **6.3.	Missing Value**

Several steps were taken to address missing values in the dataset to ensure data quality and reliability for segmentation.

<img width="250" height="650" alt="image" src="https://github.com/user-attachments/assets/775394fa-28de-4b5f-919c-c5a63381ef82" />

###### _Fig 12 List of Features with Missing Values_

##### **6.3.1.	Helpfulness** 

For the helpfulness feature, defined as the ratio of total_pos_feedback_count to total_feedback_count, NaN values arose when no feedback was provided. These were replaced with 0 to indicate the absence of feedback.

##### **6.3.2.	Skin Tone and Skin Type**

Missing values were also found in skin type (~1.6%) and skin tone (~4.0%). Since these are critical demographic characteristics for skincare analysis, reviews lacking this information were removed to preserve the integrity of the clustering results.

##### **6.3.3.	Eye Color and Hair Color**

In contrast, eye color and hair color were considered less relevant for skincare-focused analysis and were therefore dropped entirely.

##### **6.3.4.	Size, Variation Type, Variation Value, Variation Desc, Child count**
For product-related attributes, missing size values were replaced with “Not Found” to indicate unavailable information, while missing variation type values were filled with “Not Applicable” in cases where no product variations existed (i.e., child_count = 0). Finally, redundant fields such as variation value and variation description were removed to avoid duplication.

##### **6.3.5.	Ingredients and Highlights**
Missing values were observed across various product categories for the ingredients and highlights features. Due to the inconsistent coverage of these fields, it was not possible to identify common patterns or association rules. To handle these missing entries, all null values were replaced with “Not Found” to indicate unavailable information, ensuring that the dataset remained complete and consistent for further analysis.

#### **6.4.	Feature Engineering**

##### **6.4.1.	Product Category**

During analysis of product categories, it was observed that the tertiary category was often insufficiently detailed. For example, Self Tanner products had vague tertiary labels such as “For Body” or “For Face,” which provided little distinction between product types.

<img width="350" height="90" alt="image" src="https://github.com/user-attachments/assets/4d7a9808-265f-42b9-a661-ef56a2d43263" />

###### _Fig 13a Vague tertiary labels_

To address this, the secondary and tertiary categories were combined to create more specific distinctions between products. 

Additionally, products with unclear or underpopulated categories were reclassified when appropriate; for instance, a product listed under “Shop by Concern” was reclassified under “Masks – Face Masks” to better reflect its function. 

After these adjustments, the original primary, secondary, and tertiary category columns were dropped from the dataset, as the refined categorization provided a more meaningful representation for analysis.

<img width="350" height="90" alt="image" src="https://github.com/user-attachments/assets/68b43534-12d0-4c8a-89fe-ab8c19158969" />

###### _Fig 13b Refined Product Category labels_


##### **6.4.2.	Skin Tone**

The original skin tone feature contained a large number of categories, which could reduce the effectiveness of clustering. To simplify the data and improve segmentation, similar skin tone labels were reclassified into broader groups. Specifically, "fairLight", "light", and "porcelain" were grouped as "fair"; "lightMedium" and "mediumTan" were grouped as "medium"; and "deep", "olive", "rich", and "dark" were grouped as "tan". This consolidation reduced sparsity across categories, making the feature more suitable for K-Means clustering while still preserving meaningful demographic distinctions.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/2fba1db1-500d-4bc3-b076-1b79ae641d9e" />

##### **6.4.3.	Standardize Size Measurement**

The dataset contains product sizes expressed in different units, including "ml", "g", "oz", and "fl oz", “ounce” etc.  To ensure consistency across products, all measurements were converted to milliliters (ml). 

Subsequently, a unit price per ml was calculated by dividing the product’s selling price by its size in milliliters. This standardization allows for meaningful comparisons of price across products of different sizes and ensures that size-related features are appropriately scaled for clustering and analysis.

<img width="350" height="250" alt="image" src="https://github.com/user-attachments/assets/8b4e14c9-4d9b-4646-8c0b-8c99f16f98d8" />

###### _Fig 14 Examples of Refined Size_

##### **6.4.4.	First Draft of Dataset After Cleaning**

 <img width="350" height="550" alt="image" src="https://github.com/user-attachments/assets/5d8e6c52-c54c-4288-a51b-cc4730a563d0" />

###### _Fig 15  Dataset after data preparation_

## 7. Model Design and Assessment

#### **7.1.	Model Selection – K Means Clustering Model**

The selection of the model was guided by three key criteria: the project objective, the size of the dataset, and the types of features available. The goal of this analysis is to identify distinct customer segments using unlabelled data, with a dataset containing over 380,000 reviews from 2021 and 2022. The dataset includes a mix of continuous, binary, and categorical features, which informed the choice of a suitable clustering algorithm.

<img width="350" height="350" alt="image" src="https://github.com/user-attachments/assets/e4f570b2-dfd7-4aed-9bc3-8042b0504365" />

###### _Fig 16  Model Selection Criteria_

After evaluating potential methods, K-Means clustering was selected due to its advantages for this task:

**1.	Segmentation Capability**

K-Means effectively groups customers with similar behaviors and demographics into distinct clusters, aligning with the project’s objective of identifying actionable customer segments.

**2.	Scalability and Efficiency**

The algorithm is computationally efficient, making it well-suited for large datasets like ours.

**3.	Flexibility**

K-Means works effectively with continuous variables and can accommodate categorical features once they are appropriately encoded and standardized.

**4.	Simplicity and Interpretability**

The model is easy to understand and explain: each customer is assigned to one of k clusters based on feature similarity.

**5.	Actionable Insights**

The resulting clusters provide clear, actionable groups that can inform targeted marketing, personalization strategies, and broader busine

#### **7.2.	Data scaling & preprocessing for K-Means**

Before applying K-Means clustering, the dataset was preprocessed to ensure that all features were in a format suitable for distance-based clustering. The algorithm relies on Euclidean distance, which can be disproportionately influenced by features with larger scales or inconsistent encodings. To address this, several steps were performed:

##### **7.2.1.	Encoding of Categorical Variables**

Categorical attributes such as skin type and skin tone were transformed into numerical representations through one-hot encoding. This allowed the clustering algorithm to interpret categorical differences without introducing artificial ordinal relationships.

##### **7.2.2.	Standardization of Continuous Features**

Continuous features such as price per ml, rating, and helpfulness were standardized using z-score normalization. This ensured that all features had a mean of 0 and a standard deviation of 1, preventing features with larger numerical ranges (e.g., price) from dominating the distance calculations.

### **7.3 Modelling and Parameter Modelling**

#### **7.3.1.	First Model Attempt – Full Features**

The first K-Means clustering model was built using the full set of features without dimensionality reduction. However, several issues were encountered during this stage. The model was computationally intensive and crashed multiple times due to the high dimensionality of the dataset.

An analysis of the variance structure using Principal Component Analysis (PCA) showed that between 37 and 40 components were required to explain just 80% of the total variance. This indicated that the dataset contained a large number of weakly correlated features, making it difficult for K-Means to identify clear cluster boundaries.

The elbow graph generated from this model was nearly a straight line, further confirming the lack of strong natural clusters in the unprocessed, high-dimensional feature space. These findings suggested that feature reduction or selection would be necessary to improve clustering performance and achieve more interpretable customer segments.

#### **7.3.2.	Second Model Attempt – Refined Features**

After observing the limitations of the first model, a second K-Means clustering model was built using a refined set of features focused on customer-centric variables. The feature set was structured into three main categories:

-	**Customer Characteristics**: skin_tone, skin_type
-	**Customer Purchasing Behavior**: unit_price, size_ml, online_only, limited_edition, new, sephora_exclusive
-	**Customer Engagement**: rating, helpfulness, loves_count
  
This refined feature selection aimed to remove weakly correlated or redundant attributes and emphasize variables directly linked to customer profiles and behaviors.

The refined model showed some improvement compared to the initial attempt. PCA revealed that only 10–12 components were required to explain 80–90% of the total variance, a significant reduction from the 37–40 components needed in the first model.

<img width="700" height="550" alt="image" src="https://github.com/user-attachments/assets/405923ac-2d09-4aca-96ba-616049dad600" />

###### _Fig 17 Principal Component Analysis (PCA) for second Model_

However, the elbow method still produced a relatively straight graph, suggesting that strong natural clustering tendencies remained limited in the dataset.

<img width="700" height="550" alt="image" src="https://github.com/user-attachments/assets/262dfa90-d94b-4de5-a5c9-fa80e84ddee6" />

###### _Fig 18 Elbow Graph for Second Model_

#### **7.3.3.	Third Model Attempt – Aggregation**

The third model was developed using refined features combined with an aggregation strategy to better align with the project’s objective of customer segmentation.

**1.	Reason for Aggregation**

The original dataset was structured at the review–product level, where each row represents a review of a product by a customer. Using this structure directly for clustering presents several challenges:

-	K-Means would cluster products/reviews rather than customers, resulting in product-centric rather than customer-centric insights.
-	A single customer who reviewed multiple products could be assigned to different clusters, leading to fragmented and inconsistent profiling.
  
Insights generated would therefore be diluted and less actionable for targeted marketing.

**2.	Aggregation Approach**

To overcome these issues, the dataset was aggregated to the customer level.

<img width="900" height="700" alt="image" src="https://github.com/user-attachments/assets/8eb5fdfb-9860-4e59-abb0-2665d94d2238" />

This approach summarized customer behaviour by consolidating purchase and engagement patterns to better capture overall preferences. It also removed duplication and thus ensured that each customer is represented by a single row, creating a dataset suitable for clustering.

This aggregation ensured that the K-Means model clusters were focused on customers rather than products, enabling meaningful segmentation for personalized marketing strategies

## 8. Evaluation - Optimal number of clusters

#### **8.1.	PCA**

PCA was again performed on the aggregated dataset to reduce dimensionality prior to clustering. The results showed that only 8 to 9 components were required to explain 80–90% of the total variance which was a significant improvement compared to the first and second models, which required 37–40 and 10–12 components respectively.

<img width="700" height="550" alt="image" src="https://github.com/user-attachments/assets/09d010c8-ab58-4033-996b-9826d516f520" />

###### _Fig 19 PCA for Third Model_

This reduction in dimensionality indicates that there is stronger feature representation with more informative variables driving the clustering and reduced noise leading to clearer and more robust segmentation outcomes.

#### **8.2.	Elbow Graph**

The Elbow Method was applied, however, the resulting curve appeared relatively straight with no distinct inflection point, making it difficult to determine an optimal number of clusters.

<img width="860" height="584" alt="image" src="https://github.com/user-attachments/assets/f06b2096-822f-4f7d-a2ff-59041b551012" />

###### _Fig 18 Elbow Graph for Second Model_

This outcome suggests that the dataset may not exhibit strong, naturally separable cluster structures. As a result, the Elbow Method alone is insufficient for selecting k in this case, and complementary approaches are required to guide the choice of the optimal number of clusters.

#### **8.3.	Silhouette method**

The Silhouette Method was applied to evaluate clustering performance and identify the optimal number of k clusters. Since the objective is customer segmentation, the number of clusters was capped at fewer than six to avoid excessive fragmentation and diluted customer profiling.

The analysis revealed that the overall Silhouette scores were relatively low, suggesting that clusters have a degree of overlap. This indicates that customer behaviours are not sharply separable, likely due to customers shopping across multiple categories and exhibiting overlapping preferences.

<img width="300" height="233" alt="image" src="https://github.com/user-attachments/assets/ca087695-62f1-4eed-baa8-f45f94b35554" />

###### _Fig 21 Silhouette Score for Third Model_

Among the tested values, 5 and 6 clusters yielded the highest Silhouette scores. Although 6 clusters achieved the best score of 0.240, the improvement over 5 clusters (0.229) was marginal. 

From a business perspective, **5 clusters were selected** as the optimal solution, as this balance avoids over-segmentation while ensuring sufficient differentiation to support actionable marketing insights.

## 9. Recommendation and Analysis

The K-Means model was developed with 5 clusters, and the clusters were mapped back to the original dataset for customer profiling.
The distribution of customers across clusters is as follows:

<img width="150" height="300" alt="image" src="https://github.com/user-attachments/assets/4be37fbb-ca7c-4701-808c-fe1112817054" />

###### _Fig 22 Distribution of 5 Clusters_

This distribution shows that clusters vary considerably in size, with Cluster 1 and Cluster 3 representing the largest customer segments, while Cluster 4 represents a much smaller, niche group. Such imbalances are common in customer segmentation and often indicate the presence of both mass-market customer bases and smaller, specialized personas.

#### **9.1.	Customer Profiling**

The characteristic of each clusters are summarized as follows:

<img width="944" height="947" alt="image" src="https://github.com/user-attachments/assets/290f0fe5-1cd0-48b3-9d57-7152c8c02e67" />

###### _Fig 23 Characteristic Distribution of 5 Clusters_

##### **9.1.1	Cluster 0 - Luxury Mini buyers**

Customers in Cluster 0 are characterized by high spending per product, yet they tend to purchase smaller-sized items, such as travel-size or luxury minis. This suggests that these customers are drawn to premium small-format products, possibly for trial purposes or as collectible luxury minis. 

From a marketing perspective, this cluster can be targeted with mini sets, luxury trial kits, and gift sets, emphasizing exclusivity and premium quality. Personalized recommendations that highlight limited-edition or collectible items are likely to resonate with this group and encourage engagement and repeat purchases.

##### **9.1.2.	Cluster 1 - Bargain hunters / Value-Driven Reviewers**

Customers in Cluster 1 are value-driven shoppers who tend to purchase larger-sized products at lower unit prices. They also exhibit the highest helpfulness scores, indicating that they are more critical and detailed reviewers. This segment often prefers limited-edition products but seeks good value, suggesting a focus on mainstream items rather than premium luxury products. 

From a marketing perspective, these customers can be targeted with discounts, value sets, loyalty rewards, and flash sales, which appeal to their price-conscious behaviour while encouraging engagement and repeat purchases.

##### **9.1.3.	Cluster 2 - Highly engaged reviewers / social influencer**

Customers in Cluster 2 are highly engaged shoppers who exhibit the highest loves count along with high helpfulness scores, indicating that they are both passionate about products and influential through their reviews. They typically purchase mid-sized products at lower unit prices, but also engage with mid- to high-range items, reflecting a willingness to invest in quality or premium products. This cluster shows a strong preference for Sephora-exclusive products, suggesting that these customers are drawn to unique and limited offerings.

Marketing strategies for this segment could include early access programs, exclusive product drops, and insider clubs, leveraging their engagement to drive buzz, brand advocacy, and repeat purchases.

##### **9.1.4.	Cluster 3 – Online Shopper**

Customers in Cluster 3 are primarily online-only shoppers who show high engagement with online purchases. They tend to give high product ratings but have low helpfulness scores, indicating that while they enjoy the products, they are less likely to write detailed or influential reviews. This segment typically purchases mid-sized products at mid-to-low price points, reflecting mainstream purchasing behavior.

Marketing strategies for this group should focus on digital campaigns, online-only product launches, and app-exclusive offers, catering to their preference for convenient online shopping while encouraging repeat purchases through targeted digital promotions.

##### **9.1.5.	Cluster 4 – Trendsetters / New-Product Enthusiasts**

Customers in Cluster 4 are trend-driven shoppers who show a strong preference for new and limited-edition products. They typically give high ratings and have moderate helpfulness scores, indicating they enjoy the products but provide only some review feedback. This segment tends to purchase larger-sized products at lower prices, reflecting a focus on value for money while staying ahead of trends. 

Marketing strategies for this cluster should emphasize early-launch campaigns, influencer collaborations, and TikTok-driven promotions, leveraging their enthusiasm for new releases and exclusive products to drive engagement and brand visibility.

## **10. Conclusion**

This analysis aimed to identify distinct customer segments to enable more targeted and effective marketing strategies for Sephora. While challenges such as weak clustering metrics (Elbow and Silhouette methods) suggest that customer preferences may overlap, the study successfully produced meaningful and actionable segmentation.

Using K-Means clustering on aggregated, customer-centric features, five customer segments were identified:

Cluster    | Key Characteristic | Marketing Recommendations           
--------------------- | --------------------- | --------------------- 
**0: Luxury Mini Buyers** | High spending per product; purchase small/travel-size luxury items | Highlight mini sets, luxury trial kits, gift sets; emphasize exclusivity and premium quality
**1: Bargain Hunters / Value-Driven Reviewers** | Large-size products at low unit price; highly helpful reviewers; prefer limited-edition items | Offer discounts, value sets, loyalty rewards, flash sales; appeal to price-conscious shoppers
**2: Highly Engaged Reviewers / Social Influencers** | High loves count and helpfulness; mid-size products; prefer Sephora exclusives; engage with mid-to-high range items | Implement early access programs, exclusive drops, insider clubs; leverage engagement for advocacy
**3: Online Shoppers** | Online-only buyers; high ratings but low helpfulness; mid-size, mid-low price products | Focus on digital campaigns, online-only launches, app-exclusive offers; encourage repeat online purchases
**4: Trendsetters / New-Product Enthusiasts** | Prefer new and limited-edition products; high ratings; mid helpfulness; large size, low price | Launch early-release campaigns, influencer collaborations, TikTok-driven promotions; capitalize on trend-driven behavior

## **11. AI Ethics**

The following potential data science ethical issues were identified in this analysis:

**1. Privacy**

The analysis uses customer reviews and product engagement data. Even though the dataset is anonymized, there is a responsibility to ensure that individual customer identities are not exposed.

Any future integration with demographic or transactional data must comply with data privacy laws (e.g., PDPA and GDPR) to prevent misuse of personal information.

**2. Fairness**

The dataset primarily represents Asian women customers and skincare products, which could introduce bias in segmentation results.
Marketing strategies based on these clusters may inadvertently Favor certain groups over others, potentially leading to unintended discrimination or exclusion of other demographics.

**3. Accuracy**

The segmentation relies on clustering models (K-Means) that assume equal variance and spherical clusters, which may not perfectly capture all customer behaviors.

Low silhouette scores indicate cluster overlap, which could lead to inaccurate targeting if the insights are used without caution.

**4. Accountability**

Analysts and decision-makers must take responsibility for how the cluster insights are applied in marketing. Misuse of segments (e.g., targeting based on biased assumptions) could harm customers or the brand.

**5. Transparency**

The clustering process and feature selection should be clearly documented so that stakeholders understand how decisions are derived from the data.
Transparency is important to maintain trust with customers and internal teams, especially when data-driven insights guide marketing strategies.

**6. Summary**

While this analysis provides actionable insights for Sephora, careful attention to privacy, fairness, accuracy, accountability, and transparency is essential to ensure ethical use of customer data. Future work should include ethical review procedures, bias mitigation, and clear documentation before deploying any targeted marketing strategies.

## Source Codes and Datasets

ITD214_Sephora_Data_Preparation.ipynb

ITD214_Sephora_Clustering_v4.ipynb

