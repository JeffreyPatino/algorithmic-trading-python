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
#### S&P 500 Project Overview
The S&P 500 is the world's most popular stock market index.

Many investment funds are benchmarked to the S&P 500. This means that they seek to replicate the performance of this index by owning all of the stocks that are held in the index.

One of the most important characteristics of the S&P 500 is that it is market captialization-weighted.

This means that larger companies get a correspondingly larger weight in the index.

In the first project of this course, we will build an alternative version of the S&P 500 Index Fund where each company has the same weighting.

##### My Notes:
###### Requirements.txt Version Compatibility
The original requirements.txt contained outdated package versions that are incompatible with modern Python installations:

**Problem**: 
- `numpy==1.17.4` failed to install due to deprecated build system
- Old versions lack compatibility with Python 3.9+

**Solution**:
Updated problematic packages to use minimum compatible versions:
```
numpy>=1.21.0 # was 1.17.4
pandas>=1.3.0 # was 0.25.3
scipy>=1.7.0 # was 1.5.2
```

###### Data provider choice
- IEX Cloud was closed; reference: https://iexcloud.org/.
- Initially tried Alpha Vantage, but hit free-tier rate limiting (25 requests/day).
- Switched to Finnhub Stock API, which works well for this project.

Details:
- Endpoints in use:
  - Quote: https://finnhub.io/api/v1/quote?symbol={SYMBOL}&token=...
  - Profile/market cap: https://finnhub.io/api/v1/stock/profile2?symbol={SYMBOL}&token=...
- Finnhub free tier doesn’t support batch quotes; iterating with a small sleep (e.g., 1s) avoids rate issues.

###### Pandas DataFrame.append() deprecation
- The code originally used `DataFrame.append()` to add rows to a DataFrame.
- In recent pandas versions, `.append()` has been removed and will raise an `AttributeError`.
- Solution: Use `pd.concat([df, new_row], ignore_index=True)` instead, where `new_row` is a DataFrame or a Series converted to a DataFrame with `.to_frame().T`.

###### Incident: NumPy ImportError due to stdlib module shadowing
Symptoms
- ImportError: cannot import name randbits (during `import numpy`)
- Traceback points into `numpy.random.bit_generator` and `mtrand`

Root Cause
- A non-standard `secrets.py` in the Python path shadowed the standard library `secrets` module.
- NumPy’s random subsystem indirectly relies on stdlib modules (e.g., `secrets` / `random`). When these are shadowed or overwritten, NumPy import breaks.

Fix
- Rename the custom secrets file to avoid name conflicts.
- Restore stdlib `secrets.py` if it was overwritten (use system Python/Conda repair or copy from CPython source).
- Reinstall clean, compatible NumPy/Pandas wheels if needed, then restart the notebook kernel.

Prevention
- Do not name project modules after stdlib modules (e.g., `secrets.py`, `random.py`, `typing.py`, etc.).
- Keep sensitive config in `.env` or a clearly named module (e.g., `app_secrets.py`), and add it to `.gitignore`.
- Verify imports point to expected files:
  ```
  python -c "import secrets; print(secrets.__file__)"
  ```

References
- NumPy issue: https://github.com/numpy/numpy/issues/14860
- CPython `secrets.py` (example): https://github.com/python/cpython/blob/3.7/Lib/secrets.py

###### API Rate Limiting
- The Finnhub Stock API free tier has a rate limit of 60 requests per minute. Reference: [Finnhub Pricing](https://finnhub.io/pricing).
- To avoid exceeding this limit, a 1-second delay (`time.sleep(1)`) was added between API calls when iterating through the list of stocks.
- This ensures compliance with the rate limit and prevents the API from returning errors due to excessive requests.

### 4. Project 2: Quantitative Momentum Screener
Momentum investing means investing in assets that have increased in price the most.

To help understand, let's consider an example.

Imagine that you have the choice between investing in two stocks that have had the following returns over the last year:
- Apple (AAPL): 35%
- Microsoft (MSFT): 20%

A momentum investing strategy would suggest investing in Apple because of its higher recent price return.

There are many other nuances to momentum investing strategies that we will explore throughout this course.

# LEFT OFF AT 1:38:42

### 5. Project 3: Quantitative Value Screener
Value investing means investing in stocks that are trading below their perceived intrinsic value.

Value investing was popularized by investors like Warren Buffet, Seth Klarman, and Benjamin Graham.

Creating algorithmic value investing strategies relies on a concept called multiples.

Here are a few examples of common multiples used in value investing:
- Price-to-earnings
- Price-to-book-value
- Price-to-free-cash-flow

Each of the individual multiples used by value investors has its pros and cons.

One way to minimize the impact of any specific multiple is by using a composite.

We'll use a composite of 5 different valuation metrics in our strategy.

