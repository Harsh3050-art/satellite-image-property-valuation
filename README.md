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
│   ├── train_image_embeddings.npy
│   ├── train_image_ids.npy
│   ├── test_image_embeddings.npy
│   └── test_image_ids.npy
│
├── outputs/
│   └── 24113050_final.csv
│
├── reports/
│   └── 24113050_report.pdf
│
└── README.md

```
## How to Run the Project

The project follows a sequential, notebook-based workflow. Each step depends on the outputs of the previous stage, so it is important to run the notebooks in the order listed below.

---

### Step 1: Tabular Data Preprocessing & EDA
Run the preprocessing notebook first.

This notebook performs the following tasks:
- Loads the raw tabular housing dataset
- Cleans missing and inconsistent values
- Performs exploratory data analysis (EDA)
- Conducts geospatial analysis using latitude and longitude
- Engineers relevant tabular features
- Applies log transformation to the target variable (`price_log`)
- Prepares the dataset for image alignment and modeling

This step establishes a clean and well-understood tabular foundation for the multimodal pipeline.

---

### Step 2: Download Satellite Images
After preprocessing, download satellite imagery using geographic coordinates.


This notebook:
- Loads the cleaned tabular housing dataset
- Uses property latitude–longitude coordinates
- Downloads satellite imagery in **GeoTIFF format** using a remote imagery API
- Ensures consistent image resolution and coverage
- Aligns each downloaded satellite image with its corresponding property ID

The output of this step is a structured collection of satellite images mapped directly to housing records.

---

### Step 3: Extract Image Embeddings
Convert satellite images into numerical feature representations.


This notebook:
- Loads the downloaded GeoTIFF satellite images
- Uses a CNN-based feature extractor
- Converts each image into a fixed-length embedding vector
- Separately processes training and test images
- Saves the following files:
  - `train_image_embeddings.npy`
  - `train_image_ids.npy`
  - `test_image_embeddings.npy`
  - `test_image_ids.npy`

These embeddings serve as the visual input for multimodal modeling.

---

### Step 4: Model Training and Evaluation
Train baseline and multimodal models and generate final predictions.


This notebook:
- Loads processed tabular features and image embeddings
- Aligns tabular rows with corresponding image embeddings using property IDs
- Trains the following models using **Gradient Boosting Regressor**:
  - Tabular-only model
  - Image-only model
  - Early fusion model
  - Late (intermediate) fusion model
- Evaluates all models using:
  - RMSE (log-price)
  - R² (log-price)
- Trains the final selected model on the full training data
- Generates predictions for the test dataset
- Applies inverse log transformation to obtain real price predictions
- Saves the final output as `submission.csv`

---

### Final Output
The final prediction file is saved at:

> **Important:**  
> Each step in the pipeline depends on artifacts generated in the previous step.  
> Skipping or reordering steps will break the workflow.

---

## Limitations

- Satellite image features provide only modest performance gains due to the dominance of strong tabular predictors.
- The CNN is used strictly as a fixed feature extractor; end-to-end multimodal training is not performed.
- Image resolution and temporal misalignment between imagery and sale dates may limit fine-grained neighborhood representation.


