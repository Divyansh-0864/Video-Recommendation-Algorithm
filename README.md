# Video Recommendation System


## Objective

Designing a recommendation algorithm that suggests videos based on user preferences and engagement patterns. The algorithm will deliver personalized video recommendations by leveraging user interaction data and video metadata obtained via provided APIs.

## Approach

### 1. **Content-Based Filtering**:
   - **Description**: The content-based filtering approach recommends videos similar to a given video based on its content (e.g., title, description, tags). This is done by transforming the video metadata into vectors using the **Count Vectorizer** and calculating the **Cosine Similarity** between videos.
   - **Steps**:
     - Extract relevant features (tags) from the video data.
     - Preprocess the text (e.g., stemming) to normalize the tags.
     - Vectorize the tags using CountVectorizer.
     - Calculate the **Cosine Similarity** between videos based on their vectors.
     - Recommend the top N similar videos.

### 2. **Collaborative Filtering**:
   - **Description**: Collaborative filtering recommends videos based on user interactions (views, upvotes, comments, etc.). This approach uses **Matrix Factorization** techniques, specifically **Singular Value Decomposition (SVD)**, to predict user preferences and recommend unseen videos.
   - **Steps**:
     - Create an interaction matrix based on user activity (views, upvotes, etc.).
     - Apply **SVD** to factorize the matrix and predict ratings for unseen videos.
     - Recommend the top N videos with the highest predicted ratings.
     - Create a **Memory Based Collaborative Filtering Algorithm** using user-item interaction matrix

### 3. **Hybrid Recommendation System**:
   - **Description**: The hybrid recommender combines both content-based and collaborative filtering techniques to provide a more personalized recommendation. It blends the results from both methods by assigning weights to each approach (content and collaborative).
   - **Steps**:
     - Generate recommendations from both the content-based and collaborative filtering systems.
     - Normalize and combine the results using weighted scoring.
     - Recommend the top N videos based on the combined score.

### 4. **Evaluation Metrics**:
   - **Click-Through Rate (CTR)**: Measures the effectiveness of a recommendation by calculating the interaction rate between the recommended content and the user. It is defined as:
     $$\text{CTR} = \frac{\text{(shares + comments + upvotes)}}{\text{(views + exits)}} \times 100$$
   - **Mean Average Precision (MAP)**: Measures the precision of the top-k recommendations for each user. It averages the precision at each rank position where a relevant video is found.
   Using Memory Based Collaborative Filtering as in Model Based Approach the post recommended are not in viewed post which will lead the calculation nearer to zero 

## **Model Architecture**

1. **Content-Based Filtering**
    - Text vectorization: `CountVectorizer`
    - Similarity measure: `Cosine Similarity`
    - Preprocessing: `Stemming`
2. **Collaborative Filtering**
    - Model Based:
        - Algorithm: `SVD (Singular Value Decomposition)`
        - Evaluation Metric: `RMSE (Root Mean Squared Error)`
    - Memory Bsed:
        - User Item Interaction Matrix
        - Item Based Recommendations
3. **Hybrid Recommender**:
    - The hybrid model combines the results of SVD with content-based filtering. The model is designed to suggest posts that users are most likely to engage with based on similar users' behavior and content features.
    
## Challenges and Solutions

 1. **Cold Start Problem**
**Challenge**: New users or new posts with no interaction history pose a challenge for the recommendation system.<br>
**Solution**: The hybrid model helps mitigate the cold start problem. The content-based filtering component can still recommend new posts based on their metadata, even if there’s no interaction data available for a new user.
2. **Redundant Data in API Responses**
**Challenge**: The API returned data in JSON format with many redundant or repeated fields.<br>
**Solution**: Applied filters to remove duplicate fields and performed normalization to ensure a clean and structured dataset.
3. **Inconsistent Data Structure**
**Challenge**: The API sometimes returned inconsistent structures, making it difficult to cleanly parse and merge the data.<br>
**Solution**: Implemented custom logic to flatten nested JSON structures and handled missing values by filling or ignoring them as needed.
4. **Scalability of Collaborative Filtering**
**Challenge**: Collaborative filtering, particularly matrix factorization, can be slow and memory-intensive with large datasets.<br>
**Solution**: Used SVD and Dimensionality Reduction for efficient matrix factorization so that the algorithm is memory efficient and fast.
5. **MAP Calculation**
**Challenge**: Using Model BasedCollaborative filtering, the score was coming out to be zero as the recommended posts were not in user viewed post so there was no relevant hit.<br>
**Solution**: Used Memory Based Approach to calculate the Mean Average Precision.

## Usage

#### 1. **Content-Based Recommendations**
`recommendations = content_based_recommend('Why fit in..?')
print(recommendations)
`
#### 2. **Collaborative Filtering Recommendations**
`recommendations = collaborative_recommendations('kinha', post_df, algo)
print(recommendations)`

#### 3. **Hybrid Recommendations**
`recommendations = hybrid_recommender('kinha', 'do it now', post_df, algo)
print(recommendations)`

#### 4. **Calculate Click-Through Rate (CTR)**
``post_ctr, user_ctr, detailed_ctr = calculate_ctr(user_df, viewed_df, post_df)
print(post_ctr)
print(user_ctr)``

#### 5. **Calculate Mean Average Precision (MAP)**
    - To calculate MAP for the recommendations, use the calculate_map(viewed_df, post_df, recommendations, k) function. For example:

`recommendation = map_util(user_df, post_df, algo, 'do it now', top_n=5)
map_score = calculate_map(viewed_df, post_df, recommendation, k=10)
print(f"MAP Score: {map_score}")`

## Results
#### 1. **Click-Through Rate (CTR)**
    - After calculating the CTR, we will get the following results:
        - CTR by Post: Displays the average CTR for each post, showing how often users interacted with a post after viewing it.
        - CTR by User: Displays the average CTR for each user, indicating the overall interaction rate with recommended posts.
        - Detailed CTR Shows detailed CTR calculations, including view counts, upvotes, comments, and exit counts.

#### 1. **Mean Average Precision (MAP)**
The Mean Average Precision (MAP) metric is used to evaluate the quality of recommendations. It calculates the precision of the top-k recommended posts for each user based on how relevant they are to the user’s historical interactions.

MAP Score: The final MAP score will give an overall measure of the recommendation system's precision. The higher the MAP score, the better the recommendations.
Output for MAP:

**MAP Score: 0.50**

This score indicates the average precision of the recommendations generated by the system, where higher values represent better performance.