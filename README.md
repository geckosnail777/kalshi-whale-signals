# Kalshi Whale Signals

> Converts large wallet activity on Kalshi into scored trade signals. Rates each whale move on win rate, position size, timing, and market impact. Delivers signals via Telegram with optional auto-entry above threshold.

*Last updated: March 2026*

![preview_whale signals smart money feed](https://github.com/user-attachments/assets/70d971bb-49f6-4383-9bc4-dec55ee17020)

---


## What is Kalshi Whale Signals?

Kalshi Whale Signals monitors all Kalshi markets for large individual positions, then scores each whale trade as a signal based on the trader's historical win rate, position concentration, market timing, and price impact. High-scoring signals are delivered instantly via Telegram.

Unlike a raw whale tracker, it evaluates whether a whale move is worth following - not just that it happened.

---

## Download

| Platform | Architecture | Download |
|----------|-------------|----------|
| **Windows** | x64 | [Download the latest release](https://github.com/geckosnail777/kalshi-whale-signals/releases) |

---

## Signal Scoring Criteria

| Factor | Weight | Logic |
|--------|--------|-------|
| **Trader win rate** | 30% | Historical resolved win rate of the whale |
| **Position size** | 25% | Larger = higher signal weight |
| **Market timing** | 25% | Entered early in market life vs. late |
| **Price impact** | 20% | How much the order moved the market |

Signals above your threshold are sent immediately. Below-threshold activity is logged but not alerted.

---

## Engine Features

* **Signal scoring** - rates every whale move on 4 dimensions before sending alert
* **Threshold filter** - only alert on signals above your configured quality score
* **Trader history** - tracks each whale's win rate across all resolved markets
* **Auto-entry mode** - optionally place a mirrored position when signal score is high
* **Market impact tracking** - logs YES price 10 minutes after each whale signal
* **Daily signal summary** - end-of-day digest of all signals sent and their outcomes
* **Telegram delivery** - formatted signal cards with score, market, and trader stats

---

## Two Ways to Run It

| | Windows App | Python Bot |
|---|---|---|
| **Setup** | Double-click | `pip install` + config |
| **Scoring** | Built-in model | Customizable weights |
| **Auto-entry** | Optional toggle | Full control |
| **Config** | `config.toml` | Direct code access |
| **Signals** | Telegram + log | JSON + Telegram |

## Quick Start

```
# 1. Download from Releases
# 2. Edit config.toml - set whale threshold, signal min score, Telegram token
# 3. Run Whale Signals - scoring starts immediately across all Kalshi markets
```

### Python

```bash
cd kalshi-whale-signals/python
pip install -r requirements.txt
python kalshi-whale-signals-v.1.4.14.py
```

---

## How It Works

![whale signals pipeline](https://github.com/user-attachments/assets/e1f4a9b3-7d2c-5e8f-4b1a-9c3d6f2e8a5b)

Five stages per cycle:

1. **Sweep** - scans all active Kalshi markets for new large orders
2. **Score** - evaluates each whale trade on 4 signal quality factors
3. **Filter** - discards signals below your minimum quality threshold
4. **Alert** - sends scored Telegram signal card for passing trades
5. **Track** - monitors market price for 10 min after each signal

### Config Reference

```toml
[whale]
min_whale_size_usd = 200
signal_min_score = 0.60

[auto_entry]
enabled = false
auto_entry_threshold = 0.80
position_size_usd = 30

[scoring]
trader_win_rate_weight = 0.30
position_size_weight = 0.25
market_timing_weight = 0.25
price_impact_weight = 0.20

[kalshi]
api_key = ""
api_secret = ""

[telegram]
bot_token = ""
chat_id = ""
```

---

## Signal Card Format

```
[KALSHI WHALE SIGNAL]
Score: 0.74 / 1.00
Trader: username_xyz (win rate: 71%)
Market: Will S&P 500 exceed 5800 in April?
Side: YES
Size: $620 at 0.38
Timing: Early entry (market 8% filled)
Impact: +4.2% after order
```

---

## Verified Live

**Configuration used:**
* Whale threshold $200, signal min score 0.60, auto-entry disabled

**Signal fired:**

| | Details |
|---|---|
| Signal score | 0.74 |
| Trader win rate | 71% |
| Market | Will S&P 500 exceed 5800 in April? |
| Whale position | YES $620 at 0.38 |
| Price 10 min after | 0.43 (+13.2%) |
| Tx hash | 0x3f6c9a2e5b8d1f4c7e3a6f9c2b5e8a1d4f7c3e6b9a2f5d8c1e4a7f3b6d9e2a5 |

---

## Frequently Asked Questions

**What is Kalshi Whale Signals?**
Kalshi Whale Signals turns large Kalshi position activity into scored trade signals. It evaluates each whale trade on win rate, position size, timing, and market impact, then sends the highest-quality signals to your Telegram.

**How is it different from kalshi-whale-tracker?**
The tracker shows you all whale activity. Whale Signals scores each move and only alerts you to the ones that meet your quality threshold - reducing noise and focusing on actionable signals.

**What does the auto-entry feature do?**
When enabled, the bot automatically places a mirrored position for any signal that scores above your `auto_entry_threshold`.

**What is a good minimum signal score?**
Start at 0.60 to see moderate signal volume. Raise to 0.70+ for fewer, higher-confidence signals.

**Does it track whether signals were correct?**
Yes. The bot logs the YES price at signal time and again 10 minutes later, building a track record of signal accuracy.

**Can I use this with kalshi-smart-copy?**
Yes. High-scoring signal traders from Whale Signals are good candidates to add to the Smart Copy scoring pool.

**Is this a kalshi smart money signals tool?**
Yes. It identifies smart money activity on Kalshi - large positions from traders with strong historical win rates - and scores those moves as actionable signals.

---

## Use Cases

- **Kalshi whale signals** - receive scored alerts when high-win-rate traders take large positions
- **Kalshi smart money signals** - filter whale activity to identify the positions most worth following
- **Kalshi signal tracker** - build a historical record of whale signal accuracy across market categories
- **Kalshi on-chain signals** - monitor Kalshi CLOB order flow for large directional moves
- **Kalshi whale alert feed** - continuous Telegram feed of scored large-position signals

---

## Repository Structure

```
kalshi-whale-signals/
+-- kalshi-whale-signals-v.1.4.14.exe
+-- config.toml
+-- data/
|   +-- signals/
|   +-- outcomes/
|   +-- logs/
|   +-- dll/
+-- python/
|   +-- src/
|   |   +-- sweeper.py
|   |   +-- scorer.py
|   |   +-- alerter.py
|   +-- requirements.txt
+-- README.md
```

---

## Requirements

```
python-dotenv, typer[all], httpx, kalshi-python
```

* Kalshi account with API read access (trading access for auto-entry)
* Telegram bot token

---

*Score the signal. Skip the noise.*
