# 📌 AI Agent Tools

## 1. Introduction: What Are AI Tools?
AI agents use **tools** to take actions beyond their internal knowledge. A **Tool** is a function provided to the LLM to fulfill a **clear objective**.

### 🔹 Commonly Used AI Tools
| **Tool**           | **Description** |
|-------------------|---------------|
| **Web Search**    | Fetches up-to-date information from the internet. |
| **Image Generation** | Creates images based on text descriptions. |
| **Retrieval**     | Retrieves information from an external source. |
| **API Interface** | Interacts with an external API (GitHub, YouTube, Spotify, etc.). |

📌 **A good tool should complement the power of an LLM rather than replace it.**  
📌 **If an agent needs up-to-date data, it must obtain it via a tool.**

### 🔹 A Tool Should Contain:
1. **A textual description** of what the function does.
2. **A callable function** (performs an action).
3. **Arguments** with proper typing.
4. *(Optional)* **Outputs** with proper typing.

---

## 2. How Do Tools Work?
LLMs **only process text**—they cannot call tools themselves. Instead, they **generate text-based commands** that an **Agent** interprets and executes.

### 🔹 How an LLM Uses a Tool:
1. The LLM detects when a tool is required.
2. It generates **text instructions** to invoke the tool.
3. The **Agent parses the LLM’s output** and recognizes a tool call.
4. The Agent **executes the tool function** and retrieves the output.
5. The result is **passed back to the LLM**, which generates the final response.

📌 **The tool call is hidden from the user—the Agent processes everything behind the scenes.**

### 🔹 Example: Weather Tool Call
1️⃣ **User asks:** "What’s the weather in Paris?"  
2️⃣ **LLM generates text:** `CALL weather_tool("Paris")`  
3️⃣ **Agent executes the tool:** Fetches weather data via an API.  
4️⃣ **LLM responds:** "The weather in Paris is 18°C and sunny."

---

## 3. How Do We Give Tools to an LLM?
Tools are **described in the system prompt**.  

### 🔹 To define a tool, we must specify:
- **What the tool does**
- **What exact inputs it expects**  

Tools are typically provided using structured formats such as **JSON** or programming languages.

### 🔹 Example: Defining a Simple Calculator Tool
We will implement a basic **multiplication tool** in Python:
```python
def calculator(a: int, b: int) -> int:
    """Multiply two integers."""
    return a * b
```

### ✅ Tool Breakdown:

- Name: "calculator"
- Description: "Multiply two integers."
- Arguments:
a (int): First number
b (int): Second number
- Output: (int): The product of a and b

### 📌 The textual description is what we provide to the LLM so it knows how to use the tool.

---

### 3.1 Auto-Formatting Tool Descriptions
Since tools are written in Python, we can automatically generate their descriptions by using function introspection.

#### 🔹 Using a Python Decorator for Tools
Instead of manually describing each tool, we use a @tool decorator that extracts metadata:
```
@tool
def calculator(a: int, b: int) -> int:
    """Multiply two integers."""
    return a * b
```
#### 🔹 Auto-Generated Tool Description:

```
Tool Name: calculator, Description: Multiply two integers., Arguments: a: int, b: int, Outputs: int
```
#### 📌 Automating tool descriptions ensures LLMs receive structured and consistent specifications.
---

### 3.2 Generic Tool Implementation
To create a reusable tool system, we define a Tool class:

```
class Tool:
    """
    A class representing a reusable piece of code (Tool).

    Attributes:
        name (str): Name of the tool.
        description (str): A textual description of what the tool does.
        func (callable): The function this tool wraps.
        arguments (list): A list of arguments.
        outputs (str or list): The return type(s) of the wrapped function.
    """
    def __init__(self, name, description, func, arguments, outputs):
        self.name = name
        self.description = description
        self.func = func
        self.arguments = arguments
        self.outputs = outputs

    def to_string(self):
        """
        Return a string representation of the tool, including its metadata.
        """
        args_str = ", ".join([f"{arg_name}: {arg_type}" for arg_name, arg_type in self.arguments])
        return f"Tool Name: {self.name}, Description: {self.description}, Arguments: {args_str}, Outputs: {self.outputs}"

    def __call__(self, *args, **kwargs):
        return self.func(*args, **kwargs)
```

#### 🔹 Creating a Tool Instance

```
calculator_tool = Tool(
    "calculator",
    "Multiply two integers.",
    calculator,
    [("a", "int"), ("b", "int")],
    "int",
)
```
#### 📌 This makes tool management modular and reusable across different applications.
---
#### 3.2.1 Automating Tool Extraction Using Inspect
To further automate tool descriptions, we use Python’s inspect module:

```
import inspect

def tool(func):
    """
    A decorator that extracts function metadata and creates a Tool instance.
    """
    signature = inspect.signature(func)
    arguments = [(param.name, str(param.annotation)) for param in signature.parameters.values()]
    return_annotation = str(signature.return_annotation) if signature.return_annotation != inspect._empty else "No return annotation"
    
    return Tool(
        name=func.__name__,
        description=func.__doc__ or "No description provided.",
        func=func,
        arguments=arguments,
        outputs=return_annotation
    )
```

##### ✅ Benefits:

- Extracts function metadata dynamically.
- Ensures consistent tool descriptions.
- Supports easy expansion of tool libraries.

##### 🔹 Using the Decorator

```
@tool
def calculator(a: int, b: int) -> int:
    """Multiply two integers."""
    return a * b

print(calculator.to_string())
```
##### 📌 Now the tool’s specification is automatically extracted without needing manual updates.
---

## 📌 Key Takeaways (3-Sentence Summary)
### **1️⃣ AI Tools allow LLMs to perform external actions, such as fetching data or executing functions.**
### **2️⃣ Tools must be well-defined with a name, description, expected inputs, and outputs to ensure LLMs can invoke them correctly.**
### **3️⃣ Automating tool descriptions using decorators and introspection simplifies tool integration and maintains consistency.**
---
## 📌 If an Interviewer Asks...
### ❓ Q1: Why do AI agents need tools?
#### ✅ Answer: AI agents rely on tools to overcome knowledge limitations, fetch real-time data, and execute specialized functions beyond their training scope.

### ❓ Q2: How does an LLM call a tool?
#### ✅ Answer: The LLM generates text commands indicating a tool call, which the Agent interprets and executes, then feeds the result back to the model.

### ❓ Q3: Why is automating tool descriptions important?
#### ✅ Answer: Automating descriptions ensures consistency, reduces manual errors, and simplifies LLM integration by providing structured, machine-readable tool specifications.

