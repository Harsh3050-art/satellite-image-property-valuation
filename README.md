# Satellite Imagery-Based Property Valuation

## Overview
This project builds a multimodal regression pipeline to predict property prices using
tabular housing data and satellite imagery. The goal is to quantify the value added by
visual neighborhood context beyond traditional structured features.

## Project Structure
- `data_fetcher.py` – Downloads satellite images using geographic coordinates
- `preprocessing.ipynb` – Data cleaning and feature engineering
- `model_training.ipynb` – Model training and evaluation
- `outputs/submission.csv` – Final test predictions
- `assets/` – Architecture diagram and sample images

## Modeling Approach
- Tabular baseline using Gradient Boosting Regressor
- Image embeddings extracted using CNN
- Early and Intermediate fusion strategies
- Evaluation using RMSE and R²

## Results
Multimodal fusion models outperform tabular-only baselines, demonstrating that satellite
imagery captures valuable neighborhood-level information.

## How to Run
1. Install requirements
2. Run `preprocessing.ipynb`
3. Run `model_training.ipynb`
4. Final predictions are saved in `outputs/submission.csv`
