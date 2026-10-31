# Ford Used Car Price Prediction

A machine learning project to predict the selling price of used Ford cars based on their specifications and vehicle condition.

## Project Overview

This project develops a regression model to estimate the listing price of used Ford cars using historical vehicle data. The model analyzes several factors, including car model, registration year, transmission, mileage, fuel type, fuel efficiency, and engine size.

The project follows the **CRISP-DM (Cross-Industry Standard Process for Data Mining)** methodology, covering business understanding, data understanding, data preparation, modeling, evaluation, and deployment.

**Final Model:** XGBoost Regressor  
**Target Variable:** `price`  
**Currency:** British Pound Sterling (£)

## Live Application

Try the deployed application:

**[Ford Used Car Price Prediction App](https://prediksi-harga-ford-enuj92mec66y2kozqucnnj.streamlit.app/)**

Users can enter vehicle specifications and receive an estimated used car price.

## Business Problem

Determining a fair price for a used car can be challenging because vehicle prices are influenced by multiple factors, such as vehicle age, mileage, model, and engine specifications.

This project aims to answer:

> How can we estimate a reasonable selling price for a used Ford car based on its specifications?

The prediction system can help:
- Sellers estimate a reasonable listing price.
- Buyers evaluate whether a car is potentially overpriced or underpriced.
- Dealers estimate vehicle values more consistently.

## Dataset

The dataset contains used Ford car listings with the following characteristics:

| Description | Value |
|---|---:|
| Initial records | 17,966 |
| Initial features | 9 |
| Year range | 1996–2020 |
| Target variable | `price` |
| Currency | GBP (£) |

### Features

| Feature | Description |
|---|---|
| `model` | Ford car model |
| `year` | Year of registration |
| `price` | Listing price (target) |
| `transmission` | Transmission type |
| `mileage` | Distance traveled |
| `fuelType` | Fuel type |
| `tax` | Annual vehicle tax |
| `mpg` | Miles per gallon |
| `engineSize` | Engine capacity |

The original dataset is available in `ford.csv`.

## Exploratory Data Analysis (EDA)

EDA was conducted to understand the relationships between vehicle characteristics and selling prices.

### 1. Year vs Price

![Year vs Price](images/Year%20vs%20Price.png)

The analysis shows a positive relationship between registration year and car price. Newer vehicles generally have higher prices than older vehicles.

### 2. Mileage vs Price

![Mileage vs Price](images/Milage%20vs%20Price.png)

Mileage has a negative relationship with car price. Vehicles with lower mileage generally have higher selling prices, while heavily used vehicles tend to have lower prices.

### 3. Engine Size vs Price

![Engine Size vs Price](images/Engine%20Size%20vs%20Price.png)

Engine capacity is another factor associated with vehicle prices. Cars with larger engines tend to have higher prices in this dataset. However, the number of observations in some engine-size groups is relatively small, so the result should be interpreted carefully.

### 4. Model Segmentation

![Model Segmentation](images/Model%20Segmentation.png)

Price distributions vary across Ford models. This indicates that the car model is an important factor in determining the estimated selling price.

## Data Preparation

Several preprocessing steps were performed to improve data quality:

1. **Data inspection:** Examined dataset dimensions, data types, missing values, and duplicate records.
2. **Text cleaning:** Removed unnecessary whitespace from car model names.
3. **Duplicate removal:** Removed duplicate records.
4. **Invalid value handling:** Identified and handled invalid vehicle records, including an invalid registration year.
5. **Engine size handling:** Treated zero engine-size values as missing information and handled them through the modeling pipeline.
6. **Feature selection:** Selected relevant vehicle specifications for price prediction.

The original dataset contained 17,966 records, including 154 duplicate rows. After removing duplicates and two invalid records, the cleaned dataset contained 17,810 records.

The `tax` feature was excluded from the final application to simplify the information users need to provide.

## Modeling

This project uses regression algorithms to predict continuous car prices.

### Model Experiments

Several algorithms and feature configurations were evaluated:

- Linear Regression
- Decision Tree Regressor
- Random Forest Regressor
- Gradient Boosting Regressor
- HistGradientBoosting Regressor
- XGBoost Regressor

Different feature configurations were also compared, including original features, engineered features, and log-transformed target values.

### Model Selection

Model selection was based on **5-fold cross-validation RMSE on the training data**. The test set was kept separate for final evaluation.

The selected model was:

**XGBoost Regressor with original features, without the `tax` feature, after hyperparameter tuning.**

### Final Hyperparameters

| Parameter | Value |
|---|---:|
| `n_estimators` | 700 |
| `learning_rate` | 0.03 |
| `max_depth` | 8 |
| `min_child_weight` | 1 |
| `subsample` | 0.7 |
| `colsample_bytree` | 0.9 |
| `reg_lambda` | 10 |

## Model Evaluation

The dataset was divided into training and testing sets using an 80:20 split, with `random_state=42`.

The final model was evaluated on 3,562 test records.

### Final Performance

| Metric | Result |
|---|---:|
| R² | 0.946 |
| RMSE | £1,104 |
| MAE | £793 |
| MAPE | 6.90% |
| Predictions within 10% of actual price | 78.8% |

### Interpretation

- **R² = 0.946:** The model explains approximately 94.6% of the variation in test-set prices.
- **RMSE = £1,104:** The root mean squared prediction error is approximately £1,104.
- **MAE = £793:** The average absolute prediction error is approximately £793.
- **MAPE = 6.90%:** The mean absolute percentage error is 6.90%.
- **Within 10% = 78.8%:** Approximately 78.8% of test predictions differ from actual prices by no more than 10%.

These results indicate that the final model provides a strong estimate of used Ford car listing prices on the test dataset.

## Prediction Features

The deployed application uses seven input features:

1. `model`
2. `year`
3. `transmission`
4. `mileage`
5. `fuelType`
6. `mpg`
7. `engineSize`

The trained model is saved as:

`ford_price_model_terbaik.pkl`

## Project Structure

```text
ford-used-car-price-prediction/
│
├── deploy-ford-car/
│   ├── analisis_regresi_ford.py
│   ├── app.py
│   ├── cek_model.py
│   ├── ford_price_model_terbaik.pkl
│   └── requirements.txt
│
├── images/
│   ├── Engine Size vs Price.png
│   ├── Milage vs Price.png
│   ├── Model Segmentation.png
│   └── Year vs Price.png
│
├── ford.csv
├── prediksi_harga_ford rev2.ipynb
├── .gitignore
└── README.md
```

## Technologies Used

- **Python**: Main programming language.
- **Pandas & NumPy**: Data manipulation and numerical computation.
- **Matplotlib & Seaborn**: Data visualization.
- **Scikit-learn**: Data preprocessing, model evaluation, and machine learning pipelines.
- **XGBoost**: Final regression model.
- **Streamlit**: Interactive web application.
- **Jupyter Notebook / Google Colab**: Data analysis and model development.
- **Git & GitHub**: Version control and project documentation.

## Conclusion

This project demonstrates an end-to-end machine learning workflow for used car price prediction, from data exploration and preprocessing to model comparison, evaluation, and deployment.

The final XGBoost model achieved an R² of 0.946 and an RMSE of £1,104 on the test dataset. The deployed Streamlit application allows users to estimate used Ford car prices using seven vehicle specifications.

## Future Improvements

- Expand the dataset to include more vehicle brands and listings.
- Evaluate model performance on newer vehicle listings.
- Explore additional features that may influence used car prices.
- Improve the application interface and prediction explanations.
