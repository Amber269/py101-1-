# Chapter 3: numpy & pandas
*by Guoliang Ma, The Chow Institute, 2025*

## What You Will Learn
- More about processing one table.
- Long-and-wide table conversion.
- Divide a table into groups.
- More practice examples.

## 3.2.4 pivot_table and melt

### Understanding `stack` and `unstack`
- `stack` and `unstack` switch the information by changing the index and columns, but the contents remain unchanged.
- In other words, only the way they are presented changes (e.g., from a 6-by-2 table to a 3-by-4 table, but the numbers do not change).

### Interchanging Index/Column and Table Contents
- We introduce methods that interchange index/column and table contents.

### Long and Wide Tables
- A table is **long** when it contains more rows than we want.
- A table is **wide** when it contains more columns than we want.
- We pivot long tables to wider ones.
- We melt wide tables to longer ones.

### Creating a Long Table for Pivoting
```python
import numpy as np
import pandas as pd

data = {
    "score": [90, 91, 92, 80, 81, 82, 60, 61, 62, 50, 51, 52],
    "grades": ["A"] * 3 + ["B"] * 3 + ["C"] * 3 + ["D"] * 3,
    "date": pd.to_datetime(["2020-01-03", "2020-01-04", "2020-01-05"] * 4)
}
df = pd.DataFrame(data)
```

### Converting a Long Table to Wide
- Use `.pivot_table` to convert a long table to wide.
- Example of the desired table:
  ```
  grades   A   B   C   D
  2020-01-03  90  80  60  50
  2020-01-04  91  81  61  51
  2020-01-05  92  82  62  52
  ```

```python
pivoted = df.pivot_table(index="date", columns="grades", values="score")
print(pivoted)
```

### In-Class Exercise 1
- Pivot the following table:
```python
data = {
    "value": range(13),
    "variable ": ["A"] * 4 + ["B"] * 3 + ["C"] * 3 + ["D"] * 3,
    "date": pd.to_datetime(["2020-01-03"] * 5 + ["2020-01-04"] * 4 + ["2020-01-05"] * 4)
}
df = pd.DataFrame(data)
```

- Read in the `MallSales.csv` data and perform the following tasks:
  1. Pivot the table to compute the sum of Sales by Year and Category.
     - What issues do you encounter?
  2. Pivot the table to compute the average Sales by Year.
  3. Pivot the table to see the average rating by Product.
     - To convert a column into string, use `.str`.
     - To remove the last character, use `rstrip`.
  4. Pivot the table to show both sum and mean of Sales by Year and Category.
  5. Nest Product under Category and redo question 4.

### Using `aggfunc` Parameter
- The `.pivot_table` method can summarize information for each group using the `aggfunc` parameter.
```python
pivoted = df.pivot_table(index="date", columns="variable ", values="value", aggfunc="sum")
print(pivoted)
```

### Converting a Wide Table to Long
- Use `.melt` to convert a wide table to long.
- When a DataFrame contains too many columns, it's considered "messy" because useful information becomes harder to find.
- Example:
```python
messydata1 = pd.read_csv("messydata1.csv")
wide_messy = messydata1.melt(id_vars=["Name"])
ids = wide_messy.loc[lambda df: df['variable '] == 'ID', :]
ages = wide_messy.loc[lambda df: df['variable '] == 'Age', :]
genders = wide_messy.loc[lambda df: df['variable '] == 'Gender', :]
```

### Another Example: French Fries
- We are looking for a table like:
  ```
  time  treatment  subject  rep  scale  score
  1     1         3        1    potato  2.9
  1     1         3        1    buttery 0
  1     1         3        1    grassy  0
  ...
  ```
- Melt the table:
```python
ffm = pd.read_csv("french_fries.dat", delimiter='')
melted_ffm = ffm.melt(id_vars=['time', 'treatment ', 'subject', 'rep'], var_name='scale', value_name='score')
print(melted_ffm)
```

### In-Class Exercise 2
- Read in the `cake.dat` data and melt it to a long table:
  ```
  cr  fr  variable  value
  1   1   baker1   7.5
  2   1   baker1   6.1
  1   1   baker2   4.2
  2   1   baker2   3.7
  1   2   baker3   3.8
  ...
  ```

## 3.2.5 groupby
- The last DataFrame operation is to separate the rows into different groups.
- For example, students from grade 1 form a group, students from grade 2 form a group, etc.
- This uses the `.groupby` method.
- Example with the iris data:
```python
irisdata = pd.read_csv("./iris/iris.data", names=["sepal_length", "sepal_width", "petal_length", "petal_width", "type"], header=None)
iris_group = irisdata.groupby("type")
print(iris_group.size())
```

- Note that these groups don't mean we have multiple new DataFrames. It simply records which rows are in which group.
- Many methods can be applied to each group. Try them out and summarize your findings.

## 3.2.6 Other Methods
- We won’t introduce these methods in detail, but please read the documents and do the exercises.
- `dropna`
- `drop_duplicates`
- `reset_index`

### In-Class Exercise 3
- Count how many people selected two courses from different channels (online or onsite) from the `course_form.csv` file.
```python
course_form = pd.read_csv("./course_form.csv")
counts = course_form.groupby(["class", "form"]).count()
print(counts)
```

### In-Class Exercise 4
- Read in the `animal.csv` data and perform the following tasks:
  1. Find the fastest animal in each class using `.first`.
  2. Find the fastest and the second fastest animal in each class using `.nth`.
  3. Find the index of the fastest animal in each class using `.idxmax`.
  4. Get the bird group using the `get_group` method.
  5. Read in the `race.csv` file containing information of a competition. The column `id` shows the athletes' id and the column `time` records their times spent for several tries. Find the average time for each athlete.

### In-Class Exercise 5
- `class1.xlsx` and `class2.csv` contain information on the average scores from two classes: both language and math.
- Reorganize the information in the two tables so that we can compare the language scores and math scores.
- Make two DataFrames to store the language scores of the two classes and the math scores of the two classes.
- Your new tables should be like these:
  - **DataFrame: language**
    ```
    Year  Class1  Class2
    2022  ...     ...
    2023  ...     ...
    2024  ...     ...
    ```
  - **DataFrame: math**
    ```
    Year  Class1  Class2
    2022  ...     ...
    2023  ...     ...
    2024  ...     ...
    ```

## Example I: GDP from stats.gov.cn
- The Bureau of Statistics of our country provides rich information about the country’s economy, especially macroeconomic balances.
- For example, we can easily find GDP data by province and by sector.
- Previously, we have seen the consumption and income data. However, the data we used then were already processed. Now let’s turn to the raw data.

### In-Class Exercise 6
- We will need four files: `consumption.csv`, `cpi.csv`, `gpd.csv`, and `population.csv`.
  1. Read in these data and make necessary changes so we can have one DataFrame with all the information.
     - Hint: when setting new columns, you need to make sure that the order is correct.
  2. Compute the total savings of each province in each year and store the DataFrame in the “long” format.
     - Hint: you can also store them with the “wide” format but it would cause trouble.
  3. Adjust the total savings of each province by the price index. We call the adjusted savings “real savings.”
  4. What’s the average real savings of each province across the years?
  5. Which province saves the most on average?

## Example II: Returns of Individual Stocks
- ⚠This exercise is more difficult!
- The file `stock_utf.csv` contains daily information of 10 listed stocks on the Chinese stock market from 1990 to 2000.
- The data is a subsample of the whole market but can serve as an example.
- The file contains 75 variables (columns) but we cannot use all of them.

### In-Class Exercise 7
  1. What are the variables?
  2. Focus on `PrevClPr` and `Clpr`. Create a new DataFrame to only include relevant columns.
  3. Create returns from the new DataFrame. It’s defined as `Clpr / PrevClPr`.
  4. Compute the cumulative return every 5 days for each stock.

## Example III: Market Return
- The file `Chinese_market.csv` contains all Chinese listed firms but due to privacy concerns, we have added tremendous noise to the original data.
- It contains three columns: `stkcd` (stock code), `trdmnt` (trading month), `mclsprc` (closing price), and `msmvttl` (total market values).
- Compute the weighted (by total market value) average of closing price of all the firms in each month. This weighted average shows us how the stock market performs as a large portfolio.

## Example IV: Index Return
- Use `Chinese_market.csv` again. This time, we have another data `FT50.xlsx`, which contains the composite of Financial Times 50 index.
- Take a subset of the FT50 composites from `Chinese_market` and compute the weighted average of these firms.

## Example V: Portfolio Returns
- Compute the most complex returns: the portfolio returns.
- First, divide the stock universe into ten parts according to the log of the market value of stocks.
- The largest 10% of stocks are put together and we compute the simple average return of them (called a portfolio and portfolio returns).
- The following largest 10% to 20% stocks are put together and we compute their simple average returns, etc.
- Construct ten portfolios according to the market value in each month and compute the simple average of price of each portfolio.
- Find the gross return of each stock in each month.
- Construct ten portfolios according to the log of market value in each month and compute the simple average of return of each portfolio.
- Find the average return of each portfolio series.