# Programming for AI (Python)
*The Chow Institute, 2025*

## Chapter 1: Basics
### What You Will Learn
- Very elementary Python theory
- More building blocks and more functions
- Control flow (for loop and if condition)

If we liken a program to a building, the **variables** are blocks and **syntax** tells the programmer how to put the blocks together to form walls. Different programs are just different ways to put the walls together.

## 1.2.3 Mapping — Dictionary

### Example 1.2.3.1: Ways to Create a Dictionary
```python
age = {"Alice": 21, "Bob": 32, "Charlie": 44}
name = dict(stu1="Alice", stu2="Bob", stu3="Charlie")
grade = dict([['stu1', 87], ['stu2', 99], ['stu3', 65]])
dict.fromkeys(["key1", "key2"], ...)
```

- The names of the elements in a dictionary are known as **keys** and the values are just **values**.
- One can look up an element (key-value pairs) in a dict by passing the value string to the dict. But if the key is not in the dict, Python will raise an error and the program is stopped. However, if we use the `get()` function, the program will continue.

#### In-class Exercise 1.2.3.1
1. What are the keys and values in the three dictionaries?

### Accessing Elements in a Dictionary
- Taking out elements by subscripting
- Error handling and the `get` function

```python
age["Alice"]
l = [1, 2, 3]
l[3]  # Raises an IndexError
age["David"]  # Raises a KeyError
```

## Special Topic: Flow Control (I) — For Loop

### How to Access All Elements in a List
- We could take them out manually by `l[0]`, `l[1]`, `l[2]`, etc.
- We could create an index variable `i` to help us:
- A more convenient way is to rely on the automated `for`-loop

```python
l = [1, 2, 3]
i = 0
print(l[i])
i = 1
print(l[i])  # Not often used, usually print is ignored

for element in container:
    ...
```

#### In-class Exercise Special (I).1
1. Make a list and use a `for` loop to print its elements.
2. Make a tuple and use a `for` loop to print its elements.
3. Make a dictionary and use a `for` loop to print its values.
4. Make a dictionary and use a `for` loop to print its keys.
5. Print the key-value pairs in a formatted way using the f-string.

### Useful Function: `range`
- `range` is very similar to slices.

```python
range(10)
range(1, 11)
range(0, 30, 5)
range(0, 10, 3)
range(0, -10, -1)
range(0)
range(1, 0)
```

## Special Topic: Flow Control (I) — If Statement

### Conditional Printing
- There are circumstances when we only want to print out certain elements of a list, tuple, or dictionary.
- For example, given a list, we only need the numbers that are squares of some integer, or cubics of some integers, or just odd numbers.
- In other words, only if the element satisfies a condition.

```python
l = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13]
if element % 2 == 0:
    ...
```

#### In-class Exercise Special (I).2
For the list containing 13 elements:
1. Print elements that are odd.
2. Print elements that are squares.
3. Print elements that are cubics.

### Comparison Operators
- `==` vs. `is`
- In the `if`-statement, the most commonly used condition is comparisons.
- Compare the values of two numbers by magnitude: `<`, `>`, `==`.
- Compare the identity: `is`.
- `None` is a special object in Python. Investigate it.

### Boolean Expressions
- Comparison expressions return boolean variables.
- Evaluation of comparisons supports chained expressions.

```python
a, b, c, d, e = 1, 4, 3, 3, 5
a < b > c == d != e
```

### Special Note on `None`
- When we compare an object with `None`, we should use `is`.
- In Python, we use `is` to compare the `id` of objects, so when we write `a = None`, `b = None`, `a is b` returns `True`.

## 1.2.3 Mapping — Dictionary

### Adding and Combining Dictionaries
- We can use the `[]` operator to add more elements.
- We can use the `update` method to combine two dictionaries.

```python
d = {'a': [1], 'b': [1, 2], 'c': [], 'd': []}
for i in d:
    if not d[i]:
        d.pop(i)

d = [1, 2, 3, 0, 5]
for i in range(4):
    if not d[i]:
        d.pop(i)
```

### JSON File as a Dictionary
```python
import json
with open("settings.json", "r") as f:
    setting_dict = json.load(f)
    print(setting_dict)
    print(setting_dict.items())
```

### Zips
- `zip` in language means 拉链.
- As the name suggests, Python `zips` involve two sequences just as real-life zippers.

```python
account = ["622848", "600314", "500297"]
balance = (1_000_000, 1_300_500, 500)
z1 = zip(account, balance)
for k, v in z1:
    print(k, "has a balance of", v)
```

## 1.2 Objects’ Types

#### In-class Exercise 1.2.2
1. What simple types have we learned?
2. What complex types have we learned?
3. How do you tell them apart?

## 1.2.4 String

### String Methods
- Strings are enclosed in quotation marks.
- Python seldom distinguishes between double and single quotation marks.
- String is a special collection of a container (strictly speaking, not).
- We introduce three sets of functions/string methods:
  - Upper and lower case conversions (`upper`, `lower`, `title`)
  - Unnecessary letter removing (`strip`, `lstrip`, `rstrip`)
  - Functions from the `re` module (`re.sub`)

#### In-class Exercise 1.2.4.1
1. Reverse a string. For example, given `s = "desserts"`, reverse it to get `"stressed"`. Reverse `"drawer"` to get `"reward"`. These are known as anadromes.
2. Remove vowels from a string. For example, `"drawer"` would become `"drwr"`.
3. Count the number of words in a string (using the `split` method).

## 1.2.5 Unordered Nonduplicate — Set

### Definition and Properties
- A set object is an unordered collection of distinct hashable objects.
- Set behaves just like the set concept we encounter in math courses. The elements of a set are unique and unordered.
- We can also use math concepts like `in` (∈), `issubset` (⊂), `union` (∪), `intersection` (∩), `difference` (−), and `symmetric difference` (Δ) to work on sets.

#### In-class Exercise 1.2.5.1
How do you check if an object is hashable?

### Creating and Modifying Sets
- Use brackets `{}`.
- Use the `set` function `set()`.
- Use set comprehension (later).
- We can modify a set once it's created. See the exercise.
- We create a `frozenset` mainly using `frozenset()`.

#### In-class Exercise 1.2.5.2
1. Create a set containing the numbers 1, 2, 3, 4, and 5.
2. Add the number 6 to the set.
3. Create two sets: `set_a = {1, 2, 3, 4}` and `set_b = {3, 4, 5, 6}`.
4. Find the union of `set_a` and `set_b`.
5. Find the intersection of `set_a` and `set_b`.
6. Find the difference between `set_a` and `set_b`.
7. Find the symmetric difference between `set_a` and `set_b`.
8. Given the list `numbers = [1, 2, 2, 3, 4, 4, 4, 5]`. Create a set to remove duplicate elements.
9. Convert the set back into a list (with fewer elements).

## 1.3 Pythonics

### Pythonic Code
- Pythonic is a loosely defined term that means “being special to Python” and is usually used to indicate the syntax that only Python has.
- Examples include `zip`, `with`, `None`, `sorted`, `f-string`, comprehensions, and `enumerate`.

### Comprehensions
- List comprehension
- Set comprehension
- Dictionary comprehension

```python
[x for x in range(5)]  # Usually faster than list, if not too complicated
{c for c in 'abcdcba'}
{x: x ** 2 for x in range(5)}
(if i in range(3))  # This does not create a tuple
```

### Adding Control Flow to Comprehensions
- Only the `if` condition
- `if` condition with `else` condition

```python
[x for x in range(10) if x % 2 == 0]
[x if x % 2 == 0 else x + 1 for x in range(10)]
```

#### In-class Exercise 1.3.1
How long does each of the following code take to run?

```python
[time.sleep(1), time.sleep(1), time.sleep(1)][0]
(time.sleep(1), time.sleep(1), time.sleep(1))[0]
```

## 1.4 Objects’ Values

### Object Storage
- An object is stored by its (i) `id`, (ii) `type`, and (iii) `contents`.
- What roles do these components play?
- Type-casting using the name of the type.
- Changing the types results in a new variable (object). How about changing the values?
- When the value change does not alter the address, the name reference is not changed. We say the object and the type of the object is **mutable**.
- Mutable objects are very useful and sometimes tricky. We’ll explore more in the next chapter.

#### In-class Exercise 1.4.1
What types of objects are mutable? How do you verify it?

## Further Readings/Watching
- Python开发者: https://nedbatchelder.com/text/names.html
- 高端学院派: https://cs61a.org/Review
- More types: Dictionary, string, and set
- Control flow: for-loop and if-statement
- Pythonic styles

*Source: https://www.thepaper.cn/newsDetail_forward_21329531*