# Programming for AI (Python): Chapter 2 - Functions
*The Chow Institute, 2025 - Guoliang Ma*

## What You Will Learn
- **Functions** in functional programming
- "Functions are first-class objects."
- Functions are also objects.
- Functions can interact with other functions.
- Functions can hold inside variables and be assigned names.
- (Moreover) a necessity for functional programming
- (Moreover) in the design of a language

We’ll rely on several specific use cases to introduce these topics.

## 2.4 Use Case I: Decorator
### Review
Before we start decorators, let's review what we've learned.

#### In-Class Exercise 1
1. Write a function to compute the average of `t1`.
2. Write a function to compute the standard deviation of `t1`.
3. Write a function to compute the skewness of `t1`.

```python
t1 = (1, 2, 3, 4, 5)
```

### Handling Invalid Values
If you encountered an error when we tried to pass `t2` to our function, the right thing to do is to make sure there are no invalid values in the arguments passed to our function. Here we introduce the `assert` statement.

- `assert` is a claim; we assert an expression. If the value of the expression is `True`, nothing happens, and the program proceeds. But if the value is `False`, the assertion fails, and an error occurs.

#### In-Class Exercise 2
1. Apply your average function to `t2`.

```python
t2 = (1, 2, 3, None, 5)
```

### Examples of `assert` Statements
```python
assert None not in [1, 2, 3], "All values look good!"
assert None not in [1, 2, 3], "Contains None!"
```

#### In-Class Exercise 3
1. Use `assert` to improve your functions.

### Higher-Order Functions
Although `assert` is handy, if you have written many functions, it is hard to modify all of them. In other cases, if someone else defines the function for us, we may not modify the function ourselves. So when we want to extend the functionality of a function, we can define a higher-order function and use the function we want to modify as its argument. Do whatever we want, and then send out the new function.

#### Syntax Template
```python
# to define a decorator as a higher order function
def decor(func):
    def wrapper(*args, **kwargs):
        ...
        return func(*args, **kwargs)
    ...
    return wrapper

# to decorate a function
@decor
def func():
    pass
```

#### In-Class Exercise 4
Write decorators to:
1. Print a line `<function name> function is being called.` before the execution and print a line after the execution: `finish calling <function name> function.`
2. Record return values of previous function calls. Every time a function is called, record the return value so that next time the function is called, we don't have to wait for the function to run.

```python
print(1)
# Output:
# print function is being called.
# finish calling print function.
```

## 2.4 Use Case II: Recursion
Another use case of higher-order functions is recursion: if any function in Python can be passed to another function as a parameter, what if the function itself is passed to itself?

### Revisit Newton’s Method
Suppose we want to find the root of \( f(x) = x^2 - 3 \). Start from 0.5.

- By Newton’s method:
  - \( f'(x) = 2x \)
  - \( x_{\text{next}} = x_{\text{current}} - \frac{f(x)}{f'(x)} = x_{\text{current}} - \frac{x^2 - 3}{2x} \)

```python
def newton(x):
    return x - (x**2 - 5) / (2 * x)

x = 0.5
for _ in range(10):
    x = newton(x)
    print(x)
```

### Recursive Calls
Pay attention to the relation:
- Is it the same as:
  ```python
  for _ in range(10):
      x = newton(x)
      newton(newton(newton(newton(newton(newton(newton(newton(newton(newton(0.5))))))))))
  ```

### Simplifying Recursive Calls
Here is a starting point. Try to modify the code by yourself to make it work.

- Summary:
  - A boundary condition is used to ...
  - A recursion body is used to ...
  - The difference between recursion and loops includes ...

```python
def newton_recursion(x):
    return newton_recursion(x)
```

### Sum Function
Based on what we learned from the previous example, can you see how the `sum_recursion` function works?

```python
def sum_recursion(x):
    if len(x) == 0:
        return 0
    else:
        return x[0] + sum_recursion(x[1:])
```

### Square Root
We have relied on Newton’s method to find the square root of a number. There is another method.

- Mathematically, if we want to find the root of \( x \), the root must satisfy:
  - \( r = \sqrt{x} \)
  - If the relation is satisfied, then we will see:
    - \( 2r = r + \frac{x}{r} \)
    - Or,
      - \( r = 0.5 \times \left( r + \frac{x}{r} \right) \)

#### In-Class Exercise 5
1. Write a `root_recursion` function to find the square root of a number.
   - Hint: Start from finding the square root of 7 and then generalize your function.

### Fibonacci Sequence
Recursive relation is fundamental in guiding the implementation of a recursion function. You’re (or should be) extremely familiar with it.

#### In-Class Exercise 6
1. 推导斐波那契数列的通项公式。它的递推公式为：
   - \( a_{n+2} = a_{n+1} + a_n \)
2. Please implement a Python function to compute the values of the Fibonacci sequence.
   - Hint: You can choose from the two ways.

### Factorial Function
As a last exercise, we work on an easier one.

#### In-Class Exercise 7
1. Please write a function to compute the factorial of some number.

## 2.4 Use Case III: Map, Filter, and Reduce
Let’s start from the familiar `max` function. You already know the output of the `max` function:

- If you try `max("hello", "world", "Python")`, you’ll see:
  - `Python`
- `max((1, 2, 3))`
- `max(1, 2, 3, 4)`

### Customizing `max` Function
If we’re not satisfied with the default behavior, we can change it by defining a helper function that:
- Takes the elements for comparison as parameters
- Returns the standard on which we want to sort

```python
def helper(s):
    return len(s)

max("hello", "world", "Python", key=helper)
```

### Anonymous Functions
Is it unnecessary to define the helper function every time we want to find the maximum of some elements according to some standards? We can abandon the name of the function and just use it. This is known as anonymous functions.

- The syntax is:
  ```python
  max("hello", "world", "Python", key=lambda x: len(x))
  ```

### Map, Filter, and Reduce
- The `map` function applies a function to every element of a sequence.
- The `filter` function selects the elements according to a standard. Similar to the helper function in the `map` function, we need to set the standard as a function. But this time, the return value of the helper function has to be Boolean.

```python
map(len, ("hello", "world", "Python"))

def long_string(x):
    return len(x) > 5

filter(long_string, ("hello", "world", "Python"))
```

#### In-Class Exercise 8
1. Replace the helper function with an anonymous function.

### Reduce Function
The last technique is the `reduce` function. Unlike the other two, we need to import it from the `functools` module. The `reduce` function requires a helper function. The helper function for `reduce` must:
- Take two parameters
- Return the value of the same type

```python
from functools import reduce

reduce(lambda x, y: x + "" + y, ("hello", "world", "Python"))
```

## 2.4 Use Case IV: Generators
We have learned iterators (what is an iterator?). Generators are a special way to create iterators. There are several ways we can make generators.

- We can create our own generators in two ways:
  1. Through generator expression
  2. Through a generator function

#### In-Class Exercise 9
1. List examples of functions that output generators.

### Generator Expression
We learned the idea of comprehensions in Chapter 1. The parentheses-enclosed comprehensions were not "tuple comprehensions". They are generators.

```python
type((x for x in range(5)))
```

#### In-Class Exercise 10
1. Make a generator to help compute squares (1, 4, 9, 16, ...). What is necessary for you to define a generator like this?

### Generator Function
Generator functions are almost the same as regular functions, except that we use the word `yield`. Wherever in a function there is a `yield`, the function becomes a generator function.

- `Yield` means to give up the right to someone else. The yield signs we see on the road tell us to give up road right to others.
- In a computer program, when a generator function sees `yield`, it does the same thing. The generator function is suspended, and other functions can use the computing resources. When we revoke the generator function in a future time, it regains the computing resources until it sees `yield` next time.

#### Example 2.4.1: Generator Function
```python
import time

def g():
    print(f'g() sleeping: gi_state : {g1.gi_running}')
    time.sleep(3)
    print(f'g() sees yield: gi_state : {g1.gi_running}')
    yield 1

g1 = g()
next(g1)
print("after, gi_state :", g1.gi_running)
```

### Computing Squares
Now we see how a generator function works, and we can define a generator with it. Suppose we want to compute the squares of integers from 1 to infinity. This is impossible if we are using a list (why?). But with a generator, we don't need to store all squares in the computer. Just compute them on-the-fly.

```python
def squares():
    i = 1
    while True:
        yield i**2
        i += 1
```

## 2.4 Use Case V: Error Message
We introduce error message reading and handling. To handle an error, we rely on the `try-except` control flow.

- The following code shows you how to print a hand-written number:

```python
with open("../data/numbers/number1.csv", "r") as f:
    header = f.readline()
    data = f.readline()

import matplotlib.pyplot as plt
import numpy as np

pixels = np.array(data.split(","), dtype='uint8').reshape((28, 28))
plt.imshow(pixels)
plt.imsave(f"./mnist{i}.png", pixels)
```

### Handling Errors
You can see that the flow is interrupted because of an error. But all other pictures are good. Checking all data before we plot and save the pictures is a daunting task. So we need to rely on the `try-except` control flow. The syntax is as follows:

#### In-Class Exercise 11
1. Plot and save all numbers in the numbers folder.

```python
try:
    ...
except KeyError as e:
    ...
else:
    ...
finally:
    ...
```

## 2.5 PEP 8
- The Python Enhancement Proposals (PEPs) are a set of documents aiming at improving the Python programming language.
- **PEP 8** is a style guide for coding in Python.
- It is not required but recommended.

### References and Read More
- [Bilibili Video](https://www.bilibili.com/video/BV1Y2421A7sB/?spm_id_from=333.337.search-card.all.click&vd_source=ec7b194853f6121829b0f428c7736022)
- *Mastering Functional Programming with Python*, Steven Lott, 2015