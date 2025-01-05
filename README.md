## **README for PS1: Semantic Grouping of Clinical Studies**

---

### **1. Problem Statement**
Design a mechanism to retrieve **relevant clinical trials** based on query inputs (e.g., `Study Title`, `Primary Outcome Measure`, `Secondary Outcome Measure`, `Criteria`) by leveraging AI/ML techniques. The objective is to assist researchers in finding similar trials for faster and more accurate trial design, reducing manual effort and improving efficiency.

---

### **2. Objective**
- Build a **similarity model** capable of retrieving **10 unique similar trials** for any given query trial.
- Ensure the solution:
  - Handles unstructured textual data efficiently.
  - Incorporates domain-relevant features like `Conditions`, `Phases`, and `Criteria`.
  - Aligns with clinical trial design logic and domain expert requirements.
  - Provides explainability and visualizations for stakeholder trust.

---

### **3. Key Features of the Solution**
1. **Data Preprocessing:**
   - Merged 5 datasets using `NCT Number` as the primary key.
   - Handled missing values, duplicates, and inconsistent formats.
   - Preprocessed text fields by lowercasing, removing stopwords, and stemming for uniformity.
   - Normalized categorical fields like `Phases`, `Age_group`, and `Sex`.

2. **Feature Engineering:**
   - Created a `combined_text` field by concatenating key textual fields (`Study Title`, `Conditions`, `Criteria`, etc.) to serve as input for similarity computation.
   - Added `Age_group` and `Sex` fields to explore their impact on similarity scores.

3. **Model Building:**
   - **TF-IDF Vectorization**: Converted `combined_text` into numerical vectors emphasizing unique terms.
   - **Cosine Similarity**: Computed similarity between trials based on their TF-IDF vectors.

4. **Evaluation Metrics:**
   - **Cosine Similarity Scores**: Used to evaluate the relevance of retrieved trials.
   - Time Efficiency: Measured the time taken to compute similarities for a large dataset.

5. **Model Explainability:**
   - Used **SHAP** to explain feature importance for structured data like `Phases` and `Age_group`.
   - Visualized text attributions using TF-IDF weights for unstructured data.

6. **Visualization:**
   - **Heatmap** to visualize pairwise similarity scores.
   - Evaluation table summarizing top 10 similar trials and average similarity scores.

---

### **4. Dataset**
- **Source**: A subset of publicly available clinical trials from clinicaltrials.gov.
- [Raw and Processed CSV files](https://drive.google.com/drive/folders/1JqDY1EM_VgbThqMtaDUGCzkVQbvxrP2b?usp=sharing)
- **Size**: ~450,000 trials.
- **Key Fields**:
  - `Study Title`
  - `Primary Outcome Measures`
  - `Secondary Outcome Measures`
  - `Criteria`
  - `Conditions`
  - `Phases`
  - `Age_group`
  - `Sex`

---

### **5. Methodology**
1. **Text Preprocessing:**
   - Applied stopword removal, stemming, and lowercasing.
   - Removed special characters and numbers for clean textual input.

2. **TF-IDF Vectorization:**
   - Experimented with parameters like `ngram_range`, `max_features`, `min_df`, and `max_df`.
   - Two versions were created:
     - **With demographic fields (`Age_group` and `Sex`).**
     - **Without demographic fields.**

3. **Similarity Computation:**
   - Calculated cosine similarity scores for trials.
   - Retrieved top 10 similar trials for a given query.

4. **Model Explainability:**
   - Used SHAP to explain feature importance (e.g., `Conditions` contributed 50%, `Criteria` 30%).
   - Visualized high-impact terms in text fields for better understanding.

---

### **6. Results**
- **Evaluation Metrics:**
  - Average similarity score for queries: **0.643**.
  - Time taken for similarity computation: **3.82 seconds** for ~450,000 trials.

- **Example Output:**
  - Query Trial: **NCT00385736**
  - Top Similar Trial: **NCT03844932** (Similarity Score: 0.773)

- **Visualization:**
  - Heatmap showing pairwise similarity for selected trials.
  - SHAP summary and force plots explaining feature contributions.

---

### **7. Tools and Frameworks**
- **Preprocessing and Modeling:**
  - `pandas`, `numpy`, `scikit-learn`
  - `TF-IDF` and `Cosine Similarity`
- **Explainability:**
  - `SHAP` for feature importance.
- **Visualization:**
  - `matplotlib`, `seaborn`

---

### **8. How to Run the Solution**
1. **Install Dependencies:**
   ```
   pip install pandas numpy scikit-learn shap matplotlib seaborn
   ```

2. **Load Preprocessed Dataset:**
   - File: `preprocessed_clinical_trials.csv`

3. **Run Similarity Model:**
   - Load `tfidf_matrix_with_demo.npz` and `tfidf_vectorizer_with_demo.pkl`.
   - Use `find_similar_trials_with_demo()` to retrieve similar trials for a given `NCT Number`.

4. **Evaluate Results:**
   - Use the output to validate the retrieved trials.
   - View SHAP plots for explainability.

---

### **9. Caveats and Future Improvements**
- **Caveats:**
  - Limited context understanding: TF-IDF misses deep semantic relationships.
  - Dataset biases: Underrepresented conditions may affect similarity accuracy.

- **Future Enhancements:**
  - Use embedding models like **BERT** or **BioBERT** for better semantic understanding.
  - Optimize similarity computation with **Approximate Nearest Neighbors (ANN)** for scalability.

---

### **10. Conclusion**
This solution provides an efficient, explainable, and scalable framework for retrieving similar clinical trials based on unstructured text. By integrating domain-specific explainability and scalable modeling techniques, it aligns with clinical trial design needs while ensuring trust and interpretability.
