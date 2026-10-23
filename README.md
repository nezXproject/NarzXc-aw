# LyTa 🚀

**LyTa** is a lightweight, zero-dependency Python load-testing tool designed exclusively for testing the performance, throughput, and resilience of your own web servers and applications. 

Written entirely using Python's built-in standard library (`urllib` and `concurrent.futures`), LyTa requires zero third-party packages or complex setups. It features a clean command-line interface, adjustable concurrency, real-time progress tracking, and a comprehensive summary report.

---

## ⚠️ Important Disclaimer
LyTa is built **strictly** for testing web servers and applications that you own or have explicit authorization to test. Unauthorized load testing against third-party servers is illegal and violates standard terms of service. Always use responsibly.

---

## Features

- **Zero External Dependencies:** Runs out of the box with standard Python (3.6+).
- **Concurrent Threading:** Uses Python's `ThreadPoolExecutor` to simulate simultaneous user traffic.
- **Real-time Progress:** Keeps you updated on request progress during execution.
- **Comprehensive Summary:** Outputs total time elapsed, successful requests, failed requests, and requests per second.
- **Lightweight & Fast:** Simple script that is easy to inspect, modify, and run anywhere.

---

## Installation

1. Ensure you have Python 3.6 or higher installed.
2. Download or copy the `lyta.py` script into your working directory.

---

## Usage

Run LyTa from your command line by passing the target URL and optional flags.

### Basic Command
```bash
python lyta.py http://localhost:8000
```

### Advanced Options
You can configure the total number of requests (`-n`) and the number of concurrent worker threads (`-w`):
```bash
python lyta.py http://localhost:8000 -n 500 -w 25
```

### Command-Line Arguments
| Argument / Flag | Description | Default |
| :--- | :--- | :--- |
| `url` | **(Required)** Target URL to test (e.g., `http://127.0.0.1:5000`) | *None* |
| `-n`, `--requests` | Total number of requests to send | `100` |
| `-w`, `--workers` | Number of concurrent worker threads | `10` |
| `-h`, `--help` | Show the help message and exit | *None* |

---

## Example Output

```text
==================================================
🚀 LyTa - Lightweight Load Tester
⚠️  WARNING: Only use this tool on servers you own or have explicit permission to test.
==================================================
Targeting:      http://localhost:8000
Total Requests: 200
Workers:        20
Starting load test...

Progress: 20/200 requests completed...
Progress: 40/200 requests completed...
Progress: 60/200 requests completed...
Progress: 80/200 requests completed...
Progress: 100/200 requests completed...
Progress: 120/200 requests completed...
Progress: 140/200 requests completed...
Progress: 160/200 requests completed...
Progress: 180/200 requests completed...
Progress: 200/200 requests completed...

==================================================
📊 LyTa Summary
==================================================
Time taken:   0.84 seconds
Successful:   200
Failed:       0
Req/Second:   238.10
==================================================
```

---

## License
This project is open-source and provided under the MIT License. Feel free to adapt and modify for your personal testing needs.
