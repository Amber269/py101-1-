# Programming for AI (Python) - Chapter 6: Objects
*The Chow Institute, 2025*

## What You Will Learn
- **Everything in Python is an OBJECT**
- An object has:
  - member attributes
  - member methods
- The `self` keyword
- Some magic dunder methods
- The `@property` descriptor

## 6.1 Python Objects
We have learned in the first week that "everything in Python is an **object**." At that time, we only said that an object consists of three parts. I promised to tell you more about objects, and here we are.

### Abstraction Layers
- Abstraction of basic Python statements form loops
- Abstraction of procedures form functions
- Another layer says:
  - Abstraction of relationships of and separation of variables from objects

### Defining Objects
Let's define some objects:

```python
import pandas as pd
a = int(1.0)
b = pd
c = (lambda x: x + 1)
d = type

def func():
    ...
```

#### In-class Exercise 6.1.1
Which of the above defines objects?

### Object Types
- When we run `type(a)`, we will see the type of the variable.
- When we define another integer, the type is the same.
- These identities show that all integers are defined (created) in the same way. Such a way is summarized under the name `int`. We refer to such ways of creating objects as **instantiation of classes**.

### Instantiating a Class
To instantiate a class, we first define a class:

```python
class MyInt():
    ...

class MyInt():
    value = 1
```

Now let's create an integer according to our own definition.

#### In-class Exercise 6.1.2
1. In fact, `int` is a class defined by Python. How would you create an integer from it?
2. How would you create an integer from our own definition?
3. How would you create another integer per our own definition?
4. Modify the value of one self-defined integer and see if the value changes.
5. Are the two self-defined integers the same?

*The above creation of distinct `MyInt`s is known as instantiation.*

*Source: [iStock](https://www.istockphoto.com/photos/cartoon-of-the-manufacturing-plant)*

## 6.2 The `__init__` Method
As you can see, if we define a variable from the namespace of the class, all instances know the changes and will act simultaneously. We need to give each instance its own namespace.

Because the namespace is for their own, we call them **self-namespace**.

```python
class MyInt():
    # value = 1
    self.value = 1
```

Directly forcing Python to make a `self` object did not go through. We need to provide a context so that Python can work on "self".

#### The `__init__` Method
The `__init__` method is very important in programming with classes. Note that we now define a function from within a class.

```python
class MyInt():
    def __init__(self, x):
        self.value = x
```

#### In-class Exercise 6.2.1
1. Can you call `__init__` from the global environment?
2. How can you call it?

The `__init__` method is specially known as the **constructor**. It sets values to each instance. It is called as if the function name was the class name:

```python
class MyInt():
    def __init__(self, x):
        self.value = x
a = MyInt(2)
```

#### In-class Exercise 6.2.2
1. Did you notice anything unusual?

#### In-class Exercise 6.2.3
1. Define a `Dog` class, where each dog should have a name, an age, and a weight.
2. Create three dogs: Spike, Luna, and Puppy. You can give them ages and weights.
3. Define a method `bark` for all dogs. When the dog barks, print on screen its name. For example:

## 6.3 Some Other Dunder Methods
### Example 6.3.1
```python
class Cat:
    species = "felion"
    def __init__(self, name, age):
        self.name = name
        self.age = age
    def __str__(self):
        return f"A {type(self).__name__} named {self.name}."
tom = Cat('tom', 3)
print(tom)
```

### Example 6.3.2
```python
class MyIterable:
    def __init__(self, *args):
        self.data = args
    def __iter__(self):
        return iter(self.data)
i1 = MyIterable(1, 2, 3, 4, 5)
isinstance(iter(i1), Iterator)
```

### Example 6.3.2
```python
class MyIterator:
    def __init__(self, *args):
        self.data = args
        self.counter = 0
    def __iter__(self):
        return iter(self.data)
    def __next__(self, i):
        if self.counter < len(self.data):
            ans = self.data[self.counter]
            self.counter += 1
            return ans
        else:
            raise StopIteration()
```

## 6.4 The `@property` Descriptor
In the previous dog age exercise, you set the age of a dog arbitrarily. However, a dog’s age cannot be negative. In order to protect users from setting meaningless values to objects' attributes, we introduce the `@property` descriptor.

```python
class Dog:
    def __init__(self, age=0):
        self.age = age
    def get_age(self):
        return self.age
    def set_age(self, age):
        if age > 0:
            self.age = age
        else:
            raise ValueError("Age must be positive")
xiaobai = Dog()
xiaobai.set_age(2)
xiaohuang = Dog()
xiaohuang.set_age(-1)
```

However, there are always people with mavericks who prefer other ways. In such cases, Python provides us with a descriptor which turns the variable into a property: whenever we need to access or change its value, it must be done with the functions.

```python
xiaohuang.age = -1
class Dog:
    def __init__(self, age=0):
        self.age = age
    def get_age(self): ...
    def set_age(self, age): ...
    age = property(get_age, set_age)
```

There are many other useful descriptors. Read [this](#) to learn more.

## 6.5 Python Objects
Now let’s talk philosophy: since all usage of a class/object can be done with “normal” Python variables and functions, why on earth do we need objects? The reason may differ from person to person. But one of the most important reasons is the convenience it brings, especially when we want to separate variables (by creating new namespaces).

For example, we want to manage students in a course. Let’s say this semester there are 76 students. If we want to create a profile for every person to store their names, homework scores, ages, classes. What would you use to do so?

#### In-class Exercise 6.1.1
1. We want to manage students in a course. Let’s say this semester there are 76 students. If we want to create a profile for every person to store their names, homework scores, ages, classes. What would you use to do so?
2. When it comes to the next semester, new students enroll. How would you automate the system to store the information of those new students?
3. Would you prefer to create a Python class or use some global variables?
4. Now due to privacy issues, we only want the scores to be available to each student. How would you modify your code?

Python objects are very flexible. You can even create object attributes on the fly:

```python
import pandas as pd
df = pd.DataFrame(
    [[1, 2], [3, 4]],
    columns=["A", "B"],
    index=["a", "b"]
)
df.C = [5, 6]
```

*Sources:*
- [Freepik](https://www.freepik.com/premium-vector/wrench-cartoon-style-repair-tools-vector-illustration-white-background_42202545.htm)
- [iStock](https://www.istockphoto.com/photos/cartoon-hammer)
- [Shutterstock](https://www.shutterstock.com/search/cartoon-drill?dd_referrer=https%3A%2F%2Fwww.google.com%2F)

*Programming Paradigm*