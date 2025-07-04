Financial and Investment Analyst Chatbot
This repository contains the n8n workflows for a personal project: a Financial and Investment Analyst Chatbot. This chatbot is designed to provide users with up-to-date financial news, fundamental company data, and stock price movements, all powered by AI and integrated with a popular messaging platform.

Project Overview
The project consists of two main n8n workflows:

1.Summer_chatbot.json: This is the core chatbot workflow. It acts as the user-facing interface, integrating with a Line Official Account. It leverages the power of Google Gemini 2.5 Flash for AI capabilities, enabling natural language understanding and generation for financial queries. The AI agent is equipped with "tools" to access various data sources.

2.Summer_news_acquisition.json: This is the data acquisition and processing workflow. It's responsible for pulling raw financial data from external APIs and storing it in a structured format within a MongoDB database, which then serves as the data source for the chatbot.

Features
The Financial and Investment Analyst Chatbot can:

1.Provide Investment News Analysis: Users can ask about recent news related to investing. The chatbot will summarize relevant articles concisely and interpret their potential impact on financial markets, specific industries, or investment decisions.

2.Analyze Price Movements: Get insights into stock price movements for specific tickers.

3.Retrieve Company Fundamental Data: Access key fundamental data for companies, providing a deeper understanding of their financial health and valuation.

Data Sources
The Summer_news_acquisition flow utilizes multiple external APIs to gather comprehensive financial data:

1.EOD Historical Data (EODHD): Used for pulling news, fundamental data, and real-time stock prices. Please note that the API key used in the provided workflow is a demo key, which means the data retrieved will be limited.

2.Alpha Vantage: Used for acquiring daily time series stock price data.

All acquired data is then stored and managed in a MongoDB database, which acts as the central data repository for the AI chatbot's tools.

The three main data categories stored are:

1.News: Summarized news articles with their impact analysis.

2.Fundamental Data: Key financial metrics and company information.

3.Stock Price Data: Historical and recent stock price movements by ticker.

How It Works
Summer_chatbot.json (Chatbot Flow)
Webhook Trigger: Listens for incoming messages from the Line Official Account.

AI Agent (Google Gemini 2.5 Flash): Processes the user's query, understands the intent, and decides which "tool" (MongoDB database lookup) to use.

MongoDB Tools: The AI agent has access to three MongoDB tools:

get news: Retrieves news data.

get fundamental: Retrieves fundamental company data.

get ticker: Retrieves stock price data.

HTTP Request (Line API): Sends the AI-generated response back to the user on the Line Official Account.

Summer_news_acquisition.json (Data Acquisition Flow)
Manual Trigger: Can be executed manually or scheduled to run periodically.

Edit Fields / Split Out: Prepares a list of stock symbols for API calls.

HTTP Request (EODHD & Alpha Vantage): Makes API calls to fetch news, fundamental data, and daily stock prices for specified symbols.

Code Node: Transforms the raw API responses into a more usable format, including merging symbol and date into a unique symbol_date_id for efficient database storage and retrieval. It also ensures only the 7 most recent daily stock prices are processed.

MongoDB Nodes: Stores the processed news, fundamental, and ticker data into respective collections within the MongoDB database (Summer_news, Summer_fundamental, Summer_ticker). Each daily stock price update uses symbol_date_id as a composite update key to prevent overwriting.

Setup and Deployment (n8n)
To set up and run this project:

n8n Instance: You need a running n8n instance (self-hosted or cloud).

Download Workflows: Download Summer_chatbot.json and Summer_news_acquisition.json from this repository.

Import Workflows: Import both JSON files into your n8n instance.

Credentials:

Line Official Account: Configure your Line Messaging API credentials in the Summer_chatbot workflow's HTTP Request node. Note: The Line bearer token has been removed from the provided workflow for security reasons.

Google Gemini API: Set up your Google Gemini API key in the "Google Gemini Chat Model" node.

MongoDB: Configure your MongoDB connection credentials in both workflows. Ensure the database name and collection names (Summer_news, Summer_fundamental, Summer_ticker) match your setup.

EODHD & Alpha Vantage API Keys: The Alpha Vantage API key has been removed from the provided workflow. You can easily obtain your own free API key from https://www.alphavantage.co/support/#api-key. Replace the placeholder API keys in the Summer_news_acquisition workflow's HTTP Request nodes with your actual API keys. Be mindful of API rate limits, especially for free tiers.

Line Webhook: Configure the webhook in your Line Official Account to point to the webhook URL provided by the Webhook node in Summer_chatbot.

Activate Workflows: Activate both workflows in n8n.

Schedule Data Acquisition: For Summer_news_acquisition, consider setting up a schedule trigger (e.g., daily) to keep your data up-to-date.

Important Notes on API Limits
Alpha Vantage Free Tier: The free tier has strict rate limits (e.g., 5 requests per minute, 500 requests per day). If you encounter "API limit" errors during data acquisition (in Summer_news_acquisition), consider adding a "Wait" node (1-2 seconds) after the HTTP Request nodes to space out your API calls. The AI Agent in the chatbot flow relies on the data being successfully acquired and stored, so ensuring the acquisition flow runs without hitting limits is crucial.

EODHD: Be aware of EODHD's rate limits for their free/demo keys as well.

Feel free to explore, modify, and enhance this project!
