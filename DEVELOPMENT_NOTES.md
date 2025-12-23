# Development Notes - Algorithmic Trading using Python

## Course Overview

### 1. Algorithmic Trading Basics

#### Algoritmic Trading Overview

Algorithmic trading means using computers to make investment decisions.

There are many different types of algorithmic trading. The main difference is their speed of execution.

#### The Algorithmic Trading Landscape
Here are some of the main players in the algorithmic trading landscape:
- Renaissance Technologies: $165B in AUM
- AQR Capital Management: $61B in AUM
- Citadel Securities: $32B in AUM

#### Algoritmic Trading & Python
Python is the most popular programming language for algorithmic trading.

However, Python is slow. This means that it is often used as a "glue language" to trigger code that runs in other languages.

One example of this is the NumPy library for Python, which we'll be using in this course.

NumPy is the most popular Python library for performing numerical computing.

Although it's written for using Python, the core underlying funcationality is written in C, a much faster language.

#### The Algorithmic Trading Process
The process of running a quantitative investing strategy can be broken down into the following steps:
1. Collect data
2. Develop a hypothesis for a strategy
3. Backtest that strategy
4. Implement the stategy in production

#### How This Course Will Be Different than Algorithmic Trading in Production
Because this is an introductory course, it will differ from production algorithmic trading in 3 major ways:
1. We'll be using random data
2. We will not be executing trades

### 2. API Basics and Course Configuration
#### What is an API?
An API is an Application Programming Interface.
APIs allow you to interact with someone else's software using your own code.

In this course, we'll be using the IEX Cloud API to gather stock market data to make investment decisions.

Here's an example of what a call to that API would look like:
```python
symbol='AAPL'
api_url= f'https://sandbox.iexapis.com/stable/stock/{symbol}/quote?token{IEX_CLOUD_API_TOKEN}'
data = requests.get(api_url).json()
data
```

#### Other API Functionality
In this course, we will be using GET requests to gather data from the IEX Cloud API.

With that said, there are many other ways to interact with an API.

Let's discuss them briefly before proceeding.

- POST: Adds data to the database exposed by the API. (Create only)
- PUT: Adds <ins>and overwrites</ins> data in the database exposed by the API. (Create or replace)
- DELETE: Deletes data from the API's database

### 3. Project 1: Equal-Weight S&P 500 Screener

### 4. Project 2: Quantitative Momentum Screener

### 5. Project 3: Quantitative Value Screener
