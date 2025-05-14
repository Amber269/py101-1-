# Programming for AI (Python): Chapter 1 Basics
*by Guoliang Ma, The Chow Institute, 2025*

## Iterables
Loosely speaking, we can think of **Iterables** as if they are a type. But more precisely, **Iterables** refer to any objects that share some commonalities.

### Characteristics of Iterables
1. All **Iterables** can be passed to a `for` loop, as in:
   ```python
   for i in Iterable:
       ...
   ```
   This corresponds to its name: being **ABLE** to be **ITER**ated.

2. If we think of `iter` as a verb (it is actually a function in Python), then only **Iterables** can be the argument of the `iter` function. But how do we know if an object can be passed to the `iter` function? There must be something special about the **Iterables** making them capable of being the argument of the `iter` function.
   ```python
   a = [1, 2, 3]
   b = 1
   iter(a)  # correctly get an iterator
   iter(b)  # error
   ```

3. What makes **Iterables** special is their method `__iter__`. We have learned that everything in Python is an object, and objects can contain many pieces of information. For example, you can use `dir(a)` to see what a list contains. The defining property of **Iterables** is that they contain a method called `__iter__`. So every time we ask Python to `iter(a)`, Python will sneakily do `a.__iter__()`. This can be done only when the object has the `__iter__` method.
   ```python
   a = [1, 2, 3]
   dir(a)
   ```

4. Sometimes it is also possible to pass non-**Iterable** objects to a `for` loop. In such cases, the object must contain the `__getitem__` method.

### Iterables and Iterators
- **Iterators** form a subset of **Iterables**. Therefore, iterators must contain the `__iter__` method. Moreover, iterators must contain `__next__` to distinguish from **Iterables**.

### Why Do We Need Iterables and Iterators?
- **Iterables** are everywhere, and we don’t need to create **Iterables**. We can use the **Iterables** Python provides to us. The main use is to convert an **Iterable** to an iterator using the `__iter__` method (or equivalently, the `iter` function).
- We need **iterators** because we don’t want Python to do the job immediately. We want it to do what is needed only when we tell it to do so.