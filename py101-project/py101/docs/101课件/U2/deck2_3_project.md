# Programming for AI (Python)
*The Chow Institute, 2025*

## Chapter 2: Functions
*By Guoliang Ma*

### What You Will Learn
- A few practice questions on functions from past exams.
- The types of questions you can expect to see in the midterm.

#### 1. Question 3 from Midterm of 2024 Fall
**Problem Statement:**
- If we toss a coin, it has two possible outcomes: head and tail. When a head (the number 1) appears, we write a “1” on a piece of paper. If a tail (flower) appears, we write a “0”. We toss the coin `n` times, and record all the outcomes.
- For example, this sequence is recorded as `1 0 1 1 1 0 1 0 1 1 0 0`.
- We define consecutive 1s as a **run**. So this sequence has 4 runs.
- In Python, you can use the following code to generate a random sequence:

```python
import random
n = 10
s1 = [random.randint(0, 1) for _ in range(n)]
```

**Task:**
- Please make a function to compute the number of runs of a sequence.

#### 2. Question 4 from Midterm of 2024 Fall
**Problem Statement:**
- For programmers, an important aspect of their code is the speed. Hence, it is common for programmers to try different ways to write functions. They will then test how long it takes to run each version of their code.
- To simplify the process, please make a decorator that can run the decorated function 100 times. Then after the function call, print a line including the average time and standard deviation of the 100 times. Please name this decorator `timer_100`.

**Sample Usage:**

```python
import random

@timer_100
def sums():
    numbers = [random.random() for _ in range(10000)]
    sum(numbers)

sums()
```

**Sample Output:**
```
The average run time is 0.000609s; the std is 0.000076s.
```

#### 3. Quasi-Newton’s Method
**Problem Statement:**
- **Newton’s method** has some restrictions:
  1. We need the starting point to be somehow close to the solution.
  2. We need the function to be differentiable.
- When the function is not differentiable, we need to rely on other methods. For example, the **secant method**:
  \[
  x_{n+1} = x_n - \frac{f(x_n)(x_n - x_{n-1})}{f(x_n) - f(x_{n-1})}
  \]
- **Task:**
  - Please implement this method and solve \( f(x) = x^3 - 1 = 0 \).

#### 4. Generator
**Problem Statement:**
- Make two generators:
  1. **Generator 1** is in charge of printing a line: "it is an odd second." and saves the current time for future use.
  2. **Generator 2** is in charge of printing a line: "it is an even second." and saves the current time for future use.
- Then write a function, which calls the two generators depending on if the `int(time.time())` has an even or odd integer part.