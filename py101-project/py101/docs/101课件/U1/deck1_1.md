# Programming for AI (Python)
*The Chow Institute, 2025*

## Chapter 1: Basics
### What You Will Learn
- Very elementary Python theory
- Some building blocks
- Some functions
- Memorize the names
- Know the meaning of their parameters
- Understand and interpret the output

If we liken a program to a building, the **variables** are blocks and **syntax** tells the programmer how to put the blocks together to form walls. Different programs are just different ways to put the walls together.

## 1. The Boring ⚠ Variables
Programming is an art of putting mathematical ideas into practice. During your past study of math, you must have seen abstraction several times. Let's talk about an example. The most elementary abstraction in math must be the creation of a **variable**. In primary schools, we learned to count 1, 2, 3... Then in middle school, we learned to use the notation `x` to represent a generic [general, representative] number. So when we see `x=1`, `x=2`, or `x=100`, we are not surprised that the single letter `x` can be so versatile.

In Python, we call `x` **names**. When we create a variable, we want the variable to represent some **values**. What Python does for us is to first create a chunk of memory to store the value of the variable, and then bind the value (that piece of memory) to the name we provided. See my picture on the whiteboard.

### 1.1 Example 0.6.3 Picture Generation
```python
# quick verification
a = 1
print(id(a))
b = a
print(id(b))
# not appearing as an address? format it
print(f"{id(b):02X}")
```

In the above chunk of code, we saw:
1. The `print` function: to show the contents of the printed
2. The `id`: to give you the address of the object in memory
3. F-string for formatting: just "f" and "{}"

### 1.2 In-class Exercise 1.1
1. Create a name-value pair to store the name of a person, who is "Alice"
2. Print Alice's "name"
3. Bind the variable to "Bob"
4. Using f-string to print the contents of "name"

### 1.3 “Names Refer to Objects”
- “Everything in Python is an object”.
- While we are not formally introducing objects, we can loosely say that objects are chunks of memories with specific structures.

#### 1.3.1 What Are Special About an Object?
- An object comprises...

### 1.4 In-class Exercise 1.1.1
1. Can you summarize some features of an object?
   - Hint: We have learned that everything is an object. What do they have in common?

#### 1.3.2 The Contents of the Object
- The contents of the object are referred to as the **memory layout**.
- Memory layout tells us how the chunk of memory is arranged to store the data.
  ```python
  import sys
  sys.getsizeof(a)
  ```

## 1.4 Objects’ Types
- The topic of variable types is large. The official documentation is very long: [https://docs.python.org/3/library/stdtypes.html](https://docs.python.org/3/library/stdtypes.html)
- Let’s see a few examples first.
  ```python
  a = 1
  b = 2.0
  c = 3.14 - 5j
  cond = True
  ```

### 1.4.1 In-class Exercise 1.2.1
Use the `type` function to see what types they are.

### 1.4.2 Simple Types — Boolean
- The Boolean type is special in Python. Let’s investigate.
- We will need a new function, `isinstance`. This function helps us determine if a variable is one member of a type.
  ```python
  a = 1
  isinstance(a, int)
  isinstance(a, type(a))
  cond = True
  isinstance(cond, int)
  print(cond + 1)
  ```

### 1.4.3 In-class Exercise 1.2.1.1
1. If `True` in Python is also `1`, how about `False`?
2. Try other types with the `isinstance` function. Do you find any interesting things?
3. Search the web for the document of the `isinstance` function and read.

### 1.4.4 Complex Types
- All the previous examples are types that are of one variable. In Python, we can put several variables together and create new types.
- For example, in high school, you learned that `1` is a number, `2` is a number, and `{1, 2}` is a set.
- In Python language, the type consisting of several variables includes sequences, mappings, strings, etc.

### 1.4.5 Sequences — List & Tuple
- **List** and **tuple**
- These are the most frequently encountered.
- The creation is by enumeration.
  ```python
  l = [1, 2, 3]
  t = (1, 2, 3)
  ```

### 1.4.6 In-class Exercise 1.2.2.1
1. Try to print a list.
2. Try to print one element of a list.
3. Create a list containing 100 numbers from 1 to 100.

### 1.4.7 In-class Exercise 1.2.2.2
1. Try to print a tuple.
2. Try to print one element of a tuple.
3. Create a tuple containing 100 numbers from 1 to 100.

### 1.4.8 List Functions and Tuple Functions
- Lists and tuples have their own functions that are specially designed.
- These “functions” work only for one certain list or a certain tuple.
- These special “functions” are called “methods” to distinguish from normal functions.
- Most methods indeed change the value of the caller.
- `append` vs. `extend`
- `pop`
- The “+” operator

### 1.4.9 In-class Exercise 1.2.2.3
1. Use `l` and `t` you just created.
   1. Set the second element of `l` to `-1`.
   2. Set the second element of `t` to `-1`.
2. Analyze the memory layout of the changes.

### 1.4.10 Similarities and Comparisons
```python
l = [1, 2, 3]
t = (4, 5, 6)
l[0] = 1  # ✓
t[0] = 1  # ✗
del l[1]  # ✓
del t[1]  # ✗
```

```python
l = [1, 2, 3]
t = (4, 5, 6)
l.append(4)  # ✓
l.reverse()  # ✓
l.extend((5))  # ✓
l.pop(0)  # ✓
t.append()  # ✗
t.reverse()  # ✗
t.extend()  # ✗
t.pop()  # ✗
```

```python
l = [1, 2, 3]
t = (4, 5, 6)
l += [6, 7]  # same id as before
t += (8,)  # id changes
```

```python
l = [1, 2, 3]
t = (4, 5, 6)
print(l[1])  # ✓
print(t[2])  # ✓
l[1] = "list"  # ✓
t[2] = "tuple"  # ✓
1 in l  # ✓
2 in t  # ✓
# both are sequence types
```

### 1.4.11 Slices
- You know how to take one element from a list. We can also take many elements from a list, using a slice.
- The slicer is of the form: `<start>":"<stop>[":"<step>]`
  - These must appear as is.
  - These are numbers you can choose.
  - Whatever inside square brackets may not appear. But if they appear, they follow the same rules.

### 1.4.12 Slice Objects
- To create a slice object, we need to use parentheses to enclose it.
- To use a slice object, we can ignore the parentheses inside the square brackets.
  ```python
  s = slice(1, 10, 2)
  l[s]
  ```

### 1.4.13 In-class Exercise 1.2.2.4
1. Is `slice` a function?
2. Is `l` a function?

### 1.4.14 In-class Exercise 1.2.2.5
1. Are these slices?
   1. `1:2:1`
   2. `2:4:7`
   3. `9:1:-1`
   4. `a:b:c`
   5. `1.5:2.3:3.14`
   6. `a:2:3`
   7. `6:7`
   8. `:-5:-1`
   9. `::-1`

### 1.4.15 Sequences – Array & Deque
- These are also sequence types that we do not cover, but...
- Array elements have to be the same, otherwise, it is very similar to a list.
- Array is much faster because it relies only on offsets.
- You need to import `array` before using it.
- You can find `deque` here: [https://docs.python.org/3/library/collections.html#collections.deque](https://docs.python.org/3/library/collections.html#collections.deque)
- There are too many functions/methods to cover in class.
- You have learned the “how to.”

## Review
- Python variables are actually objects with names.
- Objects consist of three components: **id**, **type**, and **value**.
- Basic types: Boolean, sequences, etc.

*Source: [https://miscrave.com/articles/your-name-flavors-youth/](https://miscrave.com/articles/your-name-flavors-youth/)*