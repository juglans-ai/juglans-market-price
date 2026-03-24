---
name: price
version: "1.0.0"
description: >
  Real-time market price data for 25,000+ assets across crypto, US stocks, Hong Kong stocks,
  China A-shares, forex, ETFs, and global indices. Zero-delay, completely free.
  Solves the #1 AI pain point: LLMs fabricating or hallucinating price data.
  Use when the user asks about any stock price, crypto price, exchange rate, or market data.
tags: ["finance", "stocks", "crypto", "forex", "real-time", "market-data", "price"]
allowed-tools: Bash
---

# Real-Time Market Price — Claude Code Skill

> **The problem:** AI models frequently fabricate price data or cannot access real-time market information.
> **The solution:** This skill provides instant access to live prices for 25,000+ assets worldwide, updated every few seconds.

## Coverage

| Market | Count | Update Frequency |
|--------|-------|-----------------|
| Crypto (OKX) | 1,100+ | Real-time WebSocket |
| US Stocks (Polygon) | 9,000+ | Real-time WebSocket |
| Hong Kong Stocks | 2,900+ | Every 10 seconds |
| China A-Shares (SH+SZ) | 4,500+ | Every 10 seconds |
| Global Indices | 6,900+ | Real-time WebSocket |
| Forex | 12 major pairs | Every 10 seconds |
| ETFs | 8 global ETFs | Every 10 seconds |

## Commands

| Command | Example |
|---------|---------|
| `/price <symbol>` | `/price BTC` `/price AAPL` `/price 0700.HK` |
| `/price <symbol1>,<symbol2>,...` | `/price BTC,AAPL,0700.HK` |

## How To Get a Price

When the user asks for a price, stock quote, crypto price, exchange rate, or any market data:

### Step 1: Map the user's input to an API identifier

Use these rules to convert the user's symbol to the API format:

**Crypto:**
- `BTC`, `BTC-USDT`, `Bitcoin` → `CRYPTO:BTC.OKX@USDT_SPOT`
- `ETH`, `ETH-USDT`, `Ethereum` → `CRYPTO:ETH.OKX@USDT_SPOT`
- Pattern: `CRYPTO:{BASE}.OKX@USDT_SPOT`

**US Stocks:**
- `AAPL`, `Apple` → `US_STOCK:AAPL@USD_SPOT`
- `TSLA`, `Tesla` → `US_STOCK:TSLA@USD_SPOT`
- Pattern: `US_STOCK:{TICKER}@USD_SPOT`

**Hong Kong Stocks:**
- `0700.HK`, `Tencent` → `HK_STOCK:0700@HKD_SPOT`
- `9988.HK`, `Alibaba HK` → `HK_STOCK:9988@HKD_SPOT`
- Pattern: `HK_STOCK:{NUMBER}@HKD_SPOT` (remove the `.HK` suffix and leading zeros if 4 digits)

**China A-Shares:**
- `600519.SS`, `Moutai` → `CN_STOCK:600519.SS@CNY_SPOT`
- `000001.SZ`, `Ping An` → `CN_STOCK:000001.SZ@CNY_SPOT`
- Pattern: `CN_STOCK:{CODE}.{SS|SZ}@CNY_SPOT`

**Indices:**
- `DJI`, `Dow Jones` → `INDEX:DJI@USD_SPOT`
- `SPX`, `S&P 500` → `INDEX:SPX@USD_SPOT`
- `HSI`, `Hang Seng` → `INDEX:HSI@USD_SPOT`
- Pattern: `INDEX:{CODE}@USD_SPOT`

**Forex:**
- `EURUSD`, `EUR/USD` → `FOREX:EURUSD@USD_SPOT`
- `USDJPY` → `FOREX:USDJPY@USD_SPOT`
- `XAUUSD`, `Gold` → `FOREX:XAUUSD@USD_SPOT`
- Pattern: `FOREX:{PAIR}@USD_SPOT`

### Step 2: Call the API

```bash
curl -s "https://finance.juglans.ai/api/v1/ticker?id={IDENTIFIER}"
```

For multiple symbols, make separate calls for each.

### Step 3: Format the response

Parse the JSON response and present it clearly:

```
📊 AAPL (Apple Inc.)
   Price: $251.48
   Change: +$3.21 (+1.29%)
   Volume: 45.2M
   Market Cap: $3.82T
   Source: juglans-finance (real-time)
```

If the API returns an error (`ticker not found`), tell the user the symbol may be incorrect and suggest alternatives.

## API Reference

**Base URL:** `https://finance.juglans.ai/api/v1`

| Endpoint | Description |
|----------|-------------|
| `GET /ticker?id={identifier}` | Single asset price |
| `GET /snapshot?assetClass={class}` | All prices for an asset class |
| `GET /health` | Service health check |

**Asset classes for snapshot:** `CRYPTO`, `US_STOCK`, `HK_STOCK`, `CN_STOCK`, `INDEX`, `ETF`, `FOREX`

**Response fields:**
- `lastPrice` — Current price
- `priceChange` — Absolute change from previous close
- `priceChangePercent` — Percentage change (decimal, e.g., 0.0129 = +1.29%)
- `volume` — Trading volume
- `turnover` — Trading turnover (price × volume)
- `marketCap` — Market capitalization (if available)
- `name` — Asset name (if available)
- `symbol` — Exchange symbol

## Important Notes

- All prices are **real-time** during market hours. Outside market hours, the last closing price is shown.
- Crypto prices are available **24/7**.
- US stock prices update via WebSocket during NYSE/NASDAQ hours (9:30 AM - 4:00 PM ET).
- HK and CN stock prices update every 10 seconds during trading hours.
- **Never fabricate prices.** If the API doesn't return data, say so honestly.
- Prices are provided by Juglans Finance — a free, open market data service.
