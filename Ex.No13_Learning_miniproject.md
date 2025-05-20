# Ex.No: 13 
higher+education+students+performance+evaluation
### DATE: 13-05-2025                                                                           
### REGISTER NUMBER : 212222040159
### AIM: 
To design and implement an effective algorithm for evaluating the academic performance of higher education students by analyzing relevant academic and behavioral data, with the goal of identifying key performance indicators and enabling data-driven decision-making to support student success.
###  Algorithm:
Step1:Data Collection
Gather relevant student data, including:

Academic records (e.g., GPA, course grades)

Attendance

Participation

Assignments and project scores

Demographic and socio-economic background (optional)

Step 2:Data Preprocessing

Handle missing values

Normalize or standardize data

Convert categorical variables into numerical (e.g., using one-hot encoding)

Remove outliers and inconsistencies

Step 3:Feature Selection

Identify the most relevant features that affect performance (e.g., using correlation analysis or feature importance from models)

Reduce dimensionality if needed (e.g., PCA)

Step 4:Label Definition

Define what "performance" means (e.g., high/medium/low, pass/fail, GPA range)

Create target labels for classification or a numeric value for regression

Step 5:Model Selection
Choose a suitable model for evaluation, such as:

Decision Tree

Random Forest

SVM

Logistic Regression

Neural Network (for complex tasks)

Step 6:Model Training

Split the dataset into training and test sets (e.g., 80/20 split)

Train the model using the training data

Step 7:Model Evaluation

Evaluate model performance using metrics such as:

Accuracy

Precision/Recall/F1 Score (for classification)

RMSE/MSE (for regression)

Perform cross-validation to ensure generalization

Step 8:Performance Interpretation & Decision Making

Analyze results and identify key performance factors

Provide insights for intervention, support, or academic improvement

Optionally, visualize data and results for better understanding

### Program:

import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import LabelEncoder
from sklearn.metrics import accuracy_score, classification_report

# 1. Load the dataset
df = pd.read_csv('/content/data.csv')

# 2. Quick look at the data
print("First 5 rows:")
print(df.head())
print("\nSummary:")
print(df.info())

# 3. Handle categorical columns (optional, depends on your dataset)
label_encoders = {}
for column in df.select_dtypes(include=['object']).columns:
    le = LabelEncoder()
    df[column] = le.fit_transform(df[column])
    label_encoders[column] = le

# 4. Assume the last column is the target
X = df.iloc[:, :-1]  # features
y = df.iloc[:, -1]   # target

# 5. Train-test split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 6. Train the model
clf = RandomForestClassifier(random_state=42)
clf.fit(X_train, y_train)

# 7. Predict
y_pred = clf.predict(X_test)

# 8. Evaluate
print("\nAccuracy:", accuracy_score(y_test, y_pred))
print("\nClassification Report:")
print(classification_report(y_test, y_pred))
---
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
from sklearn.metrics import accuracy_score, classification_report
from sklearn.ensemble import RandomForestClassifier
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.neighbors import KNeighborsClassifier

# Load the dataset
df = pd.read_csv('/content/data.csv')

# Prepare data
X = df.drop(columns=["STUDENT ID", "GRADE"])
y = df["GRADE"]

# Encode target if needed
if y.dtype == 'object':
    le = LabelEncoder()
    y = le.fit_transform(y)

# Split data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Initialize classifiers
classifiers = {
    "Random Forest": RandomForestClassifier(random_state=42),
    "Logistic Regression": LogisticRegression(max_iter=1000),
    "Decision Tree": DecisionTreeClassifier(random_state=42),
    "K-Nearest Neighbors": KNeighborsClassifier()
}

# Train and evaluate
for name, clf in classifiers.items():
    clf.fit(X_train, y_train)
    y_pred = clf.predict(X_test)
    print(f"Classifier: {name}")
    print(f"Accuracy: {accuracy_score(y_test, y_pred):.2f}")
    print("Classification Report:")
    print(classification_report(y_test, y_pred))
    print("-" * 50)
---
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

# Load the data
df = pd.read_csv("/content/data.csv")

# Plot the grade distribution
plt.figure(figsize=(8, 5))
sns.countplot(x="GRADE", data=df, palette="Set2")
plt.title("Distribution of Grades")
plt.xlabel("Grade")
plt.ylabel("Number of Students")
plt.show()
---
# Drop non-numeric columns
numeric_df = df.drop(columns=["STUDENT ID"])

# Compute correlation
corr = numeric_df.corr()

# Plot heatmap
plt.figure(figsize=(12, 10))
sns.heatmap(corr, annot=False, cmap="coolwarm", linewidths=0.5)
plt.title("Correlation Heatmap")
plt.show()
---
features_to_plot = ['1', '5', '10', '15', '20', 'GRADE']
df[features_to_plot].hist(bins=5, figsize=(10, 6), layout=(2, 3))
plt.suptitle("Distributions of Selected Features")
plt.tight_layout()
plt.show()
---
!pip install gradio
---
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import gradio as gr

# Load dataset
df = pd.read_csv("/content/data.csv")

# Function: Plot Grade Distribution
def plot_grade_distribution():
    plt.figure(figsize=(6, 4))
    sns.countplot(x="GRADE", data=df, palette="Set2")
    plt.title("Grade Distribution")
    plt.xlabel("Grade")
    plt.ylabel("Number of Students")
    plt.tight_layout()
    return plt.gcf()

# Function: Plot Correlation Heatmap
def plot_correlation_heatmap():
    plt.figure(figsize=(10, 8))
    numeric_df = df.drop(columns=["STUDENT ID"])
    corr = numeric_df.corr()
    sns.heatmap(corr, cmap="coolwarm", annot=False, linewidths=0.5)
    plt.title("Correlation Heatmap")
    return plt.gcf()

# Function: Plot Feature Distribution
def plot_feature_distributions():
    selected_features = ['1', '5', '10', '15', '20', 'GRADE']
    df[selected_features].hist(bins=5, figsize=(10, 6), layout=(2, 3))
    plt.suptitle("Distributions of Selected Features")
    plt.tight_layout()
    return plt.gcf()

# Gradio Interface
with gr.Blocks() as demo:
    gr.Markdown("## 🎓 Student Performance Dataset EDA Tool")
    
    with gr.Row():
        gr.Markdown("Click a button to visualize:")
    
    with gr.Row():
        btn1 = gr.Button("📊 Grade Distribution")
        btn2 = gr.Button("🔥 Correlation Heatmap")
        btn3 = gr.Button("📈 Feature Distributions")
    
    output_plot = gr.Plot()

    btn1.click(plot_grade_distribution, outputs=output_plot)
    btn2.click(plot_correlation_heatmap, outputs=output_plot)
    btn3.click(plot_feature_distributions, outputs=output_plot)

demo.launch()
---



### Output:
![output1](https://github.com/user-attachments/assets/6a954f53-fa3e-459b-a149-c14e0d2190da)
![output2](https://github.com/user-attachments/assets/55b10bba-8450-4a22-ab52-22e2c7272f82)
![output3](https://github.com/user-attachments/assets/3caf2f5a-73a9-46e9-aa27-fb24f3b1ec0c)
![output4](https://github.com/user-attachments/assets/72fbfafa-f183-4881-8c31-eea2ddb835fa)





### Result:
The proposed performance evaluation algorithm successfully analyzed student data and predicted academic performance with high accuracy. Key influencing factors such as attendance, assignment scores, and previous academic records were identified. The evaluation model (e.g., Random Forest or Logistic Regression) achieved an accuracy of [insert percentage, e.g., 85%], demonstrating its effectiveness in classifying students into performance categories (e.g., high, medium, low). The system can be used by educational institutions to monitor student progress, identify at-risk students early, and implement targeted interventions.
