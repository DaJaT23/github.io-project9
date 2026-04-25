---
# Do not edit the text between these lines!
layout: default
---

# Rixa and Da'Ja Analysis of Comp 

## Part 1.3: Choosing Your Analysis

1. Idea to analyze with available data: This course should more proactively encourage tutoring because the language of coding is more methodical and mathematical compared to other fields. (you have to think outside of the box when it comes to coding)

2. This idea is more valuable than the others brainstormed because: coding requires a lot more out of the box thinking, you have to be able to think more methodical and rigidly because you can only create certain options. For example, when talking in real life, you're more likely to say more than just yes or no. But in coding, that doesn't work out, you have to use booleans to translate those options which limit you to only True and False. 

## Lets import our data!
```python 
from data_utils import read_csv_rows, get_keys
data_rows: list[dict[str, str]] = read_csv_rows(DATA_FILE_NAME_IZZI)

print(f"Data File Read: {DATA_FILE_NAME_IZZI}")
print(f"{len(data_rows)} rows")
print(f"{len(get_keys(data_rows[0]))} columns")
print(f"Columns names: {get_keys(data_rows[0])}")
```

# How many students went to tutoring?

```python 
data_izzi = read_csv_rows(DATA_FILE_NAME_IZZI)
data_alyssa = read_csv_rows(DATA_FILE_NAME_ALYSSA)

combined_rows = data_izzi + data_alyssa

print(f"Total rows: {len(combined_rows)}")

def filter_by_threshold(data: list[dict[str, str]], column: str, min_value: int) -> list[dict[str, str]]:
    """Filter rows where column >= min_value (ignores empty values)."""
    result: list[dict[str, str]] = []

    for row in data:
        value = row[column]
        if value != "" and int(value) >= min_value:
            result.append(row)

    return result
filtered_rows = filter_by_threshold(combined_rows, "tutoring_effective", 4)

print(f"Students who believe tutoring is effective: {len(filtered_rows)}")
```

# Lets use the head function to view our data

```python
from data_utils import head

data_cols_head: dict[str, list[str]] = head(data_cols, 3)

tabulate(data_cols_head, get_keys(data_cols_head), "html")
```

# Now we can select specifically for tutoring effectivness and ranked difficulty

```python
from data_utils import select

selected_data: dict[str, list[str]] = select(data_cols, ["tutoring_effective", "difficulty"])

tabulate(head(selected_data, 10), get_keys(selected_data), "html")
```

# Now lets make the data readable

```python
from data_utils import columnar

data_cols: dict[str, list[str]] = columnar(data_rows)

print(f"{len(get_keys(data_cols))} columns")
print(f"{len(data_cols['year'])} rows")
print(f"Columns names: {get_keys(data_cols)}")
```

# Lets count how students rated tutoring and how they rated difficulty

```python
from data_utils import count

tutoring_effective_counts: dict[str, int] = count(selected_data["tutoring_effective"])
print(f"tutoring effective counts: {tutoring_effective_counts}")

difficulty_counts: dict[str, int] = count(selected_data["difficulty"])
print(f"difficulty counts: {difficulty_counts}")
```

## Lets clean up the empty data for smooth graphing

```python 
def clean_rows_for_columns(data: list[dict[str, str]], columns: list[str]) -> list[dict[str, str]]:
    """Remove rows where any specified column is empty or 'None'."""
    
    result: list[dict[str, str]] = []

    for row in data:
        valid = True
        for col in columns:
            if row[col] == "" or row[col] == "None":
                valid = False
        if valid:
            result.append(row)

    return result
```
    
```python
data_izzi = read_csv_rows(DATA_FILE_NAME_IZZI)
data_alyssa = read_csv_rows(DATA_FILE_NAME_ALYSSA)

combined_rows = data_izzi + data_alyssa
clean_rows = clean_rows_for_columns(
    combined_rows, 
    ["tutoring_effective", "difficulty", "understanding"]
)

clean_cols = columnar(clean_rows)

plot_data = convert_columns_to_int(
    clean_cols,
    ["tutoring_effective", "difficulty", "understanding"]
)
```

## Heres a scatterpot visualizing the difficulty in comparison to tutoring effectiveness

<img src="/static/imgs/scatterplot.png" width="500">

## Heres a histogram to visualize the same data in range form

<img src="/static/imgs/histogram.png" width="500">

## Lastly, Heres a bargraph to visualize tutoring effectivness for those that attended

<img src="/static/imgs/bargraph.png" width="500">

# Final Results

We decided to analyze the effectiveness of tutoring, and how it related to their percieved difficulty of the course. In our graphs, while there is no positive or negative corralation, a majority of the students who attended tutoring rated it as effective (>=4). Both categories were measured on a scale of 1-7, with 1 being least effective and 7 being the most. We found the scatterplot to be the most inconclusive of our data. The bar graph and the histrogram support the idea that even with tutoring, many found the course to be difficult, contributing to the inconclusive nature of our results. The tutoring category had empty strings which made working with the data difficult. We had to create additional functions to clean the data to obtain only numerical data for the graphs. Despite the missing data, it did not have much impact on the final results because only those who attended tutoring were counted. This has an impact on student, staff, and the institution. For staff it shows the potential change that may need to be implemented. For the institution it could relate to keeping high enrollment rates for the course and a steady funding. It is important to note that tutoring is one of many methods to determine difficulty, because those who attend tutoring are more likely to find the course difficult. Our reccomendations for increading tutoring effectiveness would be to expand the scope of help available and provide an incentive for students to go, as well as potentionally expanding the tutoring hours. 
