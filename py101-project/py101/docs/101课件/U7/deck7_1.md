# Programming for AI (Python)
*The Chow Institute, 2025 - Guoliang Ma*

## What You Will Learn
- Converse with LLMs from Python
- Some very basic mechanism of LLMs
- The RAG technique
- Some examples about RAG

## 7.1 Calling LLM APIs
We are all familiar with conversing with large language models (LLMs) on their websites (through the dialogue box). This is convenient but somehow restrictive. For example, consider when we want to edit a statement of purpose for an application. The statement contains private information and we do not want the LLMs to infer our personal information. But typically we will need several rounds of “discussion” with LLMs to make sure our statement is both literate and humane-sounding. In such cases, every time there are names, we need to manually change it to some fake ones.

If we were in Python, things will be much simpler; we can create a variable to replace the name and pass the encrypted message to the model. But how? We will call LLM APIs from Python.

```python
name = "Tony"
message = "Hello, {name}!"
# print(message.format(name=name))
```

### Preparing the Environment
- Install these modules: `langchain`, `langchain-deepseek`, `textwrap`, and `dotenv`.
- `langchain` is a framework that helps us call LLMs from Python more smoothly. `langchain-deepseek` is especially for DeepSeek APIs. `textwrap` is for better formatted output, and `dotenv` is to safely store our API keys.
- Create a DeepSeek account and log in to [https://platform.deepseek.com/usage](https://platform.deepseek.com/usage) to create an API key.
- You can use my key for this class. After class, if you need practice for this course, you can top up some money which I can reimburse for you.

### Example 7.1: Talking to DeepSeek from Python
```python
from langchain_deepseek import ChatDeepSeek
import textwrap

# https://python.langchain.com/docs/integrations/chat/deepseek/
llm = ChatDeepSeek(model="deepseek-chat")
messages = [
    ("system", "You are a helpful editor. Help me polish the application."),
    ("human", "My name is {myname}. I’m writing to apply to your university.")
]
ret_dp_msg = llm.invoke(messages)
```

Here’s a polished and professional version of your application opening:
---
**Subject:**
Application for Admission
Dear [Admissions Committee/Recipient’s Name],
My name is [Your Name], and I am writing to express my sincere interest in applying to [University Name] for [program name, if applicable]. [Optional: Add 1 – 2 sentences about why you’re drawn to the university or program —e.g., "I am..."]

## 7.2 Conversation with LLMs
Conversation differs from one-off ask-answer in that the LLMs remember the context. So we need to create a continuous flow and somewhere to store the conversation history. When we talk to LLMs, our messages contain different information:
- **System message** gives the background knowledge to the LLM.
- **Human message** is our true questions.
- **AI message** is what LLM returns to us.

### Storing Conversation History
We will store all this information in a list. Then we start a multiple-round discussion.

```python
chat_history = []

while True:
    query = input("You: ")
    if query.lower() == "quit":
        break
    chat_history.append(HumanMessage(content=query))
    result = llm.invoke(chat_history)
    response = result.content
    chat_history.append(AIMessage(content=response))
    print(f"AI: {response}")
```

## 7.3 What LLMs Are About
Based on our experiences, LLMs mostly act like a chatbot. But how can LLMs “understand” human languages?

### Recursive Neural Network Structure
- Source: [Understanding LSTMs](https://colah.github.io/posts/2015-08-Understanding-LSTMs/)

### Embedding
- The way languages (words, phrases, sentences, paragraphs, articles, even books!) are stored as numeric values is referred to as **embedding**.
- This is the most important idea in LLMs.
- Source: [Understanding LSTMs](https://colah.github.io/posts/2015-08-Understanding-LSTMs/)

## 7.4 Enhance Human Messages
As we have seen, LLMs are very large neural networks that take in embedded language inputs. LLMs’ responses are largely influenced by such messages. One issue of general LLMs is that they sometimes have hallucinations. The most famous one is the reference list. The RAG technique is introduced to try to fix this issue.

### Retrieval-Augmented Generation (RAG)
RAG aims to produce more relevant information to LLMs when we ask questions. So our prompt will look like:

```python
combined_input = (
    "Here are some documents that might help answer the question: "
    + doc_info
    + "\n\nRelevant Documents: \n"
    + "\n\n".join([doc for doc in relevant_docs])
    + "\n\nPlease provide an answer based only on the provided documents. If the answer is not found in the documents, respond with 'I'm not sure'. "
)
```

### Example 7.2: Speech Recognition
Suppose we are now interested in Chinese literature and we collect text of several classic novels. Let’s say we are reading 侠客行 and the text is on our local drive.

#### Famous Scenes of Main Character
- So we know who it is, but ChatGPT seems not...
- For this question, DeepSeek provides a better answer.

This shows that LLMs need more background information (context). We know where the book is, just how to pass it to the LLMs. Remember, although we can pass the raw text to LLMs, we do not know which text is most relevant to our questions. We need to let the computer (LLMs) decide which texts are more relevant.

```python
file_path = r"E\courses\2025S Python\data7\侠客行.txt"
db_directory = r"E\courses\2025S Python\data7\chroma_db"
loader = TextLoader(file_path, encoding="gb18030")
documents = loader.load()
text_splitter = CharacterTextSplitter(chunk_size=250, chunk_overlap=0)
docs = text_splitter.split_documents(documents)
embeddings = OpenAIEmbeddings(model="text-embedding-ada-002")  # Update to a valid embedding model if needed
```

### Storing Embedded Text
- We will use Chroma to store the embeddings and refer to the database as a vector store.
- Later on, we will find the most relevant text from the vector store.

```python
db = Chroma.from_documents(docs, embeddings, persist_directory=db_directory)
query = "谁是狗杂种"
retriever = db.as_retriever(search_type="similarity_score_threshold", search_kwargs={"k": 3, "score_threshold": 0.3})
relevant_docs = retriever.invoke(query)
```

### Wrapping and Sending to LLM
Now we have the most relevant text. Let’s wrap it in the prompt and send it to the LLM!

```python
combined_input = (
    "Here are some documents that might help answer the question: "
    + query
    + "\n\nRelevant Documents: \n"
    + "\n\n".join([doc.page_content for doc in relevant_docs])
    + "\n\nPlease provide an answer based only on the provided documents. If the answer is not found in the documents, respond with 'I'm not sure'. "
)
```

### Further Steps for RAG
- More ways to split the text
- More ways to embed our text
- More ways to retrieve relevant information
- More LLMs to RAG
- Converse with LLMs
- For further readings, I strongly recommend [this guy!](https://www.example.com/recommended-resource)

## 7.5 A Big Recall
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

### Learn More?
- A video
- A book