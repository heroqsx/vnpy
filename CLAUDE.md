# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

VeighNa (vnpy) is a comprehensive Python-based quantitative trading framework. The core framework provides:
- Event-driven architecture (`vnpy.event`)
- Main trading engine (`vnpy.trader.engine`)
- AI-powered quantitative research module (`vnpy.alpha`)
- Support for multiple trading gateways and data feeds
- GUI-based trading platform

## Key Architecture Components

### Core Modules
- **`vnpy.event`**: Event-driven framework with `EventEngine` and `Event` classes
- **`vnpy.trader`**: Main trading engine with `MainEngine`, gateways, apps, and data models
- **`vnpy.alpha`**: AI-powered quantitative research module (new in v4.0)
- **`vnpy.chart`**: High-performance charting components
- **`vnpy.rpc`**: Cross-process communication components

### Event System
- Central `EventEngine` distributes events based on type
- Events include: `EVENT_TICK`, `EVENT_ORDER`, `EVENT_TRADE`, `EVENT_POSITION`, etc.
- Handlers register for specific event types

### Trading Engine Structure
- `MainEngine`: Central coordinator
- `BaseGateway`: Abstract base class for all trading interfaces
- `BaseApp`: Abstract base class for trading applications
- Data objects: `TickData`, `BarData`, `OrderData`, `TradeData`, `PositionData`, `AccountData`

### AI/ML Module (vnpy.alpha)
- **Dataset**: Feature engineering with Alpha 158 factors
- **Model**: ML models (Lasso, LightGBM, MLP)
- **Strategy**: ML-based trading strategies
- **Lab**: Complete research workflow management

## Development Commands

### Installation
```bash
# Windows
install.bat

# Linux/Mac
bash install.sh
```

### Code Quality & Testing
```bash
# Linting with ruff
ruff check .

# Type checking with mypy
mypy vnpy

# Build package
uv build
```

### Running Examples
```bash
# Basic trader example
python examples/veighna_trader/run.py

# Alpha research notebooks (Jupyter)
jupyter notebook examples/alpha_research/
```

## Key Dependencies

### Core Dependencies
- `PySide6`: GUI framework
- `pyqtgraph`: Charting library
- `numpy`, `pandas`: Data processing
- `ta-lib`: Technical analysis
- `loguru`: Logging

### Optional Dependencies (alpha module)
- `polars`: High-performance data processing
- `scikit-learn`, `lightgbm`, `torch`: Machine learning
- `alphalens-reloaded`: Performance analysis

## Project Structure

```
vnpy/
├── event/           # Event-driven framework
├── trader/          # Main trading engine
│   ├── engine.py    # MainEngine and base classes
│   ├── gateway.py   # BaseGateway abstract class
│   ├── app.py       # BaseApp abstract class
│   ├── object.py    # Data objects (TickData, OrderData, etc.)
│   └── ui/          # GUI components
├── alpha/           # AI quantitative research
│   ├── dataset/     # Feature engineering
│   ├── model/       # ML models
│   ├── strategy/    # Trading strategies
│   └── lab.py       # Research workflow
├── chart/           # Charting components
├── rpc/             # RPC communication
└── __init__.py

examples/
├── veighna_trader/  # Main trading platform example
├── alpha_research/  # Jupyter notebook examples
└── various other usage examples
```

## Development Patterns

### Adding New Gateways
1. Inherit from `BaseGateway` in `vnpy.trader.gateway`
2. Implement required methods: `connect`, `subscribe`, `send_order`, etc.
3. Register with main engine using `main_engine.add_gateway()`

### Adding New Apps
1. Inherit from `BaseApp` in `vnpy.trader.app`
2. Implement app-specific functionality
3. Register with main engine using `main_engine.add_app()`

### Event Handling
```python
from vnpy.event import EventEngine, Event

def handler(event: Event):
    print(f"Received event: {event.type}")

event_engine = EventEngine()
event_engine.register(EVENT_TICK, handler)
```

## Important Files

- `pyproject.toml`: Project configuration and dependencies
- `vnpy/__init__.py`: Package version and exports
- `vnpy/trader/engine.py`: Main engine implementation
- `vnpy/event/engine.py`: Event engine implementation
- `examples/veighna_trader/run.py`: Main application entry point

## Testing Approach

This project uses:
- **Ruff** for linting (configured in `pyproject.toml`)
- **Mypy** for static type checking
- Example-based testing rather than unit test suite
- GitHub Actions for CI/CD (see `.github/workflows/pythonapp.yml`)

## Common Development Tasks

1. **Adding new trading interface**: Create gateway module
2. **Developing new strategy**: Create app module or use alpha framework
3. **Extending data models**: Modify `vnpy.trader.object.py`
4. **Custom UI components**: Extend `vnpy.trader.ui` modules
5. **ML research**: Use `vnpy.alpha` module with Jupyter notebooks

## Performance Considerations

- Event engine uses threading for concurrent processing
- Data objects are designed for high-frequency trading
- Alpha module uses Polars for efficient data processing
- Charting components optimized for real-time updates