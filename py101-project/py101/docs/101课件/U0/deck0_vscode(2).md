# Programming for AI (Python)
*Guoliang Ma, The Chow Institute, 20250.1 Syllabus*

## Course Overview
### What to Expect
- An introduction to basic Python data storage and functions.
- A basic understanding of data science with `numpy` and `pandas`.
- Classic AI/ML algorithms (theory and implementation with `scikit-learn`).
- Python classes and objects if time permits.

### Requirements
- Take notes and ask questions!
- Class attendance (occasional roll calls to be taken seriously).
- Effective communication (especially for questions and clarifications).
- High school math and some straightforward extensions.
- Please sit with someone with whom you can discuss questions.

### Group Projects
- **Kaggle competition**
- **Huggingface model exploration and evaluation**
- **Self-selected topics**

### Question Bank
- We’ll try to provide an optional question bank for you to practice what you’ve learned in class.
- For question-bank related issues, please consult the TA.

### Tentative Schedule (32 meetings/64 hours)
#### Basic (24 meetings)
- **6 meetings on Python basics: variable and its construction**
- **6 meetings on functions: basic, higher-order, algorithms, generator, and coroutine**
- **8 meetings on `numpy` and `pandas`**
- **2 meetings on drawing figures with `matplotlib`**
- **2 meetings on basic OOP**

#### Advanced (6 meetings)
- **2 meetings on classic machine learning algorithms**
- **2 meetings on large language models and Huggingface**
- **2 meetings for presentation (8 groups for two hours)**
- **2 meetings standby**

- Don’t panic! You’ll be fine (see score decomposition).
- If you really want to learn something, put in the effort.

## About You
### Background
- **Courses taken?** (e.g., data analysis/statistics, math, economics)
- **Programming experience?** (e.g., C/C++, R, SAS, Python, etc.)
- **English proficiency?** (e.g., CET4/CET6, TOEFL/IELTS, courses)
- Need to get used to my accent.
- **Data analysis contest?**
- More?

## Classroom Behavior
- Respect the instructor and fellow students.
- Use appropriate words.
- Mute your devices (QQ/WeChat, etc.).
- Obey the laws and rules.

## Why Python?
### Why Python?
- **The most widely used language in the AI/ML community.**
- One of the easiest coding languages for beginners.
- Versatility (capable of doing various things).

### Why in This Class?
- A concentrated course targeting data/ML/AI (hopefully).
- Not focused on data analysis.
- Most are too vacuous [empty] to foster your understanding.
- An informational course providing you with background knowledge (hopefully).
- Every design/feature comes from a need.
- You will know the key features of Python (pythonic as said by many).

## General Concepts (of a Language)
### ⚠ Code
- Characters, letters, etc., a form carrying information.

### Editors (Software)
- Where to write the code.

### Compiler/Interpreter (Software)
- Translating our code into something computers understand.

### Environment
- Interpreter’s home.
- **Environment manager (software)**

### In-class Exercise 0.5.1.1
1. List some editors that you have used.
2. Write your first line of code (in this class) in it.
3. Do you know how to install a software?

```python
print("Hello Python! ")
```

## Setting Up a Python Environment
### Required Software
- **VS Code**: A lightweight editor.
- **Anaconda**: A lightweight environment manager.

### Download Links
- [VS Code](https://code.visualstudio.com/)
- [Anaconda](https://www.anaconda.com/)

### Working with VS Code
1. Install two extensions: `Python` and `Jupyter`.

### In-class Exercise 0.5.2.1
- Launch VS Code and check out the layout.
  1. What do you observe?
  2. Find this...

### Working with Anaconda
- Anaconda is very different from many software you have used. It has a black box for you to enter code. Such a pure text/code interface is called a **command line interface (CLI)**.
- You enter code into the CLI to use it.

### In-class Exercise 0.5.2.2
- How do you open it?
- What do you observe?

### Environment Management Commands for Anaconda CLI
- **Create an environment:**
  ```bash
  (base) C:\Users\glma>conda create -n MLPython
  (base) C:\Users\glma>conda activate MLPython
  ```

- **Run a Python script:**
  ```bash
  (MLPython) C:\Users\glma>python "F:\2025S Python\00-introduction\test.txt"
  (MLPython) C:\Users\glma>python F:\2025S Python\00-introduction\test.txt
  ```

- **Install a package:**
  ```bash
  (MLPython) C:\Users\glma>conda install numpy
  ```

### Turning Back to VS Code
- Write some code here.
- Run [execute] the code.

```python
sentence = "Hello Python! "
print(sentence)
```

### In-class Exercise 0.5.2.3
1. How did you print “Hello Python!”?
2. What are the differences?

## What We Can Do with Python
### Example 0.6.1: Speech Recognition
```python
import whisper  # conda install -c conda-forge ffmpeg openai-whisper
model = whisper.load_model("base")
result = model.transcribe(audio="audio.m4a", fp16=False)
print(result)
```

### Example 0.6.2: Picture Recognition
```python
import easyocr  # pip install easyocr
reader = easyocr.Reader(['en'], gpu=False)
result = reader.readtext('F:/2025S Python/00-introduction/good.png', detail=0)
print(result)
```

### Example 0.6.3: Picture Generation
```python
from openai import OpenAI  # pip install openai
client = OpenAI(api_key=OPENAI_API_KEY)
response = client.images.generate(
    model="dall-e-2",
    prompt="driving in Ames in autumn",
    size="1024x1024",
    quality="standard",
    n=1,
)
print(response.data[0].url)
```

### In-class Exercise 0.6.1
1. Based on the previous three examples, can you summarize the procedure of working on a task in Python?
2. Where does the model come from?