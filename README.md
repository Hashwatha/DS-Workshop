## DS-Workshop-Data Cleaning and Data Analysis
# 1. Perform Data cleaning process wherever necessary
```
import pandas as pd
data = pd.read_csv('/content/supermarket.csv')
```
```
duplicates = data.duplicated().sum()
print(f"Duplicate Rows: {duplicates}")
data = data.drop_duplicates()
```
![image](https://github.com/user-attachments/assets/27af5e1d-44f4-4539-9b8e-4fd019e4c612)
```
missing_values = data.isnull().sum()
print(f"Missing Values:\n{missing_values}")
```
![image](https://github.com/user-attachments/assets/26fa32ed-83c4-4cee-a332-9396c60555d1)
```
data['Date'] = pd.to_datetime(data['Date'], format='%m/%d/%Y') 
data['Time'] = pd.to_datetime(data['Time'], format='%H:%M').dt.time 
data_cleaned = data.drop(columns=['Invoice ID']) 
data_cleaned['Branch'] = data_cleaned['Branch'].str.strip()
numerical_cols = ['Unit price', 'Quantity', 'Tax 5%', 'Total', 'cogs', 'gross income', 'Rating']
for col in numerical_cols:
    print(f"{col} Min: {data_cleaned[col].min()}, Max: {data_cleaned[col].max()}")
print(data_cleaned.info())
```
![image](https://github.com/user-attachments/assets/6cf5fa90-7b0d-4177-9771-ed6c891f1208)

## 2. Implement Boxplot method to detect outliers
```
import matplotlib.pyplot as plt
import seaborn as sns
numerical_cols = ['Unit price', 'Quantity', 'Tax 5%', 'Total', 'cogs', 'gross income', 'Rating']
plt.figure(figsize=(15, 10))
for i, col in enumerate(numerical_cols, 1):
    plt.subplot(3, 3, i) 
    sns.boxplot(data=data_cleaned, x=col, color='skyblue') 
    plt.title(f'Boxplot of {col}') 
plt.tight_layout() 
plt.show()
```
![image](https://github.com/user-attachments/assets/3d4c5999-0ea2-4f68-9ccd-d4955214537e)
## 3. Implement IQR method to Remove Outliers 
```
def remove_outliers_iqr(df, columns):
    for col in columns:
        Q1 = df[col].quantile(0.25)  
        Q3 = df[col].quantile(0.75)  
        IQR = Q3 - Q1 
        lower_bound = Q1 - 1.5 * IQR
        upper_bound = Q3 + 1.5 * IQR
        df = df[(df[col] >= lower_bound) & (df[col] <= upper_bound)]
    return df
numerical_cols = ['Unit price', 'Quantity', 'Tax 5%', 'Total', 'cogs', 'gross income', 'Rating']
data_no_outliers = remove_outliers_iqr(data_cleaned, numerical_cols)
print(f"Original Shape: {data_cleaned.shape}")
print(f"Shape After Outlier Removal: {data_no_outliers.shape}")
```
![image](https://github.com/user-attachments/assets/583a9d57-aedb-4439-b84f-e475c1f7624b)
## 4. Implement Count plot method for univariate analysis
```
import seaborn as sns
import matplotlib.pyplot as plt
categorical_cols = ['Branch', 'City', 'Customer type', 'Gender', 'Product line', 'Payment']
plt.figure(figsize=(15, 10))
for i, col in enumerate(categorical_cols, 1):
    plt.subplot(3, 2, i) 
    sns.countplot(data=data_cleaned, x=col, palette='viridis')  
    plt.title(f'Count Plot of {col}') 
    plt.xticks(rotation=45)  
plt.tight_layout()  
plt.show()
```
![image](https://github.com/user-attachments/assets/c5dea939-ac17-41b8-8e08-178111daab30)
## 5.Implement DistPlot method for multivariate analysis
```
import seaborn as sns
import matplotlib.pyplot as plt
plt.figure(figsize=(10, 6))
sns.kdeplot(data=data_cleaned, x='Rating', hue='Branch', fill=True, alpha=0.5, palette='Set2')
plt.title('Distribution of Ratings Across Branches')
plt.xlabel('Rating')
plt.ylabel('Density')
plt.show()
plt.figure(figsize=(10, 6))
sns.histplot(data=data_cleaned, x='Total', hue='Customer type', kde=True, palette='Set1', bins=30, alpha=0.7)
plt.title('Distribution of Total Across Customer Types')
plt.xlabel('Total')
plt.ylabel('Frequency')
plt.show()
```
![image](https://github.com/user-attachments/assets/2ff22561-e609-4034-ba1e-854566eb0f01)
