# juglans-market-price

Real-time market price skill for Claude Code. Get accurate, live prices for 25,000+ assets worldwide.

## The Problem

AI models frequently **fabricate price data** or cannot access real-time market information. Ask any LLM "What's the price of Bitcoin?" and you'll likely get outdated or hallucinated numbers.

## The Solution

This skill gives Claude Code instant access to **live market prices** — updated every few seconds, completely free.

### Coverage

- **Crypto:** 1,100+ tokens (OKX WebSocket, real-time)
- **US Stocks:** 9,000+ tickers (Polygon WebSocket, real-time)
- **Hong Kong Stocks:** 2,900+ (FMP, 10s interval)
- **China A-Shares:** 4,500+ (FMP, 10s interval)
- **Global Indices:** 6,900+ (Polygon + FMP)
- **Forex:** 12 major pairs
- **ETFs:** Global ETFs

## Install

### One-line install

```bash
curl -fsSL https://raw.githubusercontent.com/juglans-ai/juglans-market-price/main/install.sh | bash
```

### Manual install

```bash
mkdir -p ~/.claude/skills/market-price
curl -fsSL https://raw.githubusercontent.com/juglans-ai/juglans-market-price/main/SKILL.md \
  -o ~/.claude/skills/market-price/SKILL.md
```

### ClawHub

```bash
clawhub install market-price
```

## Usage

After installing, restart Claude Code and use:

```
/price BTC
/price AAPL
/price 0700.HK
/price 600519.SS
/price EURUSD
/price BTC,AAPL,TSLA
```

Or just ask naturally:

> "What's the current price of Tesla?"
> "How much is Bitcoin right now?"
> "Show me Tencent's stock price"

## API

This skill uses the free [Juglans Finance API](https://finance.juglans.ai):

```bash
# Single price
curl "https://finance.juglans.ai/api/v1/ticker?id=CRYPTO:BTC.OKX@USDT_SPOT"

# All crypto prices
curl "https://finance.juglans.ai/api/v1/snapshot?assetClass=CRYPTO"
```

No API key required. No rate limits for normal usage.

## License

MIT
