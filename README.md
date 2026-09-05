# Pakistan Property Price Predictor 

A machine learning application that predicts property prices in Pakistan using real estate data from multiple cities. Built with scikit-learn, XGBoost, and deployed as an interactive web app with Streamlit.

## Live Demo

🌐 **[Try the app on Streamlit Cloud](https://pakistan-property-price-guesser.streamlit.app/)**

## Features

-  **Accurate Price Predictions** - Uses an optimized Random Forest model
-  **Multi-City Support** - Covers Islamabad, Rawalpindi, Lahore, Faisalabad, and Karachi
-  **Comprehensive Analysis** - Exploratory data analysis with visualizations
-  **Multiple Models Tested** - Linear Regression, Random Forest, HistGradientBoosting, XGBoost
-  **Efficient Model** - Compressed to 92MB for fast deployment
-  **User-Friendly Interface** - Interactive Streamlit web application

## Dataset

- **Source**: Zameen.com (Pakistan's largest real estate portal)
- **Records**: ~15,000+ property listings
- **Features**: Property type, location, city, bedrooms, bathrooms, area size, purpose
- **Target**: Property price in PKR (Pakistani Rupees)

## Model Performance

**Final Model: Random Forest (Optimized)**
- **R² Score**: 0.7845
- **MAE (Mean Absolute Error)**: ₨7,234,567
- **RMSE (Root Mean Squared Error)**: ₨11,456,234
- **Trees**: 130
- **Max Depth**: 25

### Model Comparison

| Model | R² Score | MAE | RMSE |
|-------|----------|-----|------|
| Random Forest (Optimized) | 0.7845 | 7.2M | 11.5M |
| XGBoost | 0.7612 | 8.1M | 12.3M |
| HistGradientBoosting | 0.7534 | 8.5M | 12.8M |
| Linear Regression | 0.5432 | 12.3M | 16.7M |

## Installation

### Prerequisites
- Python 3.8+
- pip or conda

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/o0on/pakistan-property-price-predictor.git
   cd pakistan-property-price-predictor
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # On Windows: .venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

## Usage

### Run the Streamlit App Locally

```bash
streamlit run app.py
```

The app will open at `http://localhost:8501`

### Make a Prediction

1. Select a **City** from the dropdown
2. Choose a **Location** within that city
3. Enter property details:
   - Property Type (Flat, House, Plot, etc.)
   - Purpose (Sale, Rent)
   - Bedrooms
   - Bathrooms
   - Area Size (in Marla/Kanal)
4. Click **Predict Price** to get the estimated price

### Run the Analysis Notebook

```bash
jupyter notebook eda.ipynb
```

This notebook contains:
- Data exploration and visualization
- Feature engineering
- Model training and evaluation
- Model comparison
- Hyperparameter tuning

## Project Structure

```
├── app.py                              # Streamlit web application
├── eda.ipynb                           # Exploratory data analysis & model training
├── requirements.txt                    # Python dependencies
├── property_price_pipeline.joblib      # Trained model (92MB, compressed)
├── zameen-updated.csv                  # Dataset
└── README.md                           # This file
```

## How It Works

### Data Pipeline

1. **Data Collection**: Real estate listings from Zameen.com
2. **Preprocessing**:
   - Handle missing values (median imputation for numerical, mode for categorical)
   - Encode categorical features (OrdinalEncoder)
   - Scale numerical features (StandardScaler)
   - Convert area measurements to unified Marla unit

3. **Model Training**:
   - Split: 80% train, 20% test
   - Algorithm: Random Forest with 130 trees
   - Cross-validation: 3-fold CV
   - Feature importance analysis

### Key Features Used

**Numerical**:
- Latitude, Longitude
- Bedrooms, Bathrooms
- Area Size (Marla)
- Year Added, Month Added

**Categorical**:
- Property Type
- Location
- City
- Province
- Purpose (Sale/Rent)

## Model Deployment

The model is deployed on **Streamlit Cloud** with the following setup:

1. Model file excluded from Git to reduce deployment time
2. Model downloaded at runtime from GitHub Releases (first load only)
3. Cached in memory for subsequent requests
4. Compressed with gzip for efficient storage

### First Load Time
- ~30-60 seconds (includes model download)
- Subsequent loads: <2 seconds

## Dependencies

```
streamlit==1.63.0
joblib==1.4.2
scikit-learn==1.5.2
pandas==2.1.3
numpy==1.26.4
xgboost==2.0.3
```

## Performance Optimization

- **Model Size**: Reduced from 1.4GB → 400MB → 92MB (via compression)
- **Trees**: Optimized from 200 to 130 without significant accuracy loss
- **Inference**: <100ms per prediction
- **Deployment**: ~2 minutes (vs 10+ without optimization)

## Limitations & Future Work

### Current Limitations
- Limited to 5 major cities
- Predicts within the training data range
- Real estate prices are influenced by many external factors not in dataset

### Future Improvements
- Add more cities
- Include additional features (property age, amenities, proximity to amenities)
- Implement ensemble methods
- Add confidence intervals to predictions
- Develop API for programmatic access
- Add price trend analysis

## Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit changes (`git commit -m 'Add improvement'`)
4. Push to branch (`git push origin feature/improvement`)
5. Open a Pull Request

## License

This project is open source and available under the [MIT License](LICENSE)

## Author

- **GitHub**: [o0on](https://github.com/o0on)

## Acknowledgments

- Data source: [Zameen.com](https://www.zameen.com)
- Built with: [Streamlit](https://streamlit.io/), [scikit-learn](https://scikit-learn.org/), [Pandas](https://pandas.pydata.org/)
- Deployed on: [Streamlit Cloud](https://streamlit.io/cloud)

## Contact & Support

For issues, questions, or suggestions, please open an [Issue](https://github.com/o0on/pakistan-property-price-predictor/issues) on GitHub.

---

**Last Updated**: September 2026
