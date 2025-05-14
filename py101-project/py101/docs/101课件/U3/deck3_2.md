# Programming for AI (Python): Chapter 3 - numpy & pandas
*Guoliang Ma, The Chow Institute, 2025*

## What You Will Learn
- **Data organization**
- **pandas.DataFrame = 2D numpy array + row info + col info**
- Fetching --- taking out rows of a table
- Fetching --- taking out columns of a table
- Summarizing information in a table
- Sort to prioritize rows
- Aggregation to see a common trend
- MultiIndexed DataFrame
- The `loc` and `iloc` properties, making new columns

## Prelude -- Working with Data
We now officially start our journey to work with data sets, after learning so much about Python basics. A data set is where data are stored. Because most data sets appear as a table, we use data sets and data tables interchangeably. The `pandas` module is a module that specializes in analyzing data tables.

### Data Tables: Carriers of Information
Data tables are the most important carriers of information. But the information doesn't always speak for itself. So, we need to analyze the data to reveal the information. By looking at one data table, you’ll be able to:
1. Explain how a data table is organized (what the rows and columns of that table represent).
2. Apply some basic manipulations (get the information from the table).

These manipulations help us look at the data from different perspectives by changing the format of the table. They help us find the specific character of a specific person by locating it. These also help us divide the data table into smaller ones called groups and summarize information in each group.

### Data Organization in Economics
In economics, we often need to work with data. From data, we can gain insights about how the economic world operates. We can also verify our economic theory with data. Such data include the GDP of a country, the consumption details of a person, the price of an item, etc. As computing power improves, economic data is getting larger and larger. It's now common to see data sets of several GB or even TB.

In computers, we often store the data in a table. A table is mostly like a 2D matrix. It contains rows (each horizontal element) and columns (each vertical element). For example:

- Each row represents an observational unit (a person, a country, etc.: an entity).
- Each column exhibits a perspective or character of the rows (the persons' height, the countries' GDP, etc.).
- The top row is reserved for the name of the columns and is called the header. It's not part of the "contents" of the table.
- The first column is reserved for the index (name) of the rows. It's not part of the "contents."

## 3.1 Just One Column
Pandas store sequence data pretty similar to `numpy`: a 1-D sequence. But in `pandas`, this sequence is called a `Series`.

- One important requirement of a `Series` is that its elements must be of the same type.
- An important distinction of a `Series` is that its elements have names, called `index`. We also say, “The numbers in a `Series` are indexed.”

## 3.2 Processing One Table
The type we use to store a data table is called `pandas.DataFrame`.

- It's basically a matrix with `index` and `columns`:
  - `index` contains the name of each row.
  - `columns` contains the name of each column.

For example, in the following table, the boldfaced numbers on the leftmost side are the index. The boldfaced letters on top are the columns.

```python
import pandas as pd
import numpy as np

df1 = pd.DataFrame(
    np.random.randn(6, 4),  # N(0,1)
    index=range(6),
    columns=list("ABCD")
)
```

### 3.2.1 Rows and Columns
We can look up (take out) the rows and columns with their names. But the indexing differs:

- Although they look similar, rows and columns are completely different. We can check their types. The type `pandas.core.series.Series` means that `col1` is a `Series`.
- A `Series` is a 1-D object to store information of one aspect.
- `pandas.DataFrame` is a combination of multiple `Series`.

```python
row1 = df1[0:1]
col1 = df1['A']
```

The `DataFrame` provides us with structured information. For example, you’ll see that `1` and `2` do not appear as integers. Why?

We can get the analytical information of each column with the `.describe` method and the storage information with the `.dtypes` attribute.

```python
df2 = pd.DataFrame(
    [[1, 2], [1.1, 2.1]],
    index=range(2),
    columns=['col1', 'col2']
)
```

### 3.2.2 Sort and Aggregation
We can look at a table from different angles. Although the information will not change, we can highlight the information in which we are more interested.

- The first method of highlighting is to put the most important information on top of the table.
- This is done with the `.sort_values` or the `.sort_index` methods.

#### Example for Sorting

```python
race_score = pd.DataFrame(
    [['Alice', 15.6], ['Bob', 17.3], ['Charlie', 14.5]]
)
race_score.sort_values(1, ascending=True)

race_score.index = [1002, 1001, 1003]
race_score.sort_index()
```

When we want to take a bird-eye view of the table, we summarize information. That is, we do not care about individual data records, but want to know how the overall data look like.

- Such summarizing is done by the `.agg` method.

#### Example for Aggregation

```python
student_name = ['Alice', 'Bob', 'Charlie', 'David', 'Eva']
language_score = [99, 100, 35, 60, 71]
math_score = [25, 89, 36, 40, 91]

score = pd.DataFrame(
    np.array([language_score, math_score]).T,
    index=student_name,
    columns=['language', 'math']
)

score.agg("mean", axis=0)
score.agg("sum", axis=1)
```

### 3.2 Processing One Table
Pandas provide a much simpler API for reading the CSV file: the `pd.read_csv` function.

```python
irisdata = pd.read_csv("./iris/iris.data", header=None)
```

#### In-class Exercise 1
1. Read in the data and take a quick look. What are the column names?
2. Change column names to "sepal_length", "sepal_width", "petal_length", "petal_width", and "type".
3. Remove the last column "type" (hint: there's a method called `drop`).
4. Compute (aggregate) the mean and standard deviation of each of the first four columns.
5. Subtract the mean from each corresponding column and then divide by its standard deviation. The new data frame should be known as "irisdata_normed".
6. Check the mean and standard deviation of `irisdata_normed`.
7. Write the data to a .csv file (`.to_csv`).

### 3.2.2 Stack and Unstack
Section 3.2.1 is about changing the way the data contents appear. But we kept the rows as rows and columns as columns (there was no transposition or exchange between rows and columns).

- The second way of highlighting the data in which we are interested is by swapping the rows and columns.
- This is also an important way to organize the data without changing the values of the contents (compared to aggregation).
- This can be done with the `.stack` and `.unstack` methods.

The index in a data frame shows us the information on the identity of a person, but it is usual that we have multiple levels of identity. For example, if in class 1, grade 1, there is an Alice, it is equally possible that in class 2, grade 3, there is another Alice. So, we need multiple indices to help identify one person.

Let's assume we select a student from each class and ask about their height and weight.

```python
arrays = [
    ["first", "first", "second", "second", "third", "third", "fourth", "fourth"],
    ["one", "two", "one", "two", "one", "two", "one", "two"]
]

height_weight = np.array([
    [173, 176, 185, 167, 165, 193, 156, 163],
    [130, 190, 180, 170, 170, 200, 100, 105]
]).T

myindex = pd.MultiIndex.from_arrays(arrays, names=["grade", "class"])
```

#### Example for Stack and Unstack

```python
student_info = pd.DataFrame(height_weight, index=myindex, columns=["height", "weight"])
student_info.stack()
student_info.stack().unstack([1, 2])
```

### 3.2.3 `.loc` and `.iloc`
The easiest way to take out rows and columns is by the square brackets. However, these methods have limitations:
- We have to use different indexing for rows and columns.
- We cannot select a list of rows.
- We cannot slice columns.
- Multi-step selection (advanced selection) is prone to errors.

The standard (or error-proofing) way to select columns and rows is by the `.loc` (label-of-column) and `.iloc` (integer label-of-column) properties.

#### Example for `.loc` and `.iloc`

```python
student_info.loc[('second', 'one'), 'height']
student_info.iloc[2, 0]
student_info.loc[lambda df: (180 > df['height']) & (df['height'] > 160), :]
```

### Example of Income Data
#### In-class Exercise 2
1. The files `income.csv` and `consumption.csv` contain income per capita (means “per person”) and consumption per capita by province, along with the population.
2. Please make new columns to find the total income and total consumption of each province in each year and then save the files with new names (`income_total.csv` and `consumption_total.csv`). What are the units?
3. How do you compute the savings ratio (savings defined as the amount that is not used for consumption)?

### Example of Production Function Data
#### In-class Exercise 3
Another useful operation with one data set is lead/lag. Let’s consider the production function:
\[ Y_t = A_t K_t^\alpha L_t^{1-\alpha} \]

- Here \( K_t \) is the capital that is used to generate the output \( Y_t \). In accounting, you have learned that firms will disclose financial statements at the end of the fiscal year. The output is the total output over the year, the labor is the number of employees at the end of the year. The capital is also at the end of the year. This generates misalignment between the input and output. We use the `shift` function to align them.
- In terms of labor, since we are not sure which one gives the most accurate measure, we typically take the average of labor of the current year and the previous year.

The file `cdprod.xlsx` contains the capital, labor, and output information collected from a fake capital-intensive firm from 2000 to 2021. But the output of 2000 is missing.

1. Please write a function to find the return-to-scale on capital without correcting for the alignment.
   - Hint: you can write a for loop to see which value of \( \alpha \) produces the output that is closest to the observed.
2. Please correctly align capital and labor and then redo step 1.

### Example of Finance Data
#### In-class Exercise 4
In the theory of asset pricing, an important idea is called momentum, which gives clues on how the stock returns in the future using the past return information. By definition, momentum is:
\[ m_t = \sum_{j=1}^{12} r_{t-j} \]

- Here \( r_t \) is the return at time \( t \).

1. The file `return1.csv` contains information about the returns of one firm. Please compute its momentum when data are available.
   - Hint: you probably will find it useful to look at the `rolling` method.

#### In-class Exercise 4 (Cont’d)
2. The file `return4.csv` contains return information of four firms.
   1. Look at the data and find the cross-sectional average returns. What is the average of the cross-sectional average returns?
   2. Please read in the data and check if any necessary changes are needed.
   3. Please compute the momentum of these four firms when data are available.

### Example of Sector GDP by Country
#### In-class Exercise 5
The file `country_sector.xlsx` contains sector-level GDP of four countries. Please find the average per country and the average per sector.