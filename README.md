# Exp - 2 Netflix Shows & Movies

### Name : RASINDHAN R
### Reg No : 212224230222

## Aim

To analyze Netflix dataset and compare movies vs TV shows, top producing countries, and release year trends.

## Procedure / Algorithm

```
1) Load dataset (netflix_titles.csv).
2) Count movies vs TV shows. 
3) Group by country → top contributors.
4) Create pivot table (release year vs type).
5) Visualize with bar & line charts.

```
## Program and Ouptut

```py
import pandas as pd
```

### 1. Load dataset directly from GitHub
```PY
url = "https://raw.githubusercontent.com/allenkong221/netflix-titles-dataset/main/netflix_titles.csv"
df = pd.read_csv(url)

# Basic inspection
print("Shape:", df.shape)
print("Columns:", df.columns, "\n")
print("Type counts:\n", df['type'].value_counts(), "\n")
```

<img width="1063" height="405" alt="image" src="https://github.com/user-attachments/assets/f7bd2f07-31d3-420f-be0d-a801171a8a02" />


### 2. Clean 'date_added' and extract year/month and Movies vs TV Shows
```py
df['date_added'] = df['date_added'].str.strip() # remove extra spaces
df['date_added'] = pd.to_datetime(df['date_added'], errors='coerce') # invalid -> NaT
df['year_added'] = df['date_added'].dt.year
df['month_added'] = df['date_added'].dt.month_name()
count_by_type = df.groupby('type')['title'].count()
print("Count by Type:\n", count_by_type, "\n")

```

<img width="880" height="340" alt="image" src="https://github.com/user-attachments/assets/5885cfa0-2d22-4d93-8c72-3b0a08c200eb" />


### 4. Country vs Type Pivot Table
```py
pivot_country_type = df.pivot_table(
index='country',
columns='type',
values='title',
aggfunc='count',
fill_value=0
)

# Add "Total" column and find max country
pivot_country_type['Total'] = pivot_country_type.sum(axis=1)
max_country = pivot_country_type['Total'].idxmax() # country with most titles
max_count = pivot_country_type['Total'].max() # number of titles
print("Pivot Table (Country vs Type):\n", pivot_country_type.head(), "\n")
print(f"Largest Overall: {max_country} with {max_count} titles\n")
```


<img width="1174" height="558" alt="image" src="https://github.com/user-attachments/assets/0bc0a957-47f2-48d2-ad10-a550ac4391ea" />


### 5. Top 5 Directors
```py
top_directors = df['director'].value_counts().head(5)
print("Top 5 Directors:\n", top_directors, "\n")
```

<img width="789" height="252" alt="image" src="https://github.com/user-attachments/assets/71f04f64-467b-4cc8-bf29-62ad07340c6b" />


### 6. Yearly Trend of Additions (Movies vs TV Shows)
```py
trend = df.groupby(['year_added', 'type']).size().unstack(fill_value=0)
print("Yearly Trend by Type:\n", trend.head(), "\n")
```


<img width="1082" height="260" alt="image" src="https://github.com/user-attachments/assets/08b03771-5b3f-40cf-9f45-e98c7e2f1c1f" />


### 7. Expand Genres
```py
df_genre = (
df[['show_id','listed_in']]
.dropna()
.assign(listed_in=df['listed_in'].str.split(', '))
.explode('listed_in')
)

# Merge expanded genres back
df_expanded = df.merge(df_genre, on='show_id', how='left')

# Check merged columns
print("Columns after merge:\n", df_expanded.columns)

# Show sample of expanded genres
print("\nExpanded Genre Sample:\n",
df_expanded[['title','listed_in_y']].head())

# Top 5 Genres (extra useful for insights)
top_genres = df_expanded['listed_in_y'].value_counts().head(5)
print("\nTop 5 Genres:\n", top_genres, "\n")

```

<img width="1303" height="833" alt="image" src="https://github.com/user-attachments/assets/9e09207d-ba50-48cb-aa85-d618333f3956" />








## Result
Helps Netflix in content planning & investments.
