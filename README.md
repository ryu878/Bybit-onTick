# Bybit onTick
Bybit onTick - MQL5-Style Trade Stream Handler

A Python implementation that replicates MQL5's `onTick()` functionality for Bybit cryptocurrency exchange using WebSocket trade streams. This library provides real-time trade processing with automatic reconnection and robust error handling.

## Features

- **MQL5-Style onTick Function**: Each individual trade triggers your callback function, mimicking MQL5's familiar `onTick()` behavior
- **Multi-Symbol Support**: Monitor multiple trading pairs simultaneously
- **Real-Time Trade Processing**: Process live trade data including price, volume, side, and timestamp
- **Automatic Reconnection**: Built-in connection recovery and error handling
- **Flexible Configuration**: Support for spot, linear futures, and other Bybit trading categories
- **Lightweight & Fast**: Built on top of the official `pybit` library for optimal performance
- **Easy Integration**: Simple callback-based architecture for seamless integration into existing trading systems

## Installation

### Prerequisites

- Python 3.7 or higher
- Active internet connection for WebSocket streams

### Install Dependencies

```
pip install pybit
```

### Clone Repository

```
git clone https://github.com/ryu878/Bybit-onTick.git
cd bybit-ontick
```
## Quick Start

### Basic Usage

```
from bybit_ontick import BybitOnTick

# Initialize the handler
handler = BybitOnTick(
    symbols=["BTCUSDT", "ETHUSDT"],
    category="spot",  # or "linear" for futures
    testnet=False
)

# Define your trading logic
def my_trading_logic(trade_data):
    symbol = trade_data['s']
    price = float(trade_data['p'])
    size = float(trade_data['v'])
    side = trade_data['S']
    
    print(f"[{symbol}] {side} {size} @ {price}")
    
    # Your trading strategy here
    if side == "Buy" and price > some_threshold:
        # Execute your trading logic
        pass

# Start listening
handler.start_listening(callback=my_trading_logic)

```

### Advanced Usage with State Management
```
from bybit_ontick import BybitOnTick
import redis

# Initialize Redis for state management
redis_client = redis.Redis(host='localhost', port=6379, db=0)

class TradingBot:
    def __init__(self):
        self.last_prices = {}
        self.trade_counts = {}
        
    def on_tick(self, trade_data):
        symbol = trade_data['s']
        price = float(trade_data['p'])
        side = trade_data['S']
        volume = price * float(trade_data['v'])
        
        # Track price changes
        if symbol in self.last_prices:
            price_change = price - self.last_prices[symbol]
            redis_client.set(f"{symbol}_price_change", price_change)
        
        self.last_prices[symbol] = price
        
        # Implement your trading algorithm
        self.process_signal(symbol, price, side, volume)
        
    def process_signal(self, symbol, price, side, volume):
        # Your trading logic here
        pass

# Initialize and run
bot = TradingBot()
handler = BybitOnTick(symbols=["BTCUSDT", "ETHUSDT"])
handler.start_listening(callback=bot.on_tick)

```
### Configuration
Supported Categories

- spot - Spot trading pairs

- linear - Linear futures contracts

- inverse - Inverse futures contracts

- option - Options contracts

### Environment Variables
Create a .env file in your project root:
```
BYBIT_TESTNET=false
BYBIT_CATEGORY=spot
BYBIT_SYMBOLS=BTCUSDT,ETHUSDT,ADAUSDT
REDIS_HOST=localhost
REDIS_PORT=6379
REDIS_DB=0
```

## API Reference
### BybitOnTick Class
### Constructor

```
BybitOnTick(symbols, category="spot", testnet=False)
```

### Parameters:

- symbols (list): List of trading symbols to monitor

- category (str): Trading category (spot, linear, inverse, option)

- testnet (bool): Use testnet environment

### Methods
```
start_listening(callback)
```
- Start the WebSocket connection and begin processing trades.

### Parameters:

- callback (function): Your onTick function that processes each trade

### Trade Data Structure
Each trade passed to your callback contains:
```
{
    's': 'BTCUSDT',        # Symbol
    'p': '45000.50',       # Price
    'v': '0.001',          # Volume/Size
    'S': 'Buy',            # Side (Buy/Sell)
    'T': 1642694400000,    # Timestamp (milliseconds)
    'i': '1234567890',     # Trade ID
    'BT': false            # Whether buyer is market maker
}
```
## Running the Application
### Development Mode

```
python main.py
```

### Production with Process Management
```
# Using PM2
pm2 start main.py --name bybit-ontick

# Using systemd
sudo systemctl start bybit-ontick
```
### Docker Support
```
# Build image
docker build -t bybit-ontick .

# Run container
docker run -d --name Bybit-onTick \
  -e BYBIT_SYMBOLS=BTCUSDT,ETHUSDT \
  -e BYBIT_CATEGORY=spot \
  bybit-ontick

```

### Error Handling
The library includes comprehensive error handling:

- Connection Errors: Automatic reconnection with exponential backoff

- Data Parsing Errors: Graceful error logging without stopping the stream

- Rate Limiting: Built-in rate limit handling

- Network Issues: Automatic recovery from network interruptions

### Logging
Enable detailed logging:

```
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(name)s - %(levelname)s - %(message)s'
)

```

## Performance Optimization
### Redis Integration
For high-frequency trading, integrate with Redis for fast state management:
```
import redis
import json

redis_client = redis.Redis(host='localhost', port=6379, db=0)

def optimized_on_tick(trade_data):
    # Store trade data in Redis
    redis_client.lpush('trades', json.dumps(trade_data))
    redis_client.ltrim('trades', 0, 999)  # Keep last 1000 trades
    
    # Your trading logic
    process_trade(trade_data)

```


***

## 📄 License
MIT License - Feel free to modify and distribute.


## 🤝 Contributing
Contributions, issues, and feature requests are welcome! Feel free to check issues page.

## ⚠️ Disclaimer

> This project is for informational and educational purposes only. You should not use this information or any other material as legal, tax, investment, financial, or other advice. Nothing contained here is a recommendation, endorsement, or offer by me to buy or sell any securities or other financial instruments.
>
> **If you intend to use real money, use it at your own risk.**
>
> Under no circumstances will I be responsible or liable for any claims, damages, losses, expenses, costs, or liabilities of any kind, including but not limited to direct or indirect damages for loss of profits.

***

## 📞 Contact Me

I develop trading bots of any complexity, dashboards, and indicators for crypto exchanges, forex, and stocks. 🚀

To contact me, please send a message:

*   **Telegram:** [https://t.me/ryu8777](https://t.me/ryu8777) ✈️
*   **Discord:** [https://discord.gg/zSw58e9Uvf](https://discord.gg/zSw58e9Uvf) 🤝

***

## 🤝 Become My Crypto Partner

Start your trading journey on Bybit! Join using my referral link below:

**Join Bybit:** [https://www.bybit.com/invite?ref=P11NJW](https://www.bybit.com/invite?ref=P11NJW)

***

## 🖥️ VPS for Your Bots and Scripts

Keep your bots running 24/7! I prefer and recommend using **DigitalOcean**.

[![DigitalOcean Referral Badge](https://web-platforms.sfo2.digitaloceanspaces.com/WWW/Badge%202.svg)](https://www.digitalocean.com/?refcode=3d7f6e57bc04&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=badge)

**Get $200 in credit over 60 days** by using my referral link:

👉 [https://m.do.co/c/3d7f6e57bc04](https://m.do.co/c/3d7f6e57bc04)
