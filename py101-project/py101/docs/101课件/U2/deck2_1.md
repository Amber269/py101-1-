# Programming for AI (Python)
*The Chow Institute, 2025*

## Chapter 2 Functions
*Guoliang Ma*

### What you will learn
- How to define and call a simple function.
- How functions, as objects (chunk of memory), are stored and what happens internally when you call a function.
- How Python manages memory spaces and controls the accessibility of variables.
- "Functions are first-order objects."

## 2.1 Simple functions

### In-class exercise 2.1.1
**Review (read text file, work with dict):**
- **Context:** You are managing a database for a training camp. Under the data folder, there are students' information from 5 classes. Each class has an `age.txt` and `gender.txt`.
  1. Please read in the `age.txt` and `gender.txt` from class 1.
  2. Make a new `.txt` file to store the information of class 1.
  3. Repeat the steps for the other classes.

### 2.1 Simple functions
- From the above example, you notice that for each class, the job is repetitive, meaning that the same code is used more than once. What are the disadvantages?
  - Prone to errors
  - Hard to modify
  - ...
- We learn how to abstract the procedure and encapsulate it into a **function**.

### 2.1 Simple functions
- **How is a function composed?**
  - A function is designed to free us from repetitive jobs so that we can get more efficiency. So we need the function to know what we want to do. This is known as the **function body**.
  - The function will work on different classes (or more generally, objects). We need to let it know which one we want it to deal with. We give the case-dependent information to the function through **parameters**.
  - As always, name is so important. A function also needs a **name**.
  - There is another component of a function: the **return value**. We’ll talk about it later.

### 2.1 Simple functions
- **Putting them together:**
  - The above is the Pythonic style to define a function.
  ```python
  def <name>(<parameters>):
      <body>
      return None
  ```

### In-class exercise 2.1.2
- Assemble the code from exercise 2.1.1 to define a function.

### 2.1 Simple functions
- Once defined, the function will be stored in the memory as other objects. Try to print the function and see what is printed.
- Programming functions are just like math functions.
  - In math, we need to repeatedly find the distance between any two points \(P1 = (x1, y1)\) and \(P2 = (x2, y2)\). So we define the distance function \(f(P1, P2) = \sqrt{(x1 - x2)^2 + (y1 - y2)^2}\).
  - We define functions so we can use them. In Python, when we use a function, we **call** it.
  - In math, given two specific points \((1, 2)\) and \((3, 4)\). We evaluate the \(f\) function by passing the values to the function: \(f(1, 2, (3, 4))\).

### In-class exercise 2.1.3
- Call the function you just defined and apply it to class 2.

### 2.1 Simple functions
### In-class exercise 2.1.4
- Summarize the steps to define a function.

### In-class exercise 2.1.5
1. Define a function that computes the square of all numbers in a list. For example, if the list is `l = [1, 2, 3]`, the function finds `[1, 4, 9]`.
2. Store the new list for future use.

## Special topic I: the Python call stack

### Example 1.2.3.1 ways to create a dictionary
```python
import inspect

def print_stack():
    stack = inspect.stack()
    for frame in stack:
        print(f"Function : {frame.function}")
        print(f"Code context: {frame.code_context}")
        print("-" * 80)

print_stack()
```

### Special topic I: the Python call stack
- **A summary of Python function call:**
  - Add a local frame, forming a new environment.
  - Bind the function's formal parameters to its arguments in that frame.
  - Execute the body of the function in that new environment.
  - *source: Prof. John DeNero, CS61A, UCB*

### In-class exercise Special topic.1
- How do you distinguish a function from a function call?

## Special topic II: the Newton’s method
- This is more mathematical...
- Given any function, how do you find its root?
- The Newton’s method is one of the most commonly used.

### In-class exercise Special topic.2
- Write a function to find the root of \( f(x) = 0.3 \times x^2 - \sin(x) + x \) around \(-4.5\). When the error is smaller than a value, report the root.

## 2.1 Simple functions (special case I)
- As in the previous example, we can choose to set the error bound every time we run our Newton’s method function.
- But most of the time, we want to use the same value for fairness.
- Only in rare cases do we want to change the value.
- We preallocate a value to the function, which is known as the **default parameter**.

### In-class exercise 2.1.6
- Try the code below. What do you find? Can you explain?
  ```python
  def append_to(element, to=[]):
      to.append(element)
      return to

  my_list = append_to(12)
  print(my_list)
  my_other_list = append_to(42)
  print(my_other_list)
  ```

## 2.1 Simple functions (special case II)
### In-class exercise 2.1.7
- Try the code below. What do you find? Can you explain?
  ```python
  ff = {}
  for i in range(5):
      def f():
          return i
      ff[i] = f
  f[3]()
  ```

## 2.2 Namespaces and scope of variables
- We have learned that the Python interpreter binds names to objects. Objects live in the heap memory (mostly) and names live in the stack (frames). A **namespace** is a dictionary telling us which name is bound to which value.
- For example, we have the builtins, globals, enclosure, and the local namespaces.
- A relative concept is **scope**, which describes the range where an object can be accessed freely.

### 2.2.1 Namespaces and scope of variables
### In-class exercise 2.2.2.1
1. Write a function to swap the values of two objects. For example, `a, b = 1, 2`. After swapping, `a` is 2 and `b` is 1.
2. Analyze the function for variable names.
  - Check the names in the namespaces.
  ```python
  import builtins
  print(type(builtins), dir(builtins))
  dir(globals)
  ```

### In-class exercise 2.2.2.2
- How do you check the objects in the local namespace?

### 2.2.2 Scope of a variable
- When Python needs a name, the interpreter will search in the order of:
  - Builtins
  - Globals
  - Enclosure
  - Locals
  - Interpreter
  - Modules
  - Nested functions
  - Function
  - Nonlocal
  - Global

### In-class exercise 2.2.2
- What will the following give us? How can we make it normal?
  ```python
  print(max(1, 2))
  max = min
  print(max(1, 2))
  ```

### 2.2.2 Scope of a variable
- A long example
- A more complicated example

### In-class exercise 2.2.2.3 [purpose of variable scopes]
1. How does Python count the number of references?
   - Avoid unnecessary global variables.
2. How do you modify the function factory in Special case II?
   - Explicitly refer to a nonlocal variable.
3. How do you hide information from the global functions?
   - Follow the LEGB rule.

## 2.3 Functions are first-class objects
- First-class objects are flexible. Being first class means there are no restrictions on the use of the object. We can pass this object as an argument to a function and can return it as a return value. We can also create dictionaries to store it, etc.
- When we use a function as an argument and return values of another "higher-level" functions, we are using **higher-order functions**.

### 2.3 Functions are first-class objects
- **Functions as return values:**
  ```python
  def intercept_1():
      a = 1
      def slope_2(x):
          return 2 * x + a
      return slope_2

  linear_trans = intercept_1()
  linear_trans(3)
  ```

- **Functions as arguments:**
  ```python
  def call_count(func, x=[0]):
      print(f"calling {x[0] + 1} times")
      x[0] += 1
      func()

  call_count(print)
  call_count(print)
  call_count(print)
  ```

### In-class exercise 2.3.1
1. Modify code example 1, so that we can select the intercept.
2. Modify code example 1, so that we can also select the slope.
3. Modify code example 2, so that we do not need default parameters.

## 2.3 Functions … objects (special case III)
- In the `call_count` example, we can pass a function as an argument to the function. But this function cannot have its own parameters. How can we pass arguments to the function being counted?
  ```python
  def call_count(func, arg_to_called, x=[0]):
      print(f"calling {x[0]} times")
      x[0] += 1
      func(arg_to_called)

  call_count(print, "hello")
  call_count(print, "python")
  call_count(print, "world")
  ```

- When we are not sure about how many parameters to pass to the function, there are special keywords: `args` and `kwargs`.
- The `*` operator. A star is known as the (un)packing operator.

### In-class exercise 2.3.2
- How do they differ?
  ```python
  a = 1, 2, 3
  a, b, c = 1, 2, 3
  a, b = 1, 2, 3
  a, *b, c = 1, 2, 3, 4, 5
  ```

### In-class exercise 2.3.3
- Summarize the pattern by considering:
  ```python
  *a, b = 1, 2, 3, 4, 5
  a, *b = 1, 2, 3, 4, 5
  *a, *b = 1, 2, 3, 4, 5
  *a, b, c = 1, 2, 3, 4, 5
  *a, b = 1
  ```

- Note that `a` is a list but `*a` unpacks the list into several elements. Passing an indefinite number of arguments to a function involves two steps:
  1. Packing multiple arguments into a list.
  2. Unpacking the list with `*`.
- To check the unpacking behavior, we can use the `sep` parameter.
  ```python
  def call_count(func, *args, x=[0]):
      print(f"calling {x[0]} times")
      x[0] += 1
      func(*args, sep=", ")

  call_count(print, "hello", "python", "world")
  ```

- **The `**` operator:**
  - Another type of arguments is called **keyword arguments**, which must be passed to a function with the form `param=arg`. These are named arguments. Unlike `*` that unpacks a list, we use `**` to unpack a dictionary. There are fewer use cases than the unpacking of a list.
  ```python
  dict1 = {"a": 1, "b": 2, "c": 3}
  dict2 = {"d": 4, "e": 5, "f": 6}
  combined_dict = {**dict1, **dict2}
  ```

- **The order of `*args` and `**kwargs`:**
  - There is one important rule: unnamed arguments must be passed before keyword arguments.
  - Similarly, when we define a function, parameters with default arguments must follow parameters without.

### In-class exercise 2.3.4
- Note that `**kwargs` is actually unpacking a dict. This is to say, `kwargs` is a dict. We learned that a dict has keys and values. Read the document about named arguments: [https://docs.python.org/3/library/stdtypes.html#dict](https://docs.python.org/3/library/stdtypes.html#dict)
- Write a function to take the sum of several (the numbers are unknown) named arguments. For example,
  ```python
  def sum_of_kwargs(**kwargs):
      return sum(kwargs.values())

  sum_of_kwargs(Alice=5, Bob=3, Charlie=4)
  ```

## The assembly of tools
*source: [https://cn.nytimes.com/culture/20180515/2001-a-space-odyssey-kubrick/](https://cn.nytimes.com/culture/20180515/2001-a-space-odyssey-kubrick/)*