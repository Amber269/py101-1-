# Programming for AI (Python)
*The Chow Institute, 2025*

## Chapter 4: Matplotlib
*Guoliang Ma*

### What You Will Learn
- A bunch of other types of plots.

### 4.5 Scatter Plot
Scatter plots are useful for visualizing the relationship between two variables. In this section, we will use the **Iris** dataset from `sklearn.datasets` to create a scatter plot.

```python
from sklearn.datasets import load_iris
import matplotlib.pyplot as plt

# Load the Iris dataset
iris = load_iris()
features = iris.data.T

# Print the shape of the features
print(features.shape)

# Create a scatter plot
plt.scatter(features[0], features[1],
            alpha=0.2,
            s=100 * features[3],
            c=iris.target,
            cmap='viridis')

# Label the axes
plt.xlabel(iris.feature_names[0])
plt.ylabel(iris.feature_names[1])

# Show the plot
plt.show()
```

### 4.5 Histogram
Histograms are used to visualize the distribution of a single variable. Here, we will create a histogram for the "Fe" feature in the **Glass** dataset.

```python
import matplotlib.pyplot as plt

# Assuming 'glass' is a DataFrame containing the Glass dataset
plt.hist(glass["Fe"], bins=30,
         alpha=0.5,
         histtype='stepfilled', color='steelblue',
         edgecolor='none')

# Show the plot
plt.show()
```

### 4.5 Pair Plots
Pair plots are a great way to visualize the pairwise relationships between multiple variables in a dataset. We will use the **Seaborn** library to create a pair plot for the **Iris** dataset.

```python
import seaborn as sns

# Load the Iris dataset
iris = sns.load_dataset("iris")

# Display the first few rows of the dataset
print(iris.head())

# Create a pair plot
sns.pairplot(iris, hue='species', height=2.5)

# Show the plot
plt.show()
```

In this chapter, you have learned how to create different types of plots using **Matplotlib** and **Seaborn**. These plots are essential for data exploration and visualization in AI and machine learning projects.