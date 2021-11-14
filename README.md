# Smart Agent Recruitment

## :memo: Description

- This is a classification machine learning problem  to identify the best agents / applicants, for a Financial Distribution company, who will be able to source business for FinMan within 3 months post the 7 day coorporate training.
- In this project the predictions are made using **LightGBM**. Other models like **XGBoost** and **AdaBoost** was also used for experimentation.
- A **Power BI** Dashboard is developed to capture the past trends in agent recruitment and derive meaningful insights from the Data.

## :bar_chart: Power BI Dashboard
The Overview Dashboard and the Applicant Details Dashboard for FinMan Agent Recruitment.<br><br>
<img src="dashboard%20images/Overview%20Dashboard.PNG" height="450" align="middle"><br><br>
<img src="dashboard%20images/Applicant%20Details%20Dashboard.PNG" height="450" align="middle"><br> 

## :file_folder: Code
- [Smart Agent Recruitment.ipnyb](https://github.com/AbhishekGit-hash/Smart-Agent-Recruitment/blob/master/Smart%20Agent%20Recruitment.ipynb)
- [Smart Agent Recruitment.ipynb (nbviewer)](https://nbviewer.org/github/AbhishekGit-hash/Smart-Agent-Recruitment/blob/master/Smart%20Agent%20Recruitment.ipynb)


## :hourglass: Dataset

The dataset **`train.csv`** is used for training. The train dataset had 9,527 records with 23 features.<br>
The dataset consisted the following attributes : 
- **ID** : Application ID for the Applicant.
- **Office_PIN**
- **Application_Receipt_Date**
- **Applicant_City_PIN** 
- **Applicant_Gender** 
- **Applicant_Birthdate** 
- **Applicant_Marital_Status** 
- **Applicant_Occupation** 
- **Applicant_Qualification** 
- **Manager_DOJ** 
- **Manager_Joining_Designation** 
- **Manager_Current_Designation**
- **Manager_Grade**
- **Manager_Status** : Status of Employment of the Manager (Confirmed / Probation).
- **Manager_Gender**
- **Manager_DoB**
- **Manager_Num_Application** : Number of applications sourced by the Manager.
- **Manager_Num_Coded**
- **Manager_Business** : Amount of Business Sourced by the Manager in the last 3 months.
- **Manager_Num_Products** : Number of Produts sold by the Manager in the last 3 months.
- **Manager_Business2** : Amount of Business Sourced by the Manager in the last 3 months excluding the amount sourced by Category A advisor.
- **Manager_Num_Products2** : Number of Produts sold by the Manager in the last 3 months excluding the number sold by Category A advisor.
- **Business_Sourced** : If the Applicant was able to source Business within 3 months (0 : Didn't Source Business , 1 : Sourced Business).

## :page_with_curl: Technical Overview
The project has been divided into the following steps :
### 1. Exploratory Data Analysis

In this step features having missing values and outliers, target variable distribution, numerical feature distribution, categorical feature distribution, Univariate and Bivariate Analysis was performed.<br>
**Some of the data insights are given below**. _(For the detail EDA please refer to the [ipyb notebook](https://github.com/AbhishekGit-hash/Smart-Agent-Recruitment/blob/master/Smart%20Agent%20Recruitment.ipynb))_
- During univariate Analysis, it is oberserved that all the numerical features had skewness.<br><br>
<img src="dashboard%20images/Univariate_Analysis_1.png" height="300" align="middle"><br><br>
<img src="dashboard%20images/Univariate_Analysis_2.png" height="300" align="middle"><br><br>

- The features Manager_Business and Manager_Business2 are highly coorelated. Similarly a high correlation is observed between Manager_Num_Products and Manager_Num_Products2.
**In order to remove multi-colinearity the columns Manager_Business2 and Manager_Num_Products2 will be dropped**.<br><br>
<img src="dashboard%20images/Correlation.png" height="400" align="middle"><br><br>

- As expected there will be a strong correlation between Manager_Num_Products and Manager_Business. As the number of products sold increases the amount of business sourced by the Manager also increased.<br><br>
<img src="dashboard%20images/Business_Products_corr.png" height="400" align="middle"><br><br>

- The peak number of applications were received in the month of May, 2007. In initial months the number of applicatins received was low. However the number increased in the subsequent months.The a huge bulk of applications are received in the months starting from July till December in both the years of 2007 and 2008.<br><br>
<img src="dashboard%20images/Num_App_Yr_Month.png" height="500" align="middle"><br><br>

- It is observed that initially in the period of Apr - Aug 2007, the number of products sold where business was sourced is very lesser than the times when the business was not sourced. The number of products sold where business was sourced started to increase in September, 2007.
The difference between the number of products sold between busniess sourced and non-soucred gradually decreased and this trend continued till March, 2008. There were instances where Number of products sold when business was sourced is more than that when not sourced.<br><br>
<img src="dashboard%20images/Prod_Sold_Yr_Month.png" height="900" align="middle"><br><br>

- On investigating each applications received throughout the time period, a trend is captured. For a particular day the agent's application which was received first or relatively at the beginning of the day was able to source business within 3 months post 7 day training. This pattern is observed across all the 16 months of the train dataset. **This trend will be captured in a feature in the Feature Engineering step**.<br><br>
<img src="dashboard%20images/Application_Select_Status.png" height="900" align="middle"><br><br>

### 2. Data Preprocessing / Cleaning
- **19** features out of **23** had missing values.
- The Arbitray Value imputation is done for handling missing values in the numerical, categorical and date columns / features.
- The date columns were converted to proper datetime data type.
- Irrelevnt features were dropped from train and test datasets.

### 3. Feature Engineering
- In this step 4 extra numerical features were created :
  - `Agent_Age` : The age of the Applicant / agent as on Application Receipt Date.
  - `Manager_Age` : The age of the Manager as on Application Receipt Date.
  - `Manager_Exp` : The work experience of Manager in FinMan organization.
  - `App_Order_Percent` : Percentile of the position of the Application Received calculated at a daily level.
- The categorical features (Applicant_Gender, Applicant_Occupation) were **One Hot Encoded** and (Manager_Joining_Designation, Manager_Current_Designation) were **Label Encoded**.

## 📈 Modelling and Evaluation
- In the modelling part, the following models are used :
  - **XGBoost** (Mean CV Scores : 0.88256, Variance in CV Scores : 0.00564)
  - **Light Gradient Boosting** (Mean CV Scores : 0.8826, Variance in CV Scores : 0.00338)
  - **AdaBoost** (Mean CV Scores : 0.8755 0.000684)
- The scoring metric is **ROC_AUC**.
- **Randomized Search CV** is used for hyperparameter tuning and finding the best parameters under **roc_auc** scoring.

## 	:clipboard: Results
### Feature Importance
#### XGBoost
In the **XGBoost** model, the top 5 features of importance are : **Agent_Age**, **App_Order_percent**, **Manager_Age**, **Applicant_City_PIN** and **Manager_Exp**.<br>
<img src="dashboard%20images/Xgb_Feat_Imp.png" height="500" align="center"><br>
#### LightGBM
In the **LightGBM** model, the top 5 features of importance are : **App_Order_percent**, **Manager_Exp**, **Applicant_City_PIN**, **Office_PIN**, **Manager_Age** and **Agent_Age**.<br> 
<img src="dashboard%20images/lightgbm_feat_imp.png" height="500" align="center"><br>

## :gear: Tools and Technologies used
The tools used in this project include:
- __Python__ - This was needed to conduct Data Quality Assessment, Data Cleaning processes, Exploratory Data Analysis of the datasets and to gain useful insights, feature engineering and building the model.
- __Power BI__ - This <b>Business Intelligence</b> tool was required to explore data and create charts, graphs, dashboard%20images to come up with a <b>Dashboard to capture the past trends in Agent Recruitment</b>.

## :black_nib: Authors
- Abhishek Chowdhury - [Github Profile](https://github.com/AbhishekGit-hash)
