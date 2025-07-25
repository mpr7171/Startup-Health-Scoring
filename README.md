# Startup Health Scoring

A comprehensive scoring system for evaluating early-stage startups, functioning similarly to a credit score but specifically designed for startup potential assessment.

## Task Overview

This task implements a Startup Evaluation Engine that generates composite scores (0-100) for 100 fictional startups based on six key business indicators. The engine enables systematic ranking and analysis of startup potential through data-driven methodologies and machine learning validation.

## Project Structure

```
startup-evaluation-engine/
├── notebooks/
│   └── Startup-Health-Scoring.ipynb             
├── outputs/
│   ├── Feature Correlation Heatmap.png
│   ├── Feature Weight Distribution.png
│   ├── Final Score Vs Monthly Active Users.png
│   ├── KMeans Cluster sizes and Average Score by Cluster.png
│   ├── ML Models Feature Importance.png
│   ├── Performance Tier Distribution.png
│   ├── Score Distribution Histogram.png
│   └── Top 20 Startup Sorted Scores.png
├── datasets/
│   └── Startup_Scoring_Dataset.csv              
└── README.md                                    
```

## Dataset Description

The dataset contains 100 fictional startups with the following features:

- **startup_id**: Unique identifier
- **team_experience**: Average years of relevant experience (1-10 scale)
- **market_size_million_usd**: Total addressable market in million USD
- **monthly_active_users**: Current monthly active user count
- **monthly_burn_rate_inr**: Average monthly expenses in INR
- **funds_raised_inr**: Total funding raised in INR
- **valuation_inr**: Current company valuation in INR

## Methodology

### 1. Data Preprocessing

**Normalization Strategy:**
- Applied Min-Max scaling to normalize all features to 0-1 range
- Special handling for burn rate by inverting the normalized values since lower burn rates indicate better financial health
- Revised Formula for burn rate: `normalized_burn_rate = 1 - ((original_value - min_value) / (max_value - min_value))


### 2. Feature Weighting & Scoring Formula

**Weight Assignment Rationale:**

| Feature | Weight | Justification |
|---------|--------|---------------|
| Monthly Active Users | 25% | Direct indicator of product-market fit and traction |
| Market Size | 20% | Represents growth potential and scalability |
| Team Experience | 20% | Critical for execution capability and decision-making |
| Burn Rate | 15% | Financial sustainability and operational efficiency |
| Funds Raised | 10% | Market validation and runway extension |
| Valuation | 10% | Market perception and investor confidence |


**Scoring Formula:**

```
Composite Score = (
    0.25 × users_normalized + 
    0.20 × market_normalized + 
    0.20 × team_normalized + 
    0.15 × burn_rate_normalized + 
    0.10 × funds_normalized + 
    0.10 × valuation_normalized
) × 100
```

### 3. Machine Learning Validation

Implemented four ML models to validate the scoring methodology:

- **Ridge Regression**: Baseline linear model
- **Random Forest**: Ensemble method for feature importance analysis
- **Gradient Boosting**: Advanced ensemble technique
- **XGBoost**: Optimized gradient boosting

**Model Performance:**

| Model | CV R² | Test R² | RMSE | MAE |
|-------|--------|---------|------|-----|
| Ridge Regression | 0.9545 | 0.9760 | 2.15 | 1.69 |
| Random Forest | 0.6068 | 0.7680 | 6.69 | 5.73 |
| Gradient Boosting | 0.6809 | 0.8811 | 4.79 | 4.00 |
| XGBoost | 0.5413 | 0.7102 | 7.48 | 5.22 |

- Ridge Regression achieved the highest predictive accuracy (Test R² = 0.9760)
- Feature importance analysis confirmed the relevance of chosen weights
- All models demonstrated good predictive capability with varying performance levels

### 4. Key Design Decisions

**Why These Weights?**

- **User Traction (25%)**: Highest weight because active users directly indicate product-market fit - the most critical factor for startup success
- **Market Size (20%)**: Large markets provide greater growth potential and scalability opportunities
- **Team Experience (20%)**: Experienced teams are more likely to navigate challenges and execute effectively
- **Burn Rate (15%)**: Financial discipline is crucial but secondary to growth metrics in early stages
- **Funding Metrics (20% combined)**: Important for runway and validation but not the primary success drivers

**Handling Negatively Correlated Metrics:**
- **Burn Rate Inversion**: Since higher burn rates are detrimental, we inverted the normalized values
- **Mathematical Approach**: `inverted_score = 1 - normalized_score`
- **Business Logic**: A startup with the lowest burn rate receives the highest score (1.0), while the highest burn rate receives the lowest score (0.0)

## Key Insights & Surprises

### Top Performers Characteristics:
- High user traction combined with moderate burn rates
- Experienced teams in large markets
- Balanced across multiple metrics rather than excelling in just one area

### Bottom Performers Patterns:
- Low user engagement despite adequate funding
- High burn rates without corresponding growth
- Inexperienced teams in competitive markets

### Surprising Findings:
- **Funding ≠ Success**: Several well-funded startups scored poorly due to high burn rates and low user traction
- **Experience Premium**: Teams with 8+ years of experience consistently outperformed regardless of other factors
- **Market Size Sweet Spot**: Mid-large markets (400-800M USD) showed better performance than extremely large markets, possibly due to less competition
- **Burn Rate Efficiency**: Startups with burn rates below 3M INR/month had significantly higher composite scores

### Model Validation Results:
- Feature importance analysis confirmed that user metrics and team experience were the strongest predictors
- Ridge Regression outperformed ensemble methods with Test R² of 0.9760
- Clustering analysis revealed optimal segmentation at K=3 clusters (Silhouette Score: 0.175)

### Cluster Analysis Results:

**Optimal Clustering:** K=3 clusters identified through silhouette analysis

| Cluster | Name | Size | Avg Score | Characteristics |
|---------|------|------|-----------|----------------|
| Cluster 0 | Development-Stage Startups | 27 (27.0%) | 49.72 | Early-stage with development focus |
| Cluster 1 | Growth-Ready Startups | 38 (38.0%) | 50.97 | Ready for scaling operations |
| Cluster 2 | Growth-Ready Startups | 35 (35.0%) | 51.01 | Established growth trajectory |

**Silhouette Score Analysis:**
- K=2: 0.157
- **K=3: 0.175 (Optimal)**
- K=4: 0.153
- K=5: 0.157
- K=6: 0.145
- K=7: 0.156

## Limitations & Future Improvements

### Current Limitations:
- **Static snapshot**: No time-series data to track growth trends
- **Synthetic data**: Results demonstrate methodology rather than real-world predictions
- **Equal weight assumption**: Some features might need dynamic weighting based on startup stage

### Suggested Enhancements:
- **Growth rate metrics**: Incorporating month-over-month user and revenue growth
- **Industry-specific weights**: Adjusting scoring based on startup vertical
- **Dynamic scoring**: Implementing time-based weight adjustments for different startup stages
- **External factors**: Including market conditions and competitive landscape
- **Qualitative metrics**: Adding founder background, product uniqueness, and strategic partnerships