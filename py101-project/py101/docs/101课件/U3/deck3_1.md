# Chapter 3: numpy & pandas
*by Guoliang Ma, The Chow Institute, 2025*

## What You Will Learn
- A new and more general data structure: **array**
- Introductory `numpy` data structure: **ndarray**
- Basic `numpy` functions
- Reading function documents
- Matrix operations with `numpy`
- The broadcast mechanism
- Accelerating by vectorization

## 3.1 The `numpy` Module
It's been a while since we last used the conda environment. Let's get the prompt and install the two modules with:
```bash
(base) C:\Users\glma>conda install numpy
(base) C:\Users\glma>conda install pandas
```
We will mostly use `pandas`, but it's important to know some basic operations in `numpy` to help us play with data. We'll only cover a small portion of the `numpy` module. For an official introduction, refer to [this page](https://numpy.org/doc/stable/user/quickstart.html).

### 3.1.1 The `numpy` Module (ndarrays)
We've learned that a complex data type can hold several elements (maybe of different types). A similar idea of putting numbers into a sequence gives rise to the array type. According to the `numpy` document, "In computer programming, an array is a structure for storing and retrieving data."

We emphasize two features of `numpy` arrays:
1. They have fixed size (number of elements).
2. The elements must be of the same type.

#### Creating Arrays
Let’s create some arrays. We can use the `dtype` (stands for data type) parameter to specify the type of the elements.
```python
import numpy as np

a = np.array([1, 2, 3])
b = np.array([1, 2, 'a'])
c = np.array([a, b])
# d = np.array([a, 'python'])

a = np.array([1, 2, 3])
print(a)
a = np.array(a, dtype=float)
print(a)
```

#### In-Class Exercise 1
1. Write a function to compute the average of `t1`.
2. Write a function to compute the standard deviation of `t1`.
3. Write a function to compute the skewness of `t1`.

```python
t1 = (1, 2, 3, 4, 5)
```

#### Special `ndarray` Functions
The `numpy` module also provides us with some useful functions to create special `ndarrays`:
- `ones` that creates all-one `ndarrays`
- `zeros` that creates all-zero `ndarrays`
- `eye` that creates diagonal matrices
- `random.random` for random `ndarrays` with entries \( x: 0 < x < 1 \)
- `random.normal` for random `ndarrays` with entries normally distributed
- `random.randint` for random `ndarrays` with entries randomly drawn

#### Using `numpy.arange` and `numpy.linspace`
- We can use `numpy.arange` to make a range-like object.
- We can use `numpy.linspace` to create a sequence.

#### In-Class Exercise 2
Create a 3-D `ndarray`. Each entry of this `ndarray` should follow a normal distribution with mean 5 and standard deviation 3.

#### In-Class Exercise 3
What’s the difference between `numpy.arange` and `numpy.linspace`?

### 3.1.2 The `numpy` Module (Vectorization)
What makes `numpy` so popular and important in Python data analysis is its power in math operations, especially matrix algebra. This makes `numpy` efficient in dealing with vector and matrix operations. Some authors even wrote, "NumPy is all about vectorization." `numpy` is implemented in the C programming language and is very fast even though we write code in Python.

#### Example 3.1.2.1: How Much Faster is `numpy` than List?
```python
import numpy as np
import time

size = 10_000_000
python_list = list(range(size))
numpy_array = np.arange(size)

# List timing
stime = time.perf_counter()
python_list_squared = [x**2 for x in python_list]
etime = time.perf_counter()
list_time = etime - stime
print(f"Time taken by list: {list_time:.5f} seconds")

# Numpy timing
stime = time.perf_counter()
numpy_array_squared = numpy_array ** 2
etime = time.perf_counter()
numpy_time = etime - stime
print(f"Time taken by NumPy array: {numpy_time:.5f} seconds")

# Results
print(f"NumPy is {list_time / numpy_time:.2f} times faster than lists in this example.")
```

### 3.1.3 The `numpy` Module (Matrix Algebra)
In linear algebra, you learn how to add two matrices, as long as the dimensions match:
\[ M_1 = \begin{pmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{pmatrix}, \quad M_2 = \begin{pmatrix} 6 & 5 & 4 \\ 3 & 2 & 1 \end{pmatrix} \]
\[ M_1 + M_2 = \begin{pmatrix} 7 & 7 & 7 \\ 7 & 7 & 7 \end{pmatrix} \]

#### In-Class Exercise 4
1. Use lists to implement the above matrix addition.
2. Use `numpy` to implement the above matrix addition.

#### Broadcasting
We can also add a column to a matrix in `numpy.ndarray`. The mechanism that `numpy` introduces when we use math operators (+, -, *, /, etc.) is called broadcasting. It means that the “smaller” `ndarray` is broadcast to each element of the “larger” `ndarray`.

```python
m1 = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
v1 = np.array([1, 1, 1])
m1 + v1
```

#### In-Class Exercise 5
1. What does it mean by smaller and larger?

#### Handling Mismatched Sizes
What if the size does not match any of the dimensions?
```python
m1 = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
v1 = np.array([1, 1])
m1 + v1
```

#### Transposing Matrices
In linear algebra, we also learned how to transpose a matrix:
\[ M_1 = \begin{pmatrix} 1 & 2 & 3 \\ 4 & 5 & 6 \end{pmatrix}, \quad M_1^T = \begin{pmatrix} 1 & 4 \\ 2 & 5 \\ 3 & 6 \end{pmatrix} \]
This can also be done with `numpy`:
```python
m1 = np.array([
    [0, 1, 2, 3],
    [4, 5, 6, 7],
    [8, 9, 10, 11]
])
m1T = m1.T
```

#### In-Class Exercise 6
1. There is a `numpy.transpose`. What’s the difference with `.T`?

#### Reshaping `ndarray`
The `numpy.ndarray` is even more flexible; we can use the `.reshape` method to change the shape of a `numpy.ndarray`.
```python
m11 = m1.reshape(12)
print(m11, ":", m11.shape)
m12 = m1.reshape([12,])
print(m12, ":", m12.shape)
m13 = m1.reshape([12, 1])
print(m13.T, ":", m13.shape)
m14 = m1.reshape([2, 6])
print(m14.T, ":", m14.shape)
```

### 3.1.4 Revisit Vectorization
`numpy` is faster because of its vectorized nature. Recall that we once tried to add a number to a list, and there was an error. But with `numpy.ndarray`, we can easily add a scalar to an array.

```python
lst = [1, 2, 3]
lst + 4  # This will raise an error
```

More generally, we introduce some universal functions (ufuns) of `numpy`. The reason is not only their speed but also their ease of use. We've seen that list operations are much slower than `ndarray`. Now let's focus our attention on the comparison between loops and ufuns, both applied to a `ndarray`.

#### Example 3.1.4.1
```python
def timer_100(func):
    times = []
    def wrapper(*args, **kwargs):
        for i in range(100):
            stime = time.perf_counter()
            func(*args, **kwargs)
            etime = time.perf_counter()
            times.append(etime - stime)
        avg = np.mean(times)
        std = np.std(times)
        print(f"The average run time is {avg:.6f}s; the std is {std:.6f}s")
    return wrapper

@timer_100
def compute_reciprocals(nums):
    output = np.empty(len(nums))
    for i in range(len(nums)):
        output[i] = 1.0 / nums[i]
    return output

compute_reciprocals(np.random.randint(1, 10, size=10_000))

@timer_100
def numpy_reciprocals(nums):
    return 1.0 / nums

numpy_reciprocals(np.random.randint(1, 10, size=10_000))
```

#### In-Class Exercise 7
1. Although we saw that the time difference was huge between `compute_reciprocals` and `numpy_reciprocals`, we haven't checked if the answers are correct. Verify the correctness of these two functions.

#### Common Universal Functions
Besides common operators like +, -, *, /, commonly used functions, including `abs`, `sin`, `cos`, `tan`, `arcsin`, `arccos`, `arctan`, `exp`, and `log`, are all `numpy` ufuns. And `numpy` provides more. For example, `expm1` computes \( e^x - 1 \) and `log1p` computes \( \log(1 + x) \).

#### Custom Universal Functions
Although abundant, sometimes we still find `numpy` ufuncs not enough to support all kinds of operations we need. `numpy` provides us with a `numpy.vectorize` function to help us write our own ufuns.

#### Example
```python
def f(x):
    return 1 / (1 + x**2)

vectorized_f = np.vectorize(f)
```

#### In-Class Exercise 8
1. Read the documentation and write your own function to compute \( f(x) = \frac{1}{1 + x^2} \). Then test the speed of the vectorized function.

#### In-Class Exercise 9
1. Read the documentation of the `min` and `mean` functions. Given a `ndarray`:
   1. Select those entries whose values are greater than 3 (the mask).
   2. Find the min value of each row.
   3. Find the mean of each column.

```python
arr = np.array([
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
])
```

### 3.1.5 View vs. Copy
A very complex phenomenon of `numpy` is the difference between a view and a copy. If we use analogy, a view offers us a way to look at the data so the data remains itself (no subdata is taken out). A copy, on the contrary, means we create a subdata and give it a new name. The main difference between a view and a copy is when we change their contents. For example, `y` is a view of part of `x` below:

```python
x = np.arange(10)
y = x[1:3]
print(f"before changing x, y: {y}")
x[1:3] = [10, 11]
print(f"after changing x, y: {y}")
```

We can check if `y` is a view of `x` using the `.base` attribute:
```python
y.base is x
```

For further readings:
- [Numpy Views vs. Copies](https://numpy.org/doc/2.0/user/basics.copies.html)
- [Scipy Cookbook: Views vs. Copies](https://scipy-cookbook.readthedocs.io/items/ViewsVsCopies.html)
- [Stack Overflow: Numpy Views vs. Copies](https://stackoverflow.com/questions/47181092/numpy-views-vs-copy-by-slicing)