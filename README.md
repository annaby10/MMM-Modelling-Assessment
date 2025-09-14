# MMM-Modelling-Assessment
project:
  title: "Marketing Revenue Modeling (Weekly Data)"
  description: >
    This project implements a two-stage, causally-aware marketing mix model (MMM) 
    for weekly revenue data. The model quantifies the effect of marketing channels, 
    owned channels, pricing, and promotions on revenue, providing actionable 
    insights for marketing strategy.

repository_structure:
  - folder: "notebook"
    content: "Marketing_Revenue_Modeling.ipynb (Final polished notebook)"
  - folder: "data"
    content: "Assessment 2 - MMM Weekly.csv (Dataset)"
  - folder: "output"
    content: 
      - "stage1_google_model.joblib"
      - "stage2_revenue_model.joblib"
      - "stage2_final_coefficients.csv"
      - "stage2_cv_metrics.csv"
      - "residual_plot.png"
      - "sensitivity_plots.png"
  - file: "README.md (This file)"
  - file: "requirements.txt (Python dependencies)"

environment_setup:
  python_version: ">=3.10"
  installation: |
    pip install -r requirements.txt
  dataset_instructions: |
    Place 'Assessment 2 - MMM Weekly.csv' in the 'data/' folder or upload it in Colab.
  notebook_config: |
    Set DATA_PATH variable in the notebook:
    DATA_PATH = 'data/Assessment 2 - MMM Weekly.csv'
  outputs: |
    All outputs are saved in 'output/' folder:
      - Trained models (.joblib)
      - Stage 2 coefficients and CV metrics (.csv)
      - Residual and sensitivity plots (.png)

notebook_overview:
  data_preparation:
    description: >
      Handles weekly seasonality using Fourier features, adds linear trend, 
      fills zeros for missing spend periods, and creates lagged features for 
      email/SMS campaigns.
    transformations: 
      - Adstock + Saturation for media channels (diminishing returns)
      - Log-transform for price
      - Binary flag for promotions
  stage1_mediator_model:
    description: >
      Predicts Google spend as a function of social media channels (Facebook, TikTok, Snapchat).
      Captures the causal pathway: Social → Google → Revenue.
    model: ElasticNetCV with cross-validation
  stage2_outcome_model:
    description: >
      Predicts Revenue using predicted Google spend, social channels, owned channels, 
      price, promotions, and trend/seasonality features.
    model: ElasticNetCV
    validation: TimeSeriesSplit to ensure proper out-of-sample performance
  diagnostics:
    description: >
      Checks model performance, residuals, and stability over time.
    plots: 
      - Residual scatter plot
      - Residuals over time
      - Actual vs predicted revenue
  sensitivity_analysis:
    features: [log_price, promo_flag]
    purpose: >
      Examines how revenue changes with variations in average price (price elasticity)
      and promotion (promo lift)

key_findings:
  revenue_drivers:
    - Google spend: largest direct impact
    - Social media: acts as a demand stimulator (mediated through Google)
    - Price: negative effect (higher price → lower revenue)
    - Promotions: positive lift on revenue
    - Owned channels: small but positive lagged effect (email/SMS)
  tradeoffs:
    - Diminishing returns for higher ad spend
    - Need to balance spend between social and search channels
    - Price vs. demand trade-offs
  model_performance:
    metrics: 
      - RMSE
      - MAE
      - R²
    notes: >
      Rolling CV shows good out-of-sample fit. Residuals show no systematic patterns.
  recommendations:
    marketing_allocation: >
      Focus on channels with direct and mediated revenue impact. Use sensitivity analysis 
      for weekly budget adjustments.
    pricing_promotions: >
      Avoid large price increases without value messaging. Schedule promotions to maximize lift.
    monitoring: >
      Track residuals and metrics weekly. Update model every 6-12 months to maintain accuracy.

reproducibility:
  description: >
    All results are deterministic (RANDOM_STATE=42) and reproducible. 
    Notebook runs sequentially, outputs saved to 'output/' folder.

evaluation_criteria_addressed:
  technical_rigor:
    - Proper preprocessing and feature engineering
    - Time-series cross-validation
    - Handling zero spend periods and sparsity
  causal_awareness:
    - Google-as-mediator explicitly modeled in stage 1
  interpretability:
    - Coefficients, residuals, and sensitivity analysis provide actionable insights
  product_thinking:
    - Recommendations and trade-offs are practical for marketing strategy
  reproducibility_craft:
    - Clean notebook and repository
    - Environment instructions provided
    - Deterministic results

author:
  name: "Your Name"
  email: "your.email@example.com"
  notes: "For questions or clarifications, contact the author."
