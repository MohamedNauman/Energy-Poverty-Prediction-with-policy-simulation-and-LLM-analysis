This project focused on analyzing and predicting household power consumption, and simulating the impact of energy efficiency policies. Here's a breakdown of the process:

1. Data Loading and Initial Exploration

The project started by loading the household_power_consumption.txt dataset into a pandas DataFrame. Initial checks revealed the dataset contains over 2 million entries with 9 columns, including Date, Time, and various power consumption metrics.
Data types were inspected, showing Date and Time as objects, and other power metrics as float64. Missing values were identified across all numerical power columns, with each having 25,979 null entries.
2. Data Preprocessing

Time Series Adjustment: The Date and Time columns were combined and converted into a single datetime index, making the DataFrame suitable for time series analysis. The original Date and Time columns were then dropped.
Null Handling: Missing values were handled using time-based interpolation (method='time'), effectively filling all 25,979 nulls in each numerical column.
Resampling and Feature Engineering: The data was resampled to an hourly frequency, and new time-based features (hour, day_of_week, month, year) were extracted from the datetime index. This reduced the dataset size from over 2 million to 34,589 hourly entries.
Normalization: Key consumption features (Global_active_power, Global_reactive_power, Voltage, Global_intensity, Sub_metering_1, Sub_metering_2, Sub_metering_3) were scaled using MinMaxScaler to a range of (0,1), preparing them for machine learning models.
3. Model Training and Evaluation

Data Splitting: The processed data was split into training and testing sets (80/20 split) with Global_active_power as the target variable (y) and other relevant features as predictors (X). Global_intensity was dropped as it's highly correlated with Global_active_power.
Exploratory Data Analysis (EDA): Correlation analysis and visualizations (time series plots, box plots by hour and month) were performed to understand data distributions and relationships.
Model Selection and Performance: Several regression models were trained and evaluated based on RMSE and R2 score:
Linear Regression: Achieved an RMSE of 0.0504 and R2 of 0.8016.
MLP Regressor (Multi-Layer Perceptron): After scaling features with StandardScaler, the MLP model achieved the best performance with an RMSE of 0.0416 and R2 of 0.8647. This model was chosen for policy simulation.
Random Forest Regressor: Performed strongly with an RMSE of 0.0421 and R2 of 0.8616. Further hyperparameter tuning was explored.
XGBoost Regressor: Achieved an RMSE of 0.0428 and R2 of 0.8571.
Feature Importance: SHAP (SHapley Additive exPlanations) values were used to analyze feature importance for the best-performing MLP model, providing insights into which features drive predictions.
4. Policy Simulation and Analysis

An energy efficiency policy targeting low-income households was simulated, involving reductions in sub-metered consumption (e.g., 10% for Sub_metering_1, 15% for Sub_metering_2, 5% for Sub_metering_3) and an overall reduction in Global_active_power.
Three policy scenarios were defined: 'Original Policy', 'Aggressive Policy', and 'Moderate Policy', each with different reduction percentages.
The MLP model was used to predict energy consumption under each simulated policy. The results showed a percentage reduction in mean energy consumption across all scenarios:
Original Policy: 3.76% reduction
Aggressive Policy: 11.17% reduction (highest impact)
Moderate Policy: 1.83% reduction
Visualizations (histograms, box plots, time series comparisons) demonstrated a clear downward shift in predicted consumption under policy scenarios compared to the baseline, indicating the effectiveness of the interventions.
5. LLM-Powered Policy Analysis

The simulated results, along with a detailed policy description and qualitative insights, were fed into a Cohere LLM (Large Language Model) to generate an enriched summary, specific policy recommendations, and explore potential alternative scenarios or unintended consequences.
6. Gradio Interface

A user-friendly Gradio interface was developed (code in cell tfdXSmFrkUVz) to interact with the trained model and policy simulations. The interface consists of four main tabs:

Individual Prediction 💡: Allows users to input specific energy parameters (date/time, reactive power, voltage, sub-metering values) and receive an instant prediction of Global Active Power consumption in kW.

Policy Simulation 📈: Enables users to select one of the predefined policy scenarios ('Original Policy', 'Aggressive Policy', 'Moderate Policy'). Upon selection, it displays a summary of the simulated impact, including mean baseline and policy predictions, and the percentage reduction in mean energy consumption. A time series plot visualizes the actual consumption, baseline prediction, and the selected policy's prediction over the first 200 hours.

Scenario Comparison 📊: This tab provides a comparative visualization of all policy scenarios against the actual consumption and baseline prediction. A single time series plot illustrates the effect of each policy over the first 300 hours, allowing for easy comparison of their impacts.

LLM Analysis 🧠: By clicking a button, users can trigger an LLM (Cohere) to generate an in-depth analysis of the policy simulations. This includes an enriched summary of the policy's impact, specific policy recommendations, and an exploration of potential alternative scenarios or unintended consequences, leveraging the quantitative and qualitative insights from the project.

In conclusion, the project successfully built a predictive model for household power consumption and demonstrated a data-driven approach to evaluating the potential impact of energy efficiency policies, providing valuable insights for decision-makers, all encapsulated within an intuitive Gradio application.

**To move your Gradio app to Hugging Face Spaces, you'll need to create an app.py file and a requirements.txt file in your repository:**

1. Create app.py: Copy the code from cell DxpCj7MWfzlI into a new file named app.py.
2. Create requirements.txt: Create a file named requirements.txt in the same directory with the following content:
gradio
pandas
numpy
scikit-learn
joblib
matplotlib
seaborn
cohere
xgboost
shap
3. Upload to Hugging Face Spaces: Upload these two files (app.py and requirements.txt) to your Hugging Face Space repository.
4. Cohere API Key: Remember to set your COHERE_API_KEY as a Space Secret in your Hugging Face Space settings for the app to function correctly.
