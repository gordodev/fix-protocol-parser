# FIX Protocol Toolkit

Python toolkit for parsing, analyzing, and monitoring FIX protocol messages. Extracts trading events from logs, supports real-time monitoring, identifies rejects, and provides visualization. Optimized for trading support professionals.

## Features

- Parse FIX messages from logs or real-time streams
- Identify and analyze rejection messages
- Monitor FIX logs for trading events
- Extract key trading information from message flow

## Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/fix-protocol-toolkit.git

# Install dependencies
pip install -r requirements.txt
```

## Usage

### Basic Parsing

```python
from fixparser import parse_fix_message

message = "8=FIX.4.4|9=122|35=D|34=215|49=CLIENT12|52=20100225-19:41:57.316|56=B|1=Marcel|11=13346|21=1|55=MSFT|54=1|60=20100225-19:39:52.020|40=2|44=5|38=100|10=072|"
parsed = parse_fix_message(message, delimiter='|')
print(parsed)
```

### Monitoring Logs

```python
from fixmonitor import FIXMonitor

monitor = FIXMonitor("/path/to/logs")
monitor.on_reject(lambda msg, analysis: print(f"Reject: {analysis['error_reason']}"))
monitor.start()
```

## Status

This is a work in progress. Currently implementing core parsing functionality.
