Target Task
Developing a data preprocessing and feature engineering script for the FlyRank ML pipeline, specifically converting raw historical search telemetry into analytical features for predicting content drop-off.

Baseline (Version 0: Naive Prompt)
The Prompt: Write a Python script to clean my search data and make features for an ML model.
The Output (Excerpt):

Python
import pandas as pd
df = pd.read_csv("data.csv")
df = df.dropna()
df['new_feature'] = df['col1'] + df['col2']
# ...
Iteration Note:

What changed: N/A (Baseline).

Why/Output Difference: The AI hallucinated random column names (col1, col2), used generic mathematical operations, and provided no structured way to reuse the code. It is completely unusable for the actual FlyRank dataset.

Version 1 (+ Role Assignment)
The Prompt: Act as a Senior Machine Learning Engineer. Write a Python script to clean my search data and make features for an ML model.
The Output (Excerpt):

Python
import pandas as pd
from sklearn.preprocessing import StandardScaler

def preprocess_data(df: pd.DataFrame) -> pd.DataFrame:
    """Cleans search data and generates features."""
    df_cleaned = df.dropna()
    scaler = StandardScaler()
    # ...
Iteration Note:

What changed: Added a specific role assignment ("Act as a Senior Machine Learning Engineer").

Why/Output Difference: The output improved stylistically. The AI introduced a function definition, type hinting (pd.DataFrame), and standard libraries (StandardScaler), making the code look more professional, though it is still guessing the underlying data logic.

Version 2 (+ Context and Motivation)
The Prompt: Act as a Senior Machine Learning Engineer. I am building FlyRank, a system to predict content drop-off using historical search telemetry. Write a Python script to clean my search data and make features for this ML model.
The Output (Excerpt):

Python
def build_telemetry_features(df: pd.DataFrame) -> pd.DataFrame:
    # Assuming columns like 'session_duration', 'search_queries'
    df['query_rate'] = df['search_queries'] / df['session_duration']
    df['is_churned'] = df['days_since_last_search'] > 30
    return df
Iteration Note:

What changed: Added context and motivation (FlyRank, historical search telemetry, predicting drop-off).

Why/Output Difference: The AI stopped guessing random mathematical additions and started generating domain-specific features like query_rate. It understands the why of the task, making the logic highly relevant to proactive retention.

Version 3 (+ Few-Shot Examples)
The Prompt: Act as a Senior Machine Learning Engineer. I am building FlyRank to predict content drop-off using historical search telemetry. Write a Python script to clean my search data and make features.
Input example: session_id=1, query_frequency=5, dwell_time_sec=120
Output feature example: queries_per_minute = 2.5
The Output (Excerpt):

Python
def engineer_features(df: pd.DataFrame) -> pd.DataFrame:
    # Calculate queries per minute based on the provided example
    df['dwell_time_min'] = df['dwell_time_sec'] / 60.0
    df['queries_per_minute'] = df['query_frequency'] / df['dwell_time_min']
    return df
Iteration Note:

What changed: Added few-shot examples (providing specific input/output column mappings and the desired math).

Why/Output Difference: The AI locked onto the exact variable names (query_frequency, dwell_time_sec) and calculated the precise derived metric requested, completely eliminating hallucinations about the dataset schema.

Version 4 (+ Output Structure)
The Prompt: Act as a Senior Machine Learning Engineer. I am building FlyRank to predict content drop-off using historical search telemetry. Write a Python script to clean my search data and make features.
Input example: session_id=1, query_frequency=5, dwell_time_sec=120
Output feature example: queries_per_minute = 2.5
Output Structure: Return ONLY a professional, documented Python class named FlyRankFeatureEngineer with an __init__ method and a transform method. Do not include markdown explanations.
The Output (Excerpt):

Python
import pandas as pd

class FlyRankFeatureEngineer:
    def __init__(self):
        pass

    def transform(self, df: pd.DataFrame) -> pd.DataFrame:
        df = df.copy()
        df['dwell_time_min'] = df['dwell_time_sec'] / 60.0
        df['queries_per_minute'] = df['query_frequency'] / df['dwell_time_min']
        return df
Iteration Note:

What changed: Added strict output structure constraints (Python class, specific methods, zero conversational text).

Why/Output Difference: The AI abandoned loose function definitions and conversational filler. The output is now a clean, modular object-oriented class that can be directly imported into the main pipeline repository.

Version 5 (+ Step Decomposition)
The Prompt: Act as a Senior Machine Learning Engineer. I am building FlyRank to predict content drop-off using historical search telemetry.
Input example: session_id=1, query_frequency=5, dwell_time_sec=120
Output feature example: queries_per_minute = 2.5
Output Structure: Return ONLY a professional Python class named FlyRankFeatureEngineer.
Follow these steps exactly:
Step 1: Check for and drop rows with missing dwell_time_sec.
Step 2: Engineer the queries_per_minute feature.
Step 3: Clip queries_per_minute to a maximum of 10 to handle bot outliers.
The Output (Excerpt):

Python
class FlyRankFeatureEngineer:
    def transform(self, df: pd.DataFrame) -> pd.DataFrame:
        # Step 1: Drop missing
        df = df.dropna(subset=['dwell_time_sec'])
        
        # Step 2: Engineer feature
        df['dwell_time_min'] = df['dwell_time_sec'] / 60.0
        df['queries_per_minute'] = df['query_frequency'] / df['dwell_time_min']
        
        # Step 3: Handle bot outliers
        df['queries_per_minute'] = df['queries_per_minute'].clip(upper=10)
        
        return df
Iteration Note:

What changed: Added step decomposition (breaking the processing logic into three distinct, ordered tasks).

Why/Output Difference: The AI execution became entirely deterministic. Instead of improvising how to handle edge cases, it strictly followed the business logic for outlier capping (bots), making the pipeline robust and production-ready without me having to write the pandas syntax myself.

Cross-Model Comparison: Claude vs. ChatGPT
I ran the final Version 5 prompt through both Claude and ChatGPT to observe execution differences:

Tone & Output Compliance: Claude followed the "Return ONLY a professional Python class" constraint perfectly, outputting a raw code block with zero conversational text. ChatGPT included a brief "Here is your class:" intro and a summary at the end, failing the strict negative constraint.

Accuracy & Structure: Both models accurately implemented the math and pandas logic. However, Claude's code structure was slightly more defensive (using .copy() on the dataframe to prevent SettingWithCopyWarnings), whereas ChatGPT wrote slightly more concise but riskier in-place modifications.

Failure Points: ChatGPT failed the structural constraint. Claude handled the step decomposition flawlessly but was highly literal—meaning if my step instructions contained a logical flaw, Claude would execute it blindly without suggesting an alternative.

Final Reusable Template
Plaintext
Act as a Senior Machine Learning Engineer. 

Context: I am building a project called [PROJECT NAME]. The goal is to [PREDICTION GOAL] using [DATA DESCRIPTION]. 
Write a Python script to clean the data and engineer features.

Input Data Example: [PROVIDE 1 ROW OF RAW DATA]
Target Feature Example: [PROVIDE 1 DESIRED OUTPUT FEATURE]

Output Structure: 
Return ONLY a professional, documented Python class named [CLASS NAME] with an __init__ method and a transform method. Do not include introductory or concluding text.

Execution Steps:
Step 1: [DATA CLEANING REQUIREMENT]
Step 2: [FEATURE ENGINEERING REQUIREMENT]
Step 3: [OUTLIER/EDGE CASE HANDLING]
