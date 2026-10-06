# 🍳 Custom NER Model — Identifying Key Entities in Recipe Data

> A CRF-based Named Entity Recognition (NER) pipeline for transforming unstructured recipe text into structured ingredients, quantities, and measurement units.

## 🚀 Project Highlights

This project builds a custom sequence-labeling NER system specifically for recipe data.

| Entity | Purpose |
|---|---|
| 🥕 Ingredient | Identifies ingredient-related tokens |
| 🔢 Quantity | Identifies quantity expressions |
| 📏 Unit | Identifies measurement units |

### 📊 Key Results

| Metric | Result |
|---|---:|
| Original recipes | **285** |
| Invalid records removed | **5** |
| Valid recipes | **280** |
| Training / Validation | **196 / 84** |
| Training labelled tokens | **7,114** |
| Validation labelled tokens | **2,876** |
| **Validation token accuracy** | **98.16%** |
| **Weighted validation F1** | **0.98** |
| Validation misclassified tokens | **53** |

## 🎯 Problem Statement

Recipe text is often semi-structured or unstructured. This project aims to automatically identify three important entities from recipe text: ingredients, quantities, and measurement units.

Example:

```text
2 teaspoons salt
```

- 2 → Quantity
- teaspoons → Unit
- salt → Ingredient

## 🧠 End-to-End Approach

```text
Raw Recipe Data
       ↓
Data Validation & Cleaning
       ↓
70:30 Train / Validation Split
       ↓
Exploratory Data Analysis
       ↓
Feature Engineering
       ↓
Class Imbalance Handling
       ↓
Weighted CRF Training
       ↓
Validation & Error Analysis
       ↓
Structured Recipe Entities
```

## 🧹 1. Data Preparation

- Dataset shape: **285 × 2**
- Fields: input and pos
- Unique labels: ingredient, quantity, unit
- Rows with mismatched token/label lengths: **5**
- Removed rows: **17, 27, 79, 164, 207**
- Final valid recipes: **280**
- Train/validation split: **70:30**
- Training recipes: **196**
- Validation recipes: **84**

The five invalid records were removed because their input-token and label-token counts did not match.

## 🔎 2. Exploratory Data Analysis

The training data contains **7,114 labelled tokens**.

| Entity | Training Support |
|---|---:|
| Ingredient | 5,323 |
| Quantity | 980 |
| Unit | 811 |

Ingredient tokens dominate the dataset, motivating inverse-frequency class weighting.

### Top Training Ingredients

powder, salt, leaves, oil, seeds, red, green, chilli, coriander, chopped

### Top Training Units

teaspoon, cup, tablespoon, grams, tablespoons, inch, cups, sprig, cloves, teaspoons

## ⚙️ 3. Feature Engineering

The CRF uses a combination of lexical, linguistic, quantity/unit, and contextual features:

- Token lemma
- Part-of-speech information
- Token shape
- Quantity-pattern indicators
- Unit keyword indicators
- Quantity keyword indicators
- Neighboring-token context
- Regular-expression-based quantity patterns

## ⚖️ 4. Class Imbalance Handling

Inverse-frequency class weights were applied:

| Label | Training Support | Class Weight |
|---|---:|---:|
| Ingredient | 5,323 | **0.556860** |
| Quantity | 980 | **2.419728** |
| Unit | 811 | **2.923962** |

The unit class receives the highest weight because it has the smallest training support.

## 🤖 5. CRF Model

| Hyperparameter | Value |
|---|---|
| Algorithm | lbfgs |
| c1 | 0.5 |
| c2 | 1.0 |
| Maximum iterations | 100 |
| All possible transitions | True |

## 📈 6. Validation Performance

| Entity | Precision | Recall | F1-score | Label Accuracy |
|---|---:|---:|---:|---:|
| 🥕 Ingredient | 0.98 | 0.99 | 0.99 | **99.15%** |
| 🔢 Quantity | 0.99 | 0.99 | 0.99 | **98.78%** |
| 📏 Unit | 0.95 | 0.92 | 0.93 | **91.62%** |

### Overall

**98.16% validation token accuracy** · **0.98 weighted F1-score** · **53 misclassified tokens / 2,876**

## 🔥 7. Confusion Matrix Insights

The validation confusion matrix shows:

- **406 / 411** quantity tokens correctly identified
- **328 / 358** unit tokens correctly identified
- **2,089 / 2,107** ingredient tokens correctly identified

The largest concentration of errors occurs when **unit tokens are predicted as ingredients**.

## 🧪 8. Error Analysis

| Label | Support | Correct | Accuracy | Errors |
|---|---:|---:|---:|---:|
| Ingredient | 2,107 | 2,089 | **99.15%** | 18 |
| Quantity | 411 | 406 | **98.78%** | 5 |
| Unit | 358 | 328 | **91.62%** | 30 |

Representative difficult tokens include cloves, pieces, inch, sprig, slices, spoon, and descriptive/cooking words.

These results show that **context is critical in recipe NER**.

## 💡 Key Insights

1. **Strong generalization:** 98.16% validation token accuracy and 0.98 weighted F1.
2. **Ingredient recognition is highly reliable:** 99.15% label accuracy.
3. **Quantity recognition is strong:** 98.78% label accuracy and 0.99 F1.
4. **Unit recognition is the main challenge:** 91.62% label accuracy and 0.93 F1.
5. **Class weighting addresses imbalance:** minority classes receive larger weights.
6. **Context matters:** ambiguous recipe terms depend heavily on neighboring tokens.

## 🔮 Future Improvements

- Strengthen contextual features for unit recognition
- Add more comprehensive unit normalization
- Increase training examples for ambiguous measurement terms
- Improve handling of context-dependent words
- Compare additional sequence-labeling approaches

## 🛠️ Technology Stack

- Python
- pandas
- NumPy
- spaCy
- scikit-learn
- sklearn-crfsuite
- Conditional Random Fields (CRF)
- Regular Expressions
- Matplotlib

## 📁 Repository Structure

```text
custom-ner-recipe-data/
│
├── Identifying_Key_Entities_in_Recipe_Data_Ishu_Dhakad.ipynb
├── Identifying_Key_Entities_in_Recipe_Data_Ishu_Dhakad_Report.pdf
├── ingredient_and_quantity.json
├── README.md
└── requirements.txt
```

### Files

| File | Description |
|---|---|
| Notebook | Complete executed notebook with data preparation, EDA, feature engineering, CRF training, evaluation, and error analysis |
| PDF Report | Detailed report with methodology, charts, metrics, confusion matrices, insights, and limitations |
| JSON Dataset | Recipe dataset used for the project |
| requirements.txt | Python dependencies |

## ⚠️ Assumptions & Limitations

- Evaluation is **token-level**, rather than recipe-level exact-match evaluation.
- The report uses the **executed notebook outputs as the source of truth**.
- The dataset contains noisy/descriptive recipe text, creating genuine ambiguity.
- Results are specific to the supplied dataset, split, feature engineering, and CRF configuration.

## 🏁 Conclusion

This project demonstrates a complete domain-specific **NER pipeline using Conditional Random Fields** to convert recipe text into structured ingredient, quantity, and unit entities.

With **98.16% validation token accuracy** and **0.98 weighted F1**, the model provides a strong baseline for structured recipe information extraction.

The analysis also identifies where the model fails and why. The clearest opportunity is improving unit recognition through stronger contextual features, better unit normalization, and additional examples of ambiguous measurement terms.

## 👨‍💻 Author

**Ishu Dhakad**  
B.Tech — Computer Science & Engineering

⭐ Explore the notebook and detailed PDF report to see the complete implementation and evaluation.