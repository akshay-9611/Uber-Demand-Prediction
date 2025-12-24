# 🚕 NYC Uber Demand Prediction

> Predict taxi demand across New York City regions in 15-minute intervals

## 📖 What Does This Project Do?

This machine learning project predicts how many Uber rides will be requested in different areas of New York City. It analyzes historical trip data from January-March 2016 to forecast future demand, helping optimize driver distribution and reduce wait times.

## 🎯 How It Works

1. **Divide NYC into regions** - Uses clustering (K-Means) to split NYC into 30 geographical regions
2. **Clean the data** - Removes outliers and invalid trip records
3. **Create time-based features** - Groups rides into 15-minute intervals and tracks patterns
4. **Build prediction model** - Uses Linear Regression with lag features (previous time periods)
5. **Evaluate performance** - Tests accuracy on March 2016 data

## 🚀 Quick Start

### Prerequisites
- Python 3.x
- DVC (Data Version Control)

### Installation

```bash
# Install dependencies
pip install -r requirements.txt

# Install project as package
pip install -e .
```

### Run the Pipeline

The project uses DVC to manage the entire ML pipeline:

```bash
# Run all stages (data ingestion → feature extraction → training → evaluation)
dvc repro

# Or run individual stages:
dvc repro data_ingestion      # Clean and prepare raw data
dvc repro extract_features     # Create regions and time features
dvc repro feature_processing   # Generate lag features
dvc repro train                # Train the model
dvc repro evaluate             # Test the model
```

## 📁 Project Structure

```
├── data/
│   ├── raw/              # Original NYC taxi trip CSVs
│   ├── interim/          # Cleaned data without outliers
│   └── processed/        # Final training/test datasets
├── models/               # Saved models (scaler, kmeans, encoder, linear model)
├── notebooks/            # Jupyter notebooks for exploration and analysis
├── src/
│   ├── data/            # Data ingestion and cleaning
│   ├── features/        # Feature engineering scripts
│   └── models/          # Training and evaluation scripts
├── params.yaml          # Hyperparameters (clusters, EWMA alpha, etc.)
└── dvc.yaml            # ML pipeline definition
```

## 🔧 Key Parameters

Edit `params.yaml` to adjust:
- **n_clusters**: 30 (number of NYC regions)
- **alpha**: 0.4 (EWMA smoothing for demand averaging)

## 📊 Features Used

- **Lag features**: Previous 4 time periods (1-4 lags of 15 minutes)
- **Region**: Which area of NYC (0-29)
- **Day of week**: Monday-Sunday
- **Month**: Seasonal patterns
- **Average pickups**: Smoothed historical demand (EWMA)

## 🎓 Model Performance

The model is evaluated using **MAPE** (Mean Absolute Percentage Error) on March 2016 data.

## 🛠️ Tech Stack

- **Data Processing**: Pandas, Dask (for large datasets)
- **ML Libraries**: Scikit-learn
- **Pipeline Management**: DVC
- **Model**: Linear Regression with One-Hot Encoding

## 📝 Notes

- Training data: January & February 2016
- Test data: March 2016
- Data covers pickup locations within NYC boundaries (40.60-40.85 lat, -74.05 to -73.70 long)
- Fare range: $0.50 - $81.00
- Trip distance: 0.25 - 24.43 miles
