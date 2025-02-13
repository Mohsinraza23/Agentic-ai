# Agentic-ai

# Agentic AI Series - Class 01

## Introduction
Welcome to the first class of the **Agentic AI Series**! 🎉 In this session, we will focus on setting up your environment for building AI applications, including installing the necessary dependencies and configuring your project to interact with APIs such as **phidata** and **groq**. By the end of this class, you'll have the foundational setup ready to start working with advanced AI tools.

---

## Step 1: Creating the Environment
To get started, we'll create a **Python environment** using Conda. Run the following command to create the environment with **Python version 3.13.1**:

```sh
conda create -p venv python==3.13.1
```

When prompted, type **Yes** to proceed with the creation of the environment.

Once the environment is created, activate it using the following command:

```sh
conda activate venv
```

---

## Step 2: Creating the `requirements.txt` File
Next, we'll create a `requirements.txt` file to list all the dependencies for our project. Here’s what to include in your `requirements.txt`, with a brief explanation for each library:

```
phidata            # A Python library for working with phidata APIs
python-dotenv      # Loads environment variables from a .env file
yfinance           # Provides access to Yahoo Finance data
packaging          # A tool for packaging Python code
duckduckgo-search  # Allows search queries via DuckDuckGo
fastapi            # A web framework for building APIs quickly
uvicorn            # ASGI server for FastAPI applications
groq               # A library to interact with the groq API
openai             # Python client for OpenAI's GPT API
```

Now, install the dependencies by running:

```sh
pip install -r requirements.txt
```

---

## Step 3: Setting Up Environment Variables
We are using **python-dotenv** to manage environment variables. In your project directory, create a `.env` file that contains the API keys for **phidata** and **groq**.

```
PHI_API_KEY="your_phidata_api_key"
GROQ_API_KEY="your_groq_api_key"
```

Make sure to replace the empty strings with your actual API keys. You can get the API keys by signing up on the respective platforms.

---

## 🎯 That's It for Today!
With the environment set up, dependencies installed, and API keys configured, you're ready to move forward in the **Agentic AI Series**. In the next class, we’ll dive deeper into interacting with these APIs and building a simple application.

### 📌 Notes:
- 🔒 **Make sure to keep your `.env` file safe** and never share it publicly.
- ❗ If you encounter any issues while setting up the environment or installing dependencies, **double-check the Python version and the installation steps**.

Happy coding, and see you in the next class! 🚀😊




# Agentic AI Series - Class 02

## Introduction
Welcome to Class 02 of the Agentic AI Series! In this session, we will focus on creating and configuring agents for financial and web search tasks. We will also integrate these agents with the Phi Data Platform and set up a playground to test and interact with them. By the end of this class, you'll have a fully functional AI application running locally and integrated with external APIs.

---

## Step 1: Creating `financial_agent.py`
Create a new file named `financial_agent.py` and add the following code:

```python
# Libraries
from phi.agent import Agent
from phi.model.groq import Groq
from phi.tools.yfinance import YFinanceTools
from phi.tools.duckduckgo import DuckDuckGo

# Web Search Agent
web_search_agent = Agent(
    name="Web Search Agent",
    role="Search the web for the information",
    model=Groq(id="llama3-groq-70b-8192-tool-use-preview"),
    tools=[DuckDuckGo()],
    instructions=["Always include sources"],
    show_tools_calls=True,
    markdown=True,
)

# Financial Agent
finance_agent = Agent(
    name="Finance AI Agent",
    model=Groq(id="llama3-groq-70b-8192-tool-use-preview"),
    tools=[
        YFinanceTools(stock_price=True, analyst_recommendations=True, stock_fundamentals=True,
                      company_news=True),
    ],
    instructions=["Use tables to display the data"],
    show_tool_calls=True,
    markdown=True,
)

# Multi Modal Agent
multi_ai_agent = Agent(
    model=Groq(id="llama-3.1-70b-versatile"),
    team=[web_search_agent, finance_agent],
    instructions=["Always include sources", "Use tables to display the data"],
    show_tool_calls=True,
    markdown=True,
)

multi_ai_agent.print_response("Summarize analyst recommendations and share the latest news for NVDA", stream=True)
```

### Running the Agent
To run `financial_agent.py`, enter the following command in the terminal:

```sh
python financial_agent.py
```

### Output
```
Thinking -
Summarizing
Response -
Response
Latest News -
Latest-News
```

---

## Step 2: Integrating with Phi Data Platform
Create another file named `playground.py` with the following code:

```python
import openai
from phi.agent import Agent
import phi.api
from phi.model.openai import OpenAIChat
from phi.tools.yfinance import YFinanceTools
from phi.tools.duckduckgo import DuckDuckGo
from dotenv import load_dotenv
from phi.model.groq import Groq
import os
from phi.playground import Playground, serve_playground_app

# Load environment variables
load_dotenv()
phi.api = os.getenv("PHI_API_KEY")

# Web Search Agent
web_search_agent = Agent(
    name="Web Search Agent",
    role="Search the web for the information",
    model=Groq(id="llama3-groq-70b-8192-tool-use-preview"),
    tools=[DuckDuckGo()],
    instructions=["Always include sources"],
    show_tools_calls=True,
    markdown=True,
)

# Financial Agent
finance_agent = Agent(
    name="Finance AI Agent",
    model=Groq(id="llama3-groq-70b-8192-tool-use-preview"),
    tools=[
        YFinanceTools(stock_price=True, analyst_recommendations=True, stock_fundamentals=True,
                      company_news=True),
    ],
    instructions=["Use tables to display the data"],
    show_tool_calls=True,
    markdown=True,
)

# Playground
app = Playground(agents=[finance_agent, web_search_agent]).get_app()

if __name__ == "__main__":
    serve_playground_app("playground:app", reload=True)
```

### Running the Agent
To run `playground.py`, enter the following command in the terminal:

```sh
python playground.py
```

### Output
```
Response After IWPDP -
Response-After-IWPDP
```

---

## Step 3: Accessing the Playground
Once the playground is running, it will be hosted on `localhost:7777`. If it doesn't work, follow these steps:

1. Navigate to the Phi Data dashboard.
2. Go to **Playground > Select an Endpoint**.
3. Select the **localhost endpoint** and ensure it shows a green signal.
4. Test the application by making queries.

---

## Class Summary
By the end of this class, you will have:

✅ Created agents for financial and web search tasks.
✅ Integrated these agents with the Phi Data Platform.
✅ Tested your application using the Phi Playground.

---

## Notes
- Ensure all dependencies from **Class 01** are installed.
- Keep API keys **secure** and **never share them publicly**.

🚀 **Happy coding! See you in the next class! 😊**
