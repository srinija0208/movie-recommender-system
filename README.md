# Movie Recommendation System

## Problem Statement
We need to build a **movie recommendation website** where a user enters a movie name, and we provide **5 similar movies** based on **content similarity**.

Since we have around **5000 movies**, we need a way to determine which ones are most similar. The **similarity is based on movie tags**, which include:
- **Overview** (Movie description)
- **Genre** (Action, Comedy, etc.)
- **Keywords** (Tags related to the movie)
- **Cast** (Top 3 actors)
- **Crew** (Director)

To measure similarity, we use **text-vectorization** techniques to convert text data into numerical vectors and then find the closest movies to a given input.

---

## Steps to Build the Recommendation System

### **Step 1: Data Cleaning & Preprocessing**
We begin by keeping only the most relevant columns and removing unnecessary ones.

#### **Columns to Keep:**
✅ `movie_id` (Used for fetching movie posters)
✅ `title` (Movie name)
✅ `overview` (Summary of the movie)
✅ `genre`, `keywords` (Tags for better recommendations)
✅ `cast`, `crew` (Limited to top 3 actors and director)

#### **Columns to Remove:**
❌ `budget`, `homepage`, `original_language`, `original_title`  
❌ `popularity`, `production_company`, `release_date`, `runtime`, `revenue`  
❌ `spoken_languages`, `tagline`, `status`, `vote_average`, `vote_count`, `id`

📌 **Why Remove These?**
- Numerical columns don't work well in our **content-based filtering** approach.
- Features like `budget` and `revenue` don't directly impact a movie's similarity.
- `original_language` is highly imbalanced (most movies are in English).
- `production_company` is not a common factor in movie recommendations.

---

### **Step 2: Creating the Tags Feature**
To create meaningful **tags**, we merge:
- `overview`
- `genre`
- `keywords`
- `cast` (Top 3 actors)
- `crew` (Only the director)

We then:
- Convert all text to **lowercase**.
- Remove spaces between names (e.g., "Sam Worthington" → "SamWorthington") to prevent confusion with other actors/directors.
- Handle missing values by replacing `NaN` with an empty string.

---

### **Step 3: Text Vectorization (Bag of Words)**
Since our data is textual, we convert text into numerical vectors using **Bag of Words (BoW)**:
1. Merge all tags into a single large text corpus.
2. Extract the **5000 most common words**.
3. Create a **word frequency matrix** where each movie is represented as a vector of word counts.

#### **Example (Simplified Vectorization for Two Words)**
| Movie | Action | Drama |
|-------|--------|--------|
| M1    | 3      | 4      |
| M2    | 4      | 5      |
| M3    | 2      | 6      |
| M5000 | 6      | 7      |

Each row (movie) becomes a **vector**, and we can now compare them numerically.

---

### **Step 4: Stemming (Text Processing)**
We perform **stemming** to reduce words to their root forms:
- "loved", "loving", "love" → **"love"**

This ensures that similar words are treated the same.

---

### **Step 5: Computing Movie Similarity**
To find **similar movies**, we compute the similarity between each movie’s vector:
- Instead of using **Euclidean Distance**, we use **Cosine Similarity** (measures the angle between vectors).
- If two movies have **smaller angles**, they are more similar.
- We calculate the similarity for every movie against all others (**4806 × 4806 matrix**).

🔹 **Similarity Formula:**
\[ \text{similarity} = \cos(\theta) = \frac{A \cdot B}{||A|| ||B||} \]

---

### **Step 6: Finding the Top 5 Similar Movies**
- When a user enters a movie, we find its index.
- Sort the similarity scores in descending order.
- Select the **top 5 most similar movies**.
- Use `enumerate()` to retain original indexes while sorting.

---

### **Step 7: Storing and Using Data in the Website**
- The movie data and computed similarity matrix are stored in a **PKL (pickle) file** for faster access.
- Instead of sending data as a dataframe, we store it as a **dictionary** for easy retrieval.
- The recommendation system is then integrated into a **Flask/Django web application** where users input a movie and get recommendations instantly.

---

## **Project Output**
✅ A functional movie recommendation system that suggests similar movies based on textual similarity.
✅ Uses **content-based filtering** (text similarity, vectorization, and cosine similarity).
✅ Built with **Python (Pandas, Numpy, Scikit-learn, Flask/Django, Pickle)**.
✅ Can be deployed as a **web application**.

---

## **Future Improvements**
- Add **collaborative filtering** (user-based recommendations).
- Improve tag weighting (e.g., give more weight to actors/directors).
- Use **word embeddings (TF-IDF, Word2Vec, BERT)** for better similarity.
- Integrate **movie posters and trailers** for better UX.

---

## **How to Run the Project**
1. Install dependencies:
   ```bash
   pip install pandas numpy scikit-learn flask pickle
   ```
2. Run the web application:
   ```bash
   python app.py
   ```
3. Open **localhost:5000** in your browser and enter a movie name to get recommendations!

---

## **Conclusion**
This project demonstrates how to build a **content-based movie recommendation system** using **natural language processing (NLP) and machine learning**. By converting movie descriptions into vectors and computing similarity scores, we can efficiently suggest movies that share common features. 🚀

