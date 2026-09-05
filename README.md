# Pakistan Property Price Predictor

A machine learning application that predicts property prices in Pakistan using real estate data from multiple cities. Built with scikit-learn and deployed as an interactive web app with Streamlit.

## Live Demo

🌐 **[Try the app on Streamlit Cloud](https://pakistan-property-price-guesser.streamlit.app/)**

## Features

- **Accurate Price Predictions** - Powered by an optimized Random Forest ensemble ($R^2 \approx 0.886$).
- **Multi-City Support** - Covers Islamabad, Rawalpindi, Lahore, Faisalabad, and Karachi.
- **Inflation & Appreciation Awareness** - Dynamically accounts for post-2020 macroeconomic inflation/appreciation factors alongside the historical baseline.
- **Efficient Deployment** - Serialized pipeline reduced from 1.4 GB down to ~92 MB via compression for fast cloud loading and standard Git tracking.
- **Interactive Interface** - Clean Streamlit web interface with Lakh and Crore valuation formatting.

## Dataset

- **Features**: City, location, property type, bedrooms, bathrooms, area size (Marla / Sq. Ft.), purpose
- **Target**: Property price in PKR (Pakistani Rupees)

## Model Performance

**Final Deployed Model: Random Forest (Smaller / Optimized)**
- **$R^2$ Score**: 0.8861
- **MAE (Mean Absolute Error)**: ₨2,963,513 (~₨2.96M)
- **RMSE (Root Mean Squared Error)**: ₨11,605,720 (~₨11.61M)
- **Trees (`n_estimators`)**: 130
- **Size**: ~92 MB (compressed with gzip level 9, reduced from 1.4 GB)

### Comprehensive Model Comparison

| Model | MAE (PKR) | RMSE (PKR) | $R^2$ Score | Difference vs. Smaller RF |
| :--- | :---: | :---: | :---: | :---: |
| **Random Forest (Original - 1.4 GB)** | 2.96M | 11.59M | 0.8864 | +0.0003 |
| **Random Forest (Smaller)** *(Deployed)* | **2.96M** | **11.61M** | **0.8861** | **Baseline (Deployed)** |
| **Random Forest (Tuned)** | 3.00M | 12.39M | 0.8702 | -0.0159 |
| **XGBoost** | 3.93M | 13.03M | 0.8565 | -0.0296 |
| **HistGradientBoosting** | 3.83M | 13.35M | 0.8493 | -0.0368 |
| **Linear Regression** | 13.94M | 30.49M | 0.2138 | -0.6723 |

> **Key Architectural Takeaways:**
> - The difference between the original 1.4 GB ensemble and the smaller version is negligible (a difference of only **0.0003 in $R^2$** and ~0.2% in MAE/RMSE), while reducing disk and memory usage by **over 93%**.
> - Tree bagging models significantly outperformed gradient boosting and linear approaches due to high categorical cardinality and localized non-linearities across neighborhoods.

## Performance & Deployment Optimization

1. **Size Optimization:** The default scikit-learn Random Forest serialized at over **1.4 GB**, exceeding GitHub's 100 MB limit and causing memory issues. Pruning `n_estimators` to 130 and applying `joblib.dump(..., compress=('gzip', 9))` reduced it to **~92 MB**.
2. **Native Git Versioning:** Staying under 100 MB allows tracking the model directly in the Git repository without requiring Git LFS or external cloud storage.
3. **Streamlit Resource Caching:** The pipeline loads once in memory using `@st.cache_resource`, ensuring sub-100ms prediction latency.
4. **Runtime Pinning:** Python 3.11 is pinned via `.python-version` to ensure Streamlit Cloud uses pre-built wheels, preventing build timeouts and compilation hangs.

## Installation

### Prerequisites
- Python 3.10 or 3.11
- pip or conda

### Setup

1. **Clone the repository**
   ```bash
   git clone [https://github.com/o0on/pakistan-property-price-predictor.git](https://github.com/o0on/pakistan-property-price-predictor.git)
   cd pakistan-property-price-predictor

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

- Data source: https://opendata.com.pk/dataset/property-data-for-pakistan/
- Built with: [Streamlit](https://streamlit.io/), [scikit-learn](https://scikit-learn.org/), [Pandas](https://pandas.pydata.org/)
- Deployed on: [Streamlit Cloud](https://streamlit.io/cloud)

## Contact & Support

For issues, questions, or suggestions, please open an [Issue](https://github.com/o0on/pakistan-property-price-predictor/issues) on GitHub.

---

**Last Updated**: September 2026
