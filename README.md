# Marketing Revenue Modeling (Weekly Data)

This project implements a **two-stage, causally-aware Marketing Mix Model (MMM)** to quantify the effects of marketing channels, pricing, promotions, and owned channels on weekly revenue. The goal is to provide actionable insights for marketing strategy.

---

# Notebook Overview

The notebook is organized into the following sections:
1. Title & Instructions

Provides project title, purpose, and instructions to run in Google Colab or Jupyter.
Explains dataset upload and path configuration.

2. Environment & Imports

Imports necessary Python libraries (pandas, numpy, sklearn, matplotlib, seaborn, etc.).
Sets a random seed for reproducibility.

3. Configuration

Defines column names from the dataset for revenue, channels, price, promotions, and followers.
Sets output directory for saving models, metrics, and plots.

4. Data Loading

Loads the CSV dataset into a DataFrame and displays the first few rows.

5. Preprocessing & Time Features

Prepares a linear time index.
Handles weekly seasonality using Fourier features.
Ensures missing and zero values are handled appropriately.

6. Media Transformations

Applies adstock to social and Google spend to capture lagged effects.
Applies saturation functions (log or hill) to capture diminishing returns.

7. Price, Promotions & Owned Channels

Log-transform for price.
Binary flag for promotions.
Lagged variables for email and SMS campaigns.

8. Target Transformation

Applies log1p transformation to revenue to stabilize variance.

9. Stage 1: Mediator Model

Predicts Google spend using social media channels as inputs.
Saves predicted Google spend (google_hat) for use in Stage 2.
Causal perspective: Social → Google → Revenue.

10. Stage 2: Outcome Model

Predicts revenue using predicted Google spend, social channels, price, promotions, owned channels, and trend/seasonality.
Uses ElasticNet regression with TimeSeriesSplit cross-validation.
Outputs final coefficients and CV metrics, saved to output/.

11. Diagnostics

Residual plots: scatter plot and residuals over time.
Checks model fit and identifies potential issues.

12. Sensitivity Analysis

Simulates revenue changes for variations in average price (price elasticity).
Simulates revenue changes for promotion on/off (promotion lift).
Provides actionable insights for marketing trade-offs.

13. Insights & Recommendations

Identifies key revenue drivers: Google spend, price, promotions, and owned channels.
Highlights diminishing returns for high ad spend.
Suggests trade-offs between social vs. search channels.
Provides practical recommendations for marketing allocation decisions.


---

## Environment Setup

1. **Python Version**: 3.10+  
2. **Install required packages**:

```bash
pip install -r requirements.txt
DATA_PATH = 'data/Assessment 2 - MMM Weekly.csv'
