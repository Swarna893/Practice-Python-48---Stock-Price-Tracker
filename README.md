# Practice-Python-46---Stock-Price-Tracker

import requests
import time

ALPHA_VANTAGE_API_KEY = 'YOUR_API_KEY'  # Replace with your Alpha Vantage API key


def get_stock_price(symbol):
    base_url = 'https://www.alphavantage.co/query'
    function = 'GLOBAL_QUOTE'
    params = {
        'function': function,
        'symbol': symbol,
        'apikey': ALPHA_VANTAGE_API_KEY,
    }

    response = requests.get(base_url, params=params)
    data = response.json()

    if 'Global Quote' in data:
        return float(data['Global Quote']['05. price'])
    else:
        return None


def price_alert(symbol, target_price):
    while True:
        current_price = get_stock_price(symbol)
        if current_price is not None:
            print(f"Current {symbol} Price: ${current_price:.2f}")

            if current_price <= target_price:
                print(f"Alert: {symbol} price is below ${target_price:.2f}!")
                break

        time.sleep(60)  # Check the price every 60 seconds


if __name__ == "__main__":
    stock_symbol = input("Enter the stock symbol (e.g., AAPL): ").upper()
    target_price = float(input("Enter the target price for the alert: $"))

    print(f"\nMonitoring {stock_symbol} for price alerts...")
    price_alert(stock_symbol, target_price)
