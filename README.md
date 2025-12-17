# CN-MP1
!pip install requests pandas openpyxl

import requests
import pandas as pd

API_KEY = "Your-Api-Key"

url = "https://pro-api.coinmarketcap.com/v1/cryptocurrency/listings/latest"

headers = {
    "X-CMC_PRO_API_KEY": API_KEY,
    "Accepts": "application/json"
}

params = {
    "start": "1",
    "limit": "54",   # same as your Excel image
    "convert": "USD"
}

response = requests.get(url, headers=headers, params=params)
data = response.json()

rows = []

for i, coin in enumerate(data["data"], start=1):
    rows.append({
        "Serial No": i,
        "Name": coin["name"],
        "Symbol": coin["symbol"],
        "Price (USD)": coin["quote"]["USD"]["price"],
        "Market Cap": coin["quote"]["USD"]["market_cap"],
        "24h Volume": coin["quote"]["USD"]["volume_24h"],
        "Change 24h (%)": coin["quote"]["USD"]["percent_change_24h"],
        "Circulating Supply": coin["circulating_supply"],
        "Max Supply": coin["max_supply"]
    })

df = pd.DataFrame(rows)

# ✅ EXCEL OUTPUT ONLY
df.to_excel("/content/Crypto_Market_Analyzer_Output.xlsx", index=False)
