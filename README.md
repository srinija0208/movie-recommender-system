# Movie Recommender System

## Problem Statement
We need to build a **movie recommendation system** that suggests **5 similar movies** based on content similarity.

## Steps Involved

### 1. Data Preprocessing
- **Selected Features:** `movie_id`, `title`, `overview`, `genres`, `keywords`, `cast`, `crew`.
- **Dropped Features:** Unnecessary numerical and categorical columns like `budget`, `popularity`, `release_date`, etc.

### 2. Creating Tags
- Merged `overview`, `genres`, `keywords`, `top 3 actors`, and `director` into a **single text column (tags)**.
- Removed spaces in names (`Sam Worthington` → `SamWorthington`).

### 3. Text Vectorization
- Converted movie tags into numerical vectors using **Bag of Words (BoW)**.
- **5000 most common words** were selected.

### 4. Calculating Similarity
- Used **cosine similarity** (instead of Euclidean distance) to measure movie similarity.
- Found **closest vectors** to recommend movies.

### 5. Building the Recommender
- Retrieved the **index** of the user’s selected movie and returned the top **5 most similar movies**.

### 6. Deployment
- Stored processed data in a `.pkl` file for faster loading.
- Built a **web application** for user input and recommendations.

## How to Run the Project
1. Clone the repository:
   ```bash
   git clone https://github.com/srinija0208/movie-recommender-system.git
   cd movie-recommender-system
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Run the application:
   ```bash
   python app.py
   ```
4. Open the **web interface** in your browser and enter a movie name to get recommendations.

## Technologies Used
- Python
- Pandas, NumPy
- Scikit-learn (for vectorization & similarity calculation)
- Flask (for web app deployment)
- Pickle (for model storage)

---

### Notes
- **Large files issue:** If `similarity.pkl` exceeds **100MB**, use `Git LFS` or exclude it from Git tracking.
- **To remove large files from Git history:**
  ```bash
  git filter-repo --path similarity.pkl --invert-paths --force
  ```
  Then force push:
  ```bash
  git push origin master --force
  ```

