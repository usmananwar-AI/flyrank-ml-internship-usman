Markdown
 The Prompt Ladder: Predicting Content Drop-Off

 Baseline (Version 0)
**The Prompt:** Write Python code to predict user churn from search data.
**The Output (Excerpt):** 
```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
model = RandomForestClassifier()
model.fit(X_train, y_train)
predictions = model.predict(X_test)
print("Accuracy:", accuracy_score(y_test, predictions))
Notes:

What changed: N/A (Baseline).

What improved: N/A.

What still failed: It generated completely generic boilerplate. It assumes standard accuracy is the goal, uses placeholder X and y variables, and outputs a single print statement rather than actionable data.

What I will try next: Add context about the specific telemetry data I am using.

Version 1 (+ Context)
The Prompt: Write Python code to predict user churn from search data. Context: I am analyzing historical search telemetry specifically focusing on query_frequency and dwell_time_sec to predict a binary content_drop_off_risk.
The Output (Excerpt):

Python
# Assuming df has columns: 'query_frequency', 'dwell_time_sec', 'content_drop_off_risk'
features = df[['query_frequency', 'dwell_time_sec']]
target = df['content_drop_off_risk']
model.fit(features, target)
Notes:

What changed in the prompt: Added real context (telemetry feature names and target variable).

What improved in the output: The generated code directly references my actual dataframe columns, eliminating the need for me to rename boilerplate variables.

What still failed: It is still optimizing for standard accuracy, which is useless for my imbalanced proactive retention task.

What I will try next: Define the specific success metric (PR-AUC).

Version 2 (+ Clear Goal)
The Prompt: Write Python code to predict user churn from search data. Context: I am analyzing historical search telemetry specifically focusing on query_frequency and dwell_time_sec to predict a binary content_drop_off_risk. Goal: Optimize for Precision-Recall AUC (PR-AUC) rather than standard accuracy to catch expensive false negatives.
The Output (Excerpt):

Python
from sklearn.metrics import average_precision_score

probs = model.predict_proba(X_test)[:, 1]
pr_auc = average_precision_score(y_test, probs)
print("PR-AUC Score:", pr_auc)
Notes:

What changed in the prompt: Added a clear mathematical goal (Optimize for PR-AUC).

What improved in the output: It switched from model.predict() to model.predict_proba() and correctly imported and applied average_precision_score. The evaluation now matches the ML framing.

What still failed: It just prints a score to the console. I need this pipeline to actually hand a prioritized list to an engineering team.

What I will try next: Specify the output format.

Version 3 (+ Specified Output Format)
The Prompt: Write Python code to predict user churn from search data. Context: I am analyzing historical search telemetry focusing on query_frequency and dwell_time_sec to predict a binary content_drop_off_risk. Goal: Optimize for Precision-Recall AUC (PR-AUC). Output format: Return a prioritized pandas dataframe ranking the top 50 highest-risk user sessions (session_id).
The Output (Excerpt):

Python
results_df = pd.DataFrame({'session_id': test_session_ids, 'risk_score': probs})
ranked_df = results_df.sort_values(by='risk_score', ascending=False).head(50)
print("Top 50 At-Risk Sessions:")
print(ranked_df)
Notes:

What changed in the prompt: Added a specified output format (ranked pandas dataframe of top 50 sessions).

What improved in the output: The script transforms the raw probabilities into a ranked, usable dataframe that maps directly back to the unit of analysis (session_id). This solves the actual business problem.

What still failed: The code is bloated with basic exploratory data analysis (EDA) steps and excessive comments explaining what pandas is. It is too messy for a portfolio.

What I will try next: Add a strict length and style constraint.

Version 4 (+ Constraints) — The Step Backwards
The Prompt: Write Python code to predict user churn from search data. Context: I am analyzing historical search telemetry focusing on query_frequency and dwell_time_sec to predict a binary content_drop_off_risk. Goal: Optimize for Precision-Recall AUC (PR-AUC). Output format: Return a prioritized pandas dataframe ranking the top 50 highest-risk user sessions (session_id). Constraint: Keep the code under 10 lines, use zero comments, and chain pandas methods.
The Output (Excerpt):

Python
model.fit(X_train, y_train); print(pd.DataFrame({'session_id': s_ids, 'risk': model.predict_proba(X_test)[:,1]}).sort_values('risk', ascending=False).head(50))
Notes:

What changed in the prompt: Added a strict constraint (under 10 lines, no comments, chained methods).

What improved in the output: It strictly obeyed the length limit.

What still failed: This made it worse. The output became an unreadable, tightly packed single block of code that is impossible to debug, audit, or use in a production environment.

What I will try next: Remove the arbitrary line limit and replace it with a professional quality criterion.

Version 5 (+ Quality Criteria)
The Prompt: Write Python code to predict user churn from search data. Context: I am analyzing historical search telemetry focusing on query_frequency and dwell_time_sec to predict a binary content_drop_off_risk. Goal: Optimize for Precision-Recall AUC (PR-AUC). Output format: Return a prioritized pandas dataframe ranking the top 50 highest-risk user sessions (session_id). Quality criteria: Write production-ready, modular Python functions with type hinting, omitting all basic EDA or print statements.
The Output (Excerpt):

Python
import pandas as pd
from sklearn.ensemble import RandomForestClassifier
from typing import Tuple

def train_and_rank_sessions(
    df: pd.DataFrame, 
    feature_cols: list[str], 
    target_col: str, 
    session_col: str,
    top_n: int = 50
) -> pd.DataFrame:
    
    X = df[feature_cols]
    y = df[target_col]
    
    model = RandomForestClassifier(random_state=42)
    model.fit(X, y)
    
    risk_scores = model.predict_proba(X)[:, 1]
    
    ranked_sessions = pd.DataFrame({
        session_col: df[session_col],
        'drop_off_probability': risk_scores
    }).sort_values(by='drop_off_probability', ascending=False)
    
    return ranked_sessions.head(top_n)
Notes:

What changed in the prompt: Replaced the line constraint with a quality criteria (production-ready, modular, type hinting).

What improved in the output: The code is now structured as a clean, reusable function with type hints. It is highly readable, skips the beginner fluff, and is ready to be committed directly to a portfolio repository.

What still failed: Nothing major. It satisfies all technical and structural requirements.

What I will try next: N/A.

Final Reusable Prompt
Write Python code to predict user churn from search data.
Context: I am analyzing historical search telemetry to predict a binary target column representing drop-off risk.
Goal: Optimize for Precision-Recall AUC (PR-AUC) to catch expensive false negatives.
Output format: A prioritized pandas dataframe ranking the highest-risk units (e.g., sessions, users) based on predicted probability.
Quality criteria: Write production-ready, modular Python functions with type hinting. Omit all basic EDA, print statements, and beginner tutorials. Focus exclusively on the modeling and ranking logic.
