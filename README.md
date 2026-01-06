# Multimodal Property Price Prediction

## Overview
This project builds a multimodal regression pipeline to predict residential property prices by combining structured tabular data with satellite imagery. The objective is to quantify how much neighborhood-level visual context improves price prediction beyond traditional housing attributes.

## Problem Statement
Traditional property valuation models rely heavily on structured features such as size, age, and location coordinates. However, these features often fail to capture environmental and neighborhood-level characteristics. This project explores whether incorporating satellite imagery can improve predictive performance by encoding visual cues such as urban density, greenery, and surrounding infrastructure.

## Dataset
- **Tabular Data:** Structured housing attributes including location, size, construction quality, and temporal features.
- - **Satellite Imagery:** NAIP satellite images acquired using geographic coordinates for each property.
## Methodology (High-Level)
- **Tabular Pipeline:** Feature cleaning, engineering, and log transformation of the target variable.
- **Image Feature Extraction:** CNN-based embeddings extracted from satellite images.
- **Fusion Strategy:**
  - Tabular-only baseline
  - Image-only baseline
  - Early Fusion (feature concatenation)
  - Intermediate / Late Fusion (model-level fusion)
- **Model:** Gradient Boosting Regressor used consistently across tabular, image, and fusion experiments.
## Results
| Model               | RMSE (price_log)      | R² (price_log)        |
|---------------------|------------------------|-----------------------|
| Tabular Only        | 0.181751               | 0.883536              |
| Image Only          | 0.532884               | -0.001164             |
| Early Fusion        | 0.181613               | 0.883712              |
| Intermediate Fusion | 0.206117               | 0.850215              |
## Key Insights
- Geographic location is the strongest driver of property prices.
- Property size and construction quality play a secondary but significant role.
- Neighborhood-level context captured via satellite imagery improves predictive performance when combined with tabular data.
- Multimodal fusion models outperform tabular-only baselines, validating the value of visual features.
## Project Structure
```
satellite-imagery-property-valuation/
│
├── data/
│   ├── processed(train).csv
│   └── processed(test).csv
│
├── notebooks/
│   ├── 01_preprocessing.ipynb
│   ├── 02_data_fetcher.ipynb
│   ├── 03_image_embeddings.ipynb
│   └── 04_model_training.ipynb
│
├── extrafiles/
│   └── train_image_embeddings.npy
|   └── train_image_ids.npy
│   └── test_image_embeddings.npy
|   └── test_image_ids.npy
│
├── 24113050_final.csv
├── 24113050_report.pdf
└── README.md
```

