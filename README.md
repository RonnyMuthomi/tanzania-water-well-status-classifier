# Water Wells Functionality Condition

##  Predicting Water Well Functionality in Tanzania Using Machine Learning

### Author: Ronny Muthomi


## The Problem

![image](https://github.com/user-attachments/assets/e6b75bc7-af2e-418b-9e32-4dea963a9343)


Access to clean and functional water sources remains a major challenge in Tanzania, especially in rural and underserved regions. Although thousands of water wells have been installed across the country, a significant number are either non-functional or in need of repair. The lack of timely maintenance, poor infrastructure planning, and limited resource allocation contribute to this issue, leaving many communities without reliable access to safe water. Currently, there is no efficient, data-driven system in place to proactively identify which water wells are likely to fail or require repairs. This results in reactive maintenance, inefficient use of government or NGO resources, and prolonged water shortages for affected populations.

**The core problem is the absence of a predictive system that can classify the operational status of water wells (functional, functional but needs repair, or non-functional)** based on available well-related features. Without such a system, stakeholders cannot easily prioritize interventions, optimize maintenance schedules, or learn from patterns of failure to improve future water point installations.


## Business Understanding

Tanzania struggles with providing reliable access to clean water, especially in rural areas. Many existing water wells are non-functional or in need of repair, impacting millions. This project aims to build a machine learning model that predicts the condition of water wells based on features like pump type, construction year, and location. The model will support NGOs, government agencies, and donors in identifying at-risk wells and prioritizing repairs, ultimately improving water access and resource allocation.

![image](https://github.com/user-attachments/assets/08ab02ee-12c9-4f16-83ff-e1d0644593a0)

In this project, the goal is to develop a machine learning-based classification model that can predict the current condition of a water well (functional, functional but needs repair, or non-functional) using historical and descriptive data such as the type of pump, construction year, water source, and geographic features. Such a model can support early identification of at-risk wells and optimize the deployment of limited maintenance and rehabilitation resources.

The **key stakeholders** of this project is a **Non-Governmental Organizations (NGOs)** focused on water access and rural development, who need to identify which wells to prioritize for repair.


## Data Understanding

The dataset comprises information about over 59,000 water points across Tanzania, collected to assess their operational status. Each record includes both numerical and categorical attributes, such as location coordinates, pump type, water source, construction year, and management details. The target variable, **status_group**, indicates whether a water point is **functional, functional needs repair, or non functional**. A preliminary analysis revealed data quality issues such as missing values, inconsistent labels, and class imbalance—especially underrepresentation of the "functional needs repair" category. Understanding these variables and their relationships is critical to building a model that can help NGOs and government agencies target interventions effectively. Data source Data-driven[https://www.drivendata.org/competitions/7/pump-it-up-data-mining-the-water-table/data/]


## Data Preparation

Preparing the dataset for modeling was a critical step, given its diverse structure and real-world complexity. The initial dataset contained a mix of numerical, categorical, and text-based features, some of which were incomplete, inconsistent, or irrelevant to the predictive task. Missing values in numerical features like gps_height and amount_tsh were addressed using median imputation, while categorical features such as scheme_management and installer were imputed using the most frequent category. High-cardinality fields like funder, installer, and scheme_name, which had many unique but rarely repeated values, were grouped or excluded to reduce dimensionality and improve model generalization.

![image](https://github.com/user-attachments/assets/80214717-c2bf-40ce-9a57-02eb5333dd18)

To ensure the model could effectively interpret the features, I scaled numerical data using **Min-Max scaling to normalize ranges**, and **encoded categorical variables using one-hot encoding**. This allowed algorithms like **Logistic Regression** and **Random Forest** to treat all inputs consistently. I engineered new features or consolidated for example, the three-level extraction fields (extraction_type, extraction_type_group, extraction_type_class) I reviewed for redundancy. For features that were either leaky, high-noise, or non-informative I dropped for effeciency of the model.

![image](https://github.com/user-attachments/assets/bda84608-2547-44c3-af4f-875e36cdb729)

I addressed class imbalance  at the modeling stage. Since the target variable status_group is imbalanced, with the **functional needs repair** class underrepresented, class weighting (class_weight='balanced') was applied in model training to ensure fair treatment of all classes. These preparation steps ensured that the dataset was clean, informative, and structured optimally for model training and evaluation.

![image](https://github.com/user-attachments/assets/aa84ff72-d0ae-4b21-ab9a-c06feb86540b)


## **Exploratory Data Analysis (EDA)**

The purpose of Exploratory Data Analysis (EDA) is to gain a better understanding of the dataset by visualizing patterns, identifying anomalies, and uncovering relationships between variables. In this project, EDA is tailored to support the key objectives of predicting water well conditions in Tanzania.

### **1. Proactive Identification of At-Risk or Failing Wells**

Understanding which wells are likely to fail helps stakeholders plan interventions before problems arise.

- **Distribution of Well Status**
  
  
 ![image](https://github.com/user-attachments/assets/ecc2b52d-6a9e-4c6e-8dd9-1995c8949e48)

  
  > Most wells are functional, but a significant portion are non-functional or need repair. This highlights the importance of early intervention.

- **Well Age vs. Status**
  
  Older wells are generally more prone to failure.
  
  ![image](https://github.com/user-attachments/assets/fbc69037-411b-436e-951d-670cce175cf8)

  
  > Non-functional wells tend to be older, suggesting maintenance needs increase with well age.


### **2. Efficient Allocation of Maintenance Resources**

Targeting areas with higher concentrations of failing wells can help prioritize limited resources.

- **Well Status by Region**
  
  ![image](https://github.com/user-attachments/assets/86d5cc82-6872-424a-8f55-ea9390f93ed5)

  > Some regions have a higher share of failing wells, guiding NGOs where to direct maintenance teams.


Through this EDA, I identified critical patterns that inform predictive modeling and support decision-making for well maintenance, resource allocation, and sustainable water access.


## Modeling

For the modeling phase I began with a simple and interpretable baseline model **Logistic Regression**. As a linear classifier, Logistic Regression provided a fast and transparent starting point to establish a benchmark for performance. To address class imbalance in the target variable (status_group), I configured the model with class_weight='balanced'. Performed Hyperparameter tuning  using GridSearchCV on the regularization parameter C to find an optimal balance between underfitting and overfitting.

Next, I implemented **Decision Tree Classifier**  to capture non-linear patterns and feature interactions that a linear model might miss. Decision trees offer clear decision rules and interpretability, making them ideal for understanding which features most influence predictions. Hyperparameters such as max_depth, min_samples_split, and min_samples_leaf were tuned to improve generalization and reduce overfitting.

Finally, a Implemented a **complex ensemble model**, **Random Forest**. The model aggregates predictions from multiple decision trees and generally improves robustness and accuracy. A wide hyperparameter grid was used—tuning values like n_estimators, max_depth, and bootstrap—to extract the best-performing configuration. Across all models, 5-fold cross-validation and F1-weighted scoring were used to ensure fair and consistent evaluation, especially in the presence of class imbalance.

![image](https://github.com/user-attachments/assets/e2ebd195-51bc-4aeb-a12f-2a2e59e65ade)



## Evaluation

To evaluate the performance of each model, multiple classification metrics were used, considering the multi-class nature of the problem and the class imbalance, especially the minority class "functional needs repair". The main metrics used were accuracy, precision, recall, and F1-score, obtained through classification_report, confusion_matrix, and cross-validation strategies.

Accuracy provided a general idea of the model’s correctness, but since the dataset was imbalanced, F1-score (weighted) became the primary evaluation metric. The weighted F1-score considers the harmonic mean of precision and recall while giving importance to each class according to its frequency. This helped ensure the model didn’t ignore minority classes like "functional needs repair".

The baseline Logistic Regression model achieved moderate performance, with decent precision but struggled with recall for underrepresented classes. The Decision Tree slightly improved interpretability but showed tendencies to overfit. The Random Forest, benefiting from ensemble learning and hyperparameter tuning, outperformed the others in both accuracy and F1-score, indicating better generalization and handling of class imbalance. The evaluation results, particularly confusion matrices, revealed that while all models performed best on predicting "functional" wells, more effort is needed in improving detection of "needs repair" status.

## Hyperparameter Tuning

To enhance model performance, especially in handling the imbalanced classes, hyperparameter tuning was performed using GridSearchCV. This allowed exhaustive search across defined parameter grids with cross-validation (cv=5) and the F1-weighted score as the evaluation metric.

For Logistic Regression, the regularization strength C was tuned to find the optimal balance between bias and variance. For the Decision Tree, parameters such as max_depth, min_samples_split, and min_samples_leaf were varied to reduce overfitting. The Random Forest model required a more extensive grid, tuning n_estimators, max_depth, min_samples_split, min_samples_leaf, and bootstrap. Despite the longer training time, this approach significantly improved performance, especially for underrepresented classes.

After tuning, the Random Forest model emerged as the best-performing model, providing the highest F1-score and improved generalization across all classes, especially the minority "functional needs repair".

## Final Model Selection

After training and tuning all three models — Logistic Regression, Decision Tree, and Random Forest — the Random Forest with optimized hyperparameters was selected as the final model. This decision was based on:

- Highest F1-weighted score

- Better recall on minority class

- Robustness to overfitting

- Ability to model complex patterns in the data

The final model balances predictive power with reasonable interpretability, making it suitable for real-world deployment by NGOs or government bodies for targeting water well maintenance.

## Conclusion

This project successfully demonstrated the use of machine learning to address a critical infrastructure issue in Tanzania — predicting the condition of water wells using historical and operational features.

Starting with data cleaning and preprocessing, through model training and hyperparameter optimization, we built a classification system capable of identifying whether a water point is functional, needs repair, or is non-functional. The best model, a Random Forest, achieved a weighted F1-score of over 0.70, outperforming baseline models in both accuracy and minority class handling.

This solution offers valuable insights to NGOs and government agencies, enabling data-driven decisions on water point maintenance and investments. Future work could include integrating geographic data, temporal monitoring, and real-time updates to improve long-term planning and sustainability.



## 📂 Project Structure

```bash
project/
│
├── data/                   
├── index/              
├── .gitignore/                                   
├── README.md              
└── requirements.txt        
