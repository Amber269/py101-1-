# Python 基础教程
*来源：本文内容参考了多个资源，包括但不限于 Ned Batchelder 的博客、Python 官方文档以及 Real Python 和斯坦福大学的教程。*

## Python 数据类型概览
在 Python 中，数据类型是构建程序的基础。Python 提供了几种内置的数据结构，每一种都有其独特的用途和特性。根据 [Python 文档](https://docs.python.org/3/tutorial/datastructures.html)，主要的数据类型包括列表（list）、元组（tuple）、字典（dictionary）等。这些数据类型支持多种操作，如添加元素、删除元素及访问特定位置的值。

### 列表 (List)
- **可变性**：列表是**可变**的数据结构，这意味着你可以在创建后修改列表中的元素。
- **索引**：列表中的每个元素都可以通过索引访问，索引从 0 开始。
- **方法**：列表提供了许多有用的方法，比如 `append()` 来添加新元素到列表末尾，`remove()` 用于移除指定值的第一个匹配项。

```python
my_list = [1, 2, 3]
my_list.append(4)  # 添加数字 4 到列表末尾
print(my_list)  # 输出: [1, 2, 3, 4]
```

### 元组 (Tuple)
- **不可变性**：与列表不同，元组一旦创建就不能被修改。
- **使用场景**：当你需要存储一组不会改变的数据时，可以考虑使用元组。
- **性能优势**：由于其不可变性，元组在某些情况下比列表更高效。

```python
coordinates = (10, 20)
print(coordinates[0])  # 输出: 10
```

## 字符串格式化
字符串是 Python 中非常重要的数据类型之一，它允许开发者以灵活的方式处理文本信息。Python 提供了多种方式来格式化字符串，其中最现代且推荐的做法是使用 f-string。

### F-String 格式化
- **简洁性**：f-string 提供了一种简洁的方式来嵌入表达式的值到字符串中。
- **语法**：只需在字符串前加上字母 `f` 或 `F` 并将要插入的变量或表达式放在大括号 `{}` 内即可。

```python
name = "Alice"
age = 30
greeting = f"Hello, {name}. You are {age} years old."
print(greeting)  # 输出: Hello, Alice. You are 30 years old.
```

更多关于 f-string 的详细信息，请参阅 [官方文档](https://docs.python.org/3/tutorial/inputoutput.html) 或 [Real Python 教程](https://realpython.com/python-f-strings/)。

## 循环结构
循环是编程语言中控制流的重要组成部分，允许代码块重复执行直到满足某个条件为止。Python 支持几种类型的循环，但最常用的是 `for` 循环。

### For 循环
- **遍历序列**：`for` 循环非常适合用来遍历任何序列（如列表、元组、字符串）中的元素。
- **迭代器协议**：Python 的 `for` 循环实际上依赖于对象实现的**迭代器协议**，即对象必须提供 `__iter__()` 方法返回一个迭代器对象，并且该迭代器需定义 `__next__()` 方法。

```python
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```

有关 `for` 循环的更多信息，请查看 [Real Python 上的教程](https://realpython.com/python-for-loop/)。

## 字符串操作
除了基本的格式化外，Python 还提供了丰富的字符串操作功能。这包括但不限于连接、分割、查找子串等。

### 字符串方法
- **连接**：使用 `+` 操作符或者 `join()` 方法可以将多个字符串合并成一个新的字符串。
- **分割**：`split()` 方法可以根据指定分隔符将字符串拆分成列表。
- **替换**：`replace(old, new)` 可以用来替换字符串中的部分文本。

```python
sentence = "Welcome to the world of Python"
words = sentence.split(" ")
new_sentence = " ".join(words).replace("Python", "Programming")
print(new_sentence)  # 输出: Welcome to the world of Programming
```

更多高级的字符串操作技巧，建议阅读 [斯坦福大学 Nick Parlante 教授编写的教程](https://cs.stanford.edu/people/nick/py/python-string.html)。

## 对象命名约定
良好的命名习惯对于编写易于理解和维护的代码至关重要。PEP 8 是 Python 社区广泛接受的一套编码规范，其中包括了对变量、函数以及其他标识符命名的具体指导原则。

- **小写加下划线**：对于大多数情况下的变量名，推荐使用小写字母并用下划线分隔单词。
- **驼峰式大小写**：虽然不常见于 Python，但在某些上下文中（例如类名），可能会看到首字母大写的驼峰式命名法。

更多关于如何为你的 Python 项目选择合适的名称，请参考 [PEP 8 命名约定](https://peps.python.org/pep-0008/#naming-conventions)。