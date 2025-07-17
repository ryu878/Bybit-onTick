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
