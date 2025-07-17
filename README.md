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
