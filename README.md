Import necessary libraries
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.impute import SimpleImputer

Load the dataset
def load_data(file_path):
    try:
        data = pd.read_csv(file_path)
        return data
    except Exception as e:
        print("Error loading data:", str(e))

Preprocess data
def preprocess_data(data):
    try:
        # Drop missing values
        data.dropna(inplace=True)
        
        # Handle categorical variables
        categorical_cols = data.select_dtypes(include=['object']).columns
        data[categorical_cols] = data[categorical_cols].apply(lambda x: pd.Categorical(x))
        
        # One-hot encode categorical variables
        data = pd.get_dummies(data, columns=categorical_cols)
        return data
  except Exception as e:
        print("Error preprocessing data:", str(e))

Split data into training and testing sets
def split_data(data):
    try:
        X = data.drop('target', axis=1)
        y = data['target']
        X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
        return X_train, X_test, y_train, y_test
    except Exception as e:
        print("Error splitting data:", str(e))

Transform data
def transform_data(X_train, X_test):
    try:
        # Impute missing values
        imputer = SimpleImputer(strategy='mean')
        X_train_imputed = imputer.fit_transform(X_train)
        X_test_imputed = imputer.transform(X_test)
        
        # Scale data
        scaler = StandardScaler()
        X_train_scaled = scaler.fit_transform(X_train_imputed)
        X_test_scaled = scaler.transform(X_test_imputed)
        return X_train_scaled, X_test_scaled
    except Exception as e:
        print("Error transforming data:", str(e))

Load transformed data into a new DataFrame
def load_transformed_data(X_train_scaled, y_train):
    try:
        transformed_data = pd.DataFrame(X_train_scaled, columns=X_train.columns)
        transformed_data['target'] = y_train
        return transformed_data
    except Exception as e:
        print("Error loading transformed data:", str(e))

Main function
def main():
    file_path = 'your_data.csv'  # Replace with your file path
    data = load_data(file_path)
    data = preprocess_data(data)
    X_train, X_test, y_train, y_test = split_data(data)
    X_train_scaled, X_test_scaled = transform_data(X_train, X_test)
    transformed_data = load_transformed_data(X_train_scaled, y_train)
    transformed_data.to_csv('transformed_data.csv', index=False)

if _name_ == "_main_":
    main()

