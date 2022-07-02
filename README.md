# Pima Indians Diabetes Dataset

## Problem Statement
NIDDK (National Institute of Diabetes and Digestive and Kidney Diseases) research creates knowledge about and treatments for the most chronic, costly, and consequential diseases. The dataset used in this project is originally from NIDDK. The objective is to predict whether or not a patient has diabetes, based on certain diagnostic measurements included in the dataset. Build a model to accurately predict whether the patients in the dataset have diabetes or not.

## Dataset Description
The datasets consists of several medical predictor variables and one target variable (Outcome). Predictor variables includes the number of pregnancies the patient has had, their BMI, insulin level, age, and more.

- Pregnancies: Number of times pregnant
- Glucose: Plasma glucose concentration in an oral glucose tolerance test
- BloodPressure: Diastolic blood pressure (mm Hg)
- SkinThickness: Triceps skinfold thickness (mm)
- Insulin: Two hour serum insulin
- BMI: Body Mass Index
- DiabetesPedigreeFunction: Diabetes pedigree function
- Age: Age in years
- Outcome: Class variable (either 0 or 1). 268 of 768 values are 1, and the others are 0

## Project Tasks

### Data Exploration
1. Perform descriptive analysis. Understand the variables and their corresponding values. On the columns below, a value of zero does not make sense and thus indicates missing value:
   - Glucose
   - BloodPressure
   - SkinThickness
   - Insulin
   - BMI
2. Visually explore these variables using histograms. Treat the missing values accordingly.
3. There are integer and float data type variables in this dataset. Create a count (frequency) plot describing the data types and the count of variables.
4. Check the balance of the data by plotting the count of outcomes by their value. Describe your findings and plan future course of action.
5. Create scatter charts between the pair of variables to understand the relationships. Describe your findings.
6. Perform correlation analysis. Visually explore it using a heat map.

### Data Modeling
1. Devise strategies for model building. It is important to decide the right validation framework. Express your thought process.
2. Apply an appropriate classification algorithm to build a model.
3. Compare various models with the results from KNN algorithm.
4. Create a classification report by analyzing sensitivity, specificity, AUC (ROC curve), etc. Please be descriptive to explain what values of these parameter you have used.


### Data Reporting
Create a dashboard in tableau by choosing appropriate chart types and metrics useful for the business. The dashboard must entail the following:
1. Pie chart to describe the diabetic or non-diabetic population
2. Scatter charts between relevant variables to analyze the relationships
3. Histogram or frequency charts to analyze the distribution of the data
4. Heatmap of correlation analysis among the relevant variables
5. Create bins of these age values: 20-25, 25-30, 30-35, etc. Analyze different variables for these age brackets using a bubble chart.

## Summary of Findings

### Data Exploration
- We apply the KNNImputer which performs imputation for completing missing values using k-Nearest Neighbors.
- 66% of the data belongs to Outcome class 0, while 34% belongs to class 1. The result Outcome is unbalanced with more values of class 0. So, this is an unbalanced binary classification problem. To address this problem, in the next section we will use the Synthetic Minority Oversampling Technique (SMOTE) to remove the imbalance in the training data by oversampling the minority class.
- The pairplots suggest that Outcome 0 is more likely among lower Pregnancies, Glucose, SkinThickness, Insulin, BMI, and Age values; whereas Outcome 1 is more likely when most of those variables show higher values.
- Moderate correlation between SkinThickness and BMI (0.657599), Glucose and Insulin (0.640676), and Pregnancies and Age (0.558678).

### Data Modeling
- Before building a classification model, we apply a scaler to our feature variables. Since we did not remove all outliers from the data, we use the RobustScaler to scale our features using statistics that are robust to outliers.
- Also, we use the Borderline Synthetic Minority Oversampling Technique (Borderline-SMOTE) to remove the imbalance in the training data by oversampling the minority class: we oversample those instances of the minority class that are misclassified.
- We look at different classification models and evaluate the models using different classification metrics.
- The more severe error is related to False Negatives, so it's the Type II error. KNN has the lowest Type II error, while LightGBM has the highest.
- GradientBoostingClassifier has the highest accuracy score, while KNN has the lowest. Our test data contains 95 entries from Outcome class 0 and 55 entries from Outcome class 1. Accuracy can be a misleading metric for imbalanced data sets.
- The f1-score combines precision and recall into one metric. This metric is used when we care more about the positive class. GradientBoostingClassifier has the highest f1-score, while LightGBM has the lowest.
- If we care equally about the positive and negative class or your dataset is quite balanced, then going with ROC AUC is a good idea. But if our dataset is heavily imbalanced we should rather consider the Precision-Recall curve and PR AUC (average precision [AP] scores).
- Overall, the GradientBoostingClassifier seems to be the best model: Its Type II error is 0.2 (which is the same for most of the above models), and it has the best accuracy score and f1-score among the above models.
- The top features are: 1. Glucose, 2. Age, 3. Insulin, 4. BMI, 5. SkinThickness, 6. DiabetesPedigreeFunction, 7. Pregnancies, and 8. BloodPressure, which makes sense: Diabetes occurs when the blood glucose (blood sugar) is too high. BMI has a strong relationship to diabetes and insulin resistance. The risk of type 2 diabetes increases as we get older, especially after age 45. The risk of developing type 2 diabetes increases if women developed gestational diabetes when they were pregnant or if they gave birth to a baby weighing more than 9 pounds. [Source 1](https://www.niddk.nih.gov/health-information/diabetes/overview/what-is-diabetes/type-2-diabetes), [Source 2](https://www.mayoclinic.org/diseases-conditions/type-2-diabetes/symptoms-causes/syc-20351193), [Source 3](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4259868/#:~:text=Body%20mass%20index%20has%20a,of%20insulin%20resistance%2C%20is%20increased)
    
### Data Reporting
![Tableau Dashboard](https://github.com/jessi88/pima_indians_diabetes/blob/main/images/Healthcare%20Dashboard.png)

- The population chart, heatmap, histogram plots, and scatter charts are in accordance with our data analysis we did above.
- The three bubble charts show the Average of Outcome, Median of Glucose, and Median of Insulin for the different bins of age values 20-24, 25-29, 30-34, and so on. Each bubble represents a bin of age values. The larger a bubble is, the larger is the Average of Outcome/Median of Glucose/Median of Insulin.
- Average of Outcome: The Average of Outcome is mostly increasing with increasing age values, meaning that the incidence of diabetes increases as we get older.
- Median of Glucose: The Median of Glucose is mostly increasing with increasing age values, meaning that blood glucose levels increase as we get older.
- Median of Insulin: The Median of Insulin is mostly increasing with increasing age values, meaning that insulin levels increase as we get older.

## Links
- [Notebook](https://github.com/jessi88/pima_indians_diabetes/blob/main/pima_indians_diabetes.ipynb)
- [Tableau Dashboard](https://public.tableau.com/views/ProjectHealthcarePGP/HealthcareDashboard?:language=en-US&:display_count=n&:origin=viz_share_link)
