# Programming for AI (Python)
*The Chow Institute, 2025 - Guoliang Ma*

## What you will learn
- The general idea of machine learning
- Many terminologies in the machine learning context
- Several examples about the scikit-learn module

## 5.1 What machine learning is about

### Recall
In the Cobb-Douglas production function example, we wanted to find the “best” \(\alpha\) that gives us the closest estimation of the output for given capital and labor.

#### Terms
- **Model**: The equation describing the relationship between variables. For example, \(Y = K^\alpha L^{1-\alpha}\) describes the relationship between \(Y\), \(K\), and \(L\).
- **Target variable**: The variable we want to predict (on the left-hand side) or to know how it is generated. Also known as dependent or response in econometrics.
- **Predictors**: Those used to explain or predict the target variable (on the right-hand side). Also known as independent or explanatory in econometrics (covariates).

### Parameters
- **Parameters**: The symbol whose value is unknown. For example, \(\alpha\). Once we “know” the parameter values, we can compute the model-implied target.
- **Functional models and parameters** are our understanding of the world that generates the data.
- **Estimates**: Because parameters are not known, we need to guess which one is the best value (as we did to find the \(\alpha\)). Even if we can find a best value, we still have no idea if the data is actually from the model. It can be that our understanding of the world is not fully correct. In that case, the parameters from a wrong model cannot be discovered (in general). The best we can do for a given model is called an estimate.

### In-class exercise 5.1.1
What are the model, variables, and parameters?

1. A research team wants to predict the house price (\(P\)) for some houses. They collected the area (\(A\)), the distance to the town center (\(D\)), the age (\(G\)), and the materials (\(M\)) of several houses. They decided to use a complicated relationship:
   \[
   P = f_l(A, D, G, M),
   \]
   where
   \[
   l(A, D, G, M) = a + b_1 A + b_2 D + b_3 G + b_4 M,
   \]
   and
   \[
   f(x) = x \text{ only when } x \geq 0; \text{ otherwise } 0.
   \]

2. Another team wanted to learn about if bond prices (\(R\)) will go up or down. They collected several variables but due to privacy issues, they only tell us they are \(X_1, X_2, \ldots, X_{15}\). Their way of prediction is
   \[
   \text{Prob}(R > 0) = \frac{\exp(l(X_1, X_2, \ldots, X_{15}))}{1 + \exp(l(X_1, X_2, \ldots, X_{15}))}.
   \]

#### Terms
- **Classification vs. regression**: When we want to predict discretized target variables (e.g., if the bond prices go up or down instead of how much it moves; if a student will pass a course instead of the exact score; if Trump will increase tariffs instead of the rate by which it is increased), the task is classification. When we want to know the exact value of a continuous target variable, it is called regression.
- **Loss function**: The rule which tells the machine how well the parameter is estimated. For example, we have seen the absolute-error loss
  \[
  L(y, \hat{y}) = \sum_i |y_i - \hat{y}_i|,
  \]
  and the squared-error loss
  \[
  L(y, \hat{y}) = \sum_i (y_i - \hat{y}_i)^2.
  \]
  We can take derivatives of the loss function with respect to the parameters to find the estimates. This process is also known as "fitting the model to data."

### Machine Learning
- Machine learning methods fit to the data and help find the best data-generating mechanism (represented by the parameter estimates) from a class (they are the same functional form with different parameters).
- The problem we want to solve can be simplified and summarized as finding
  \[
  \hat{y}_i = f(\mathbf{x}_i; \hat{\theta}).
  \]
- In Python, we want to find such a function.

## 5.2 The slope-interception example

### Example
- In Chapter 2.3, we encountered one example:
  ```python
  def intercept_1():
      a = 1
      def slope_2(x):
          return 2 * x + a
      return slope_2
  linear_trans = intercept_1()
  linear_trans(3)
  ```

### More General Function
- The more general function was:
  ```python
  def intercept_and_slope(a, b):
      # a = 1
      def evaluation(x):
          # b = 2
          return a + b * x  # a nonlocal
      return evaluation
  ```

### Exercise
- Now let’s extend the exercise to fit some data:

| No. | y | x |
|-----|---|---|
| 0   | 0 | 0 |
| 1   | 0 | 1 |
| 2   | 1 | 2 |
| 3   | 3 | 3 |
| 4   | 5 | 4 |

#### In-class exercise 5.1.2
1. Please make a picture of a scatter plot to see how the data looks like.
2. Make use of the function we wrote or define new functions to find the best parameters for the model
   \[
   y = a + bx.
   \]

## 5.3 Two classic machine learning methods

- You may already feel the difficulty of writing your own code to find the estimates for the simplest machine learning method (linear regression). For some other machine learning methods, the function forms are much more complicated, and we sometimes only use pictures to represent them!
- For example, the neural network model and the classification-and-regression tree model.

### Example 5.3.1: The Neural Network Model
- Suppose we have 5 observations and 3 explanatory variables. We explain one continuous variable \(y\). A neural network model can be:
  \[
  z_1[1] = \text{ReLU}(a + b_1 x_1 + b_2 x_2 + b_3 x_3)
  \]
  where \(\text{ReLU}(x) = x\) only when \(x \geq 0\); otherwise, 0.

- Now let’s make the neural network more real and complex:
  \[
  \begin{aligned}
  &\tilde{a}_1 = b_1^{(1)} + w_{11}^{(1)} x_1 + w_{12}^{(1)} x_2 + w_{13}^{(1)} x_3, \\
  &\tilde{a}_2 = b_2^{(1)} + w_{21}^{(1)} x_1 + w_{22}^{(1)} x_2 + w_{23}^{(1)} x_3, \\
  &\hat{y} = \text{ReLU}(\tilde{a}_1) + \text{ReLU}(\tilde{a}_2).
  \end{aligned}
  \]

### Example 5.3.2: A Tree Model
- Let’s review the iris data, where the flowers are put into three classes. If we only care about the sepal width and sepal length, we could plot a few flowers.
- A tree model uses horizontal and vertical lines to split the x-space.
- We can keep doing so until we got the wanted number of sub-spaces.

### In-class exercise 5.1.3
1. Summarize the model and parameters in the previous neural network and tree examples.
2. Can you write code to implement the two functions in Python?
3. Can you take the derivative to help find the estimates?

## 5.4 The scikit-learn module

- When the model structures are relatively simple, we can write our own function to represent the function and find the derivatives mathematically. However, when the models become more complex, it is hard for us to define the functions and take derivatives by ourselves.
- For example, a typical neural network model may contain thousands of cells (neurons) and multiple layers. The picture below shows the idea.
  - A more complex neural network model (picture modified from: [source](https://doc.comsol.com/6.2/doc/com.comsol.help.comsol/comsol_ref_definitions.19.050.html))

- In such cases, we turn to the scikit-learn module.
  - Do not install `sklearn` (MLPython): 
    ```bash
    C:\Users\glma>conda install scikit-learn
    ```

- The scikit-learn machine learning methods are very similar. Let’s write our first tree model to classify the iris flowers.
  ```python
  import pandas as pd
  from sklearn.tree import DecisionTreeClassifier
  from sklearn import datasets
  iris = datasets.load_iris()
  clf = DecisionTreeClassifier()
  clf = clf.fit(iris.data[:-1], iris.target[:-1])
  res = clf.predict(iris.data[:-1])
  ```

- Let’s try neural network in scikit-learn:
  ```python
  from sklearn.neural_network import MLPRegressor
  from sklearn.datasets import make_regression
  from sklearn.model_selection import train_test_split
  X, y = make_regression(n_samples=200, n_features=20, random_state=1)
  X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=1)
  regr = MLPRegressor(random_state=1, max_iter=2000, tol=0.1)
  regr.fit(X_train, y_train)
  regr.predict(X_test[:2])
  regr.score(X_test, y_test)
  ```

- Of course, scikit-learn also provides the simple linear model:
  ```python
  from sklearn import linear_model
  reg = linear_model.LinearRegression()
  reg.fit([[0, 0], [1, 1], [2, 2]], [0, 1, 2])
  reg.coef_
  ```

- Scikit-learn also provides many other machine learning models.
- If you need more reference:
  - This book is a great starting point.
  - This video has a great focus on neural networks.
  - A more advanced version of neural networks.

## 5.5 Model selection and cross validation

- In neural network modeling, we can create our own networks. For example, we have seen the simplest network with only one layer and one cell in that layer. We have also seen a more complex network with three layers and multiple cells in each layer.
- But the question is: both models are neural network models. Which one is better?
- To answer this question, we are trying to select a specific model. This is known as the model selection problem.

### Cross Validation
- Here is the idea of cross validation, which helps us select the best model.
  - Source: [scikit-learn documentation](https://scikit-learn.org/stable/modules/cross_validation.html)

### In-class exercise 5.5.1
Let’s consider the simple linear model, where there are 10 explanatory variables. The data are stored in `sparsedata.csv`. This data already has the target variable included, so if we were only interested in these 70 observations, we do not need any model. However, later on, we collected another 30 observations without the target and stored them in `X_test.csv`. In other words, we want to train a model to help us predict the target on these data.

Here is how you can train a linear model with scikit-learn.

#### In-class exercise 5.5.1 (Cont’d)
Typically, the explanatory variables cannot help predict the target at the same time, so we will only consider the following model:
- \(y = a + b_1 x_1 + b_2 x_2 + b_3 x_3 + b_4 x_4 + b_5 x_5\)
- \(y = a + b_1 x_1 + b_3 x_3 + b_5 x_5 + b_7 x_7 + b_9 x_9\)
- \(y = a + b_2 x_2 + b_4 x_4 + b_6 x_6 + b_8 x_8 + b_{10} x_{10}\)
- \(y = a + b_1 x_1 + b_4 x_4 + b_8 x_8\)
- \(y = a + b_2 x_2 + b_6 x_6 + b_9 x_9\)

Please use cross validation to select a model from the above.

#### In-class exercise 5.5.1 (Cont’d)
For this data, we finally collected the target variable and stored them in `target_test.csv`. Use your selected model to predict on the 30 observations and see if your prediction is good.

## 5.6 A practical view of Kaggle

- Let’s go through the Optiver Realized Volatility Prediction task together to grab some basic info about the Kaggle platform.
- These are some main tabs:
  - **Overview**: We learn about the background context of the competition, including a Description, the Evaluation rule, the submission Timeline (important if we were to participate in an ongoing competition), and so forth.
  - **Data**: Kaggle provides us with the data we will use to train our machine learning models. For example,
  - **Code**: We can see what other people have done. It does not hurt to learn Python and machine learning from others. But you must clearly state that you used others’ code if you used it. Let’s take a look at what people have done here.
    - Source: [Free Cartoon Robot Vector](https://www.jamiesale-cartoonist.com/free-cartoon-robot-vector/)
  - **Feed me DATA!**