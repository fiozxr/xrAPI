# xrAPI - Advanced API Endpoint Discovery & Security Analysis

<p align="center">
  <img src="https://raw.githubusercontent.com/xrapi/xrapi/main/assets/logo.png" alt="xrAPI Logo" width="200">
</p>

<p align="center">
  <a href="https://pypi.org/project/xrapi/"><img src="https://img.shields.io/pypi/v/xrapi.svg" alt="PyPI version"></a>
  <a href="https://python.org/"><img src="https://img.shields.io/badge/python-3.11+-blue.svg" alt="Python version"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-green.svg" alt="License"></a>
  <a href="https://github.com/xrapi/xrapi/actions"><img src="https://github.com/xrapi/xrapi/workflows/Tests/badge.svg" alt="Tests"></a>
</p>

<p align="center">
  <b>High-performance, asynchronous API endpoint discovery and security analysis tool</b><br>
  Designed for security professionals, penetration testers, and DevSecOps engineers
</p>

---


## 📋 Table of Contents

- [Features](#-features)
- [Installation](#-installation)
- [Quick Start](#-quick-start)
- [Usage](#-usage)
- [Dashboard](#-dashboard)
- [Documentation](#-documentation)
- [API Reference](#-api-reference)
- [Contributing](#-contributing)
- [License](#-license)
- [Contact](#-contact)

---

## 🚀 Features

### Core Capabilities

| Feature | Description |
|---------|-------------|
| **Async HTTP/2 Scanning** | High-concurrency scanning with `asyncio` and `httpx` |
| **Intelligent Clustering** | Semantic path analysis groups `/users/1`, `/users/2` → `/users/{id}` |
| **Response Classification** | Multi-factor fingerprinting beyond status codes |
| **State-Aware Scanning** | Parallel authenticated/unauthenticated probing |
| **Adaptive Rate Limiting** | Automatically adjusts to target responsiveness |
| **Real-time Dashboard** | Web interface with live updates via WebSocket |
| **Comprehensive Logging** | Structured JSON logs with audit trail |
| **Multiple Import Formats** | Burp Suite, OWASP ZAP, HAR, OpenAPI, Postman |
| **Export Options** | JSON, CSV, HTML, OpenAPI specification |

### Architecture Principles

1. **Speed without destruction** - Adaptive rate limiting responds to target behavior
2. **Intelligence over enumeration** - Semantic clustering treats endpoints as a graph
3. **Observability by design** - Every operation is logged and traceable

---

## 📦 Installation

### From PyPI (Recommended)

```bash
# Basic installation (CLI only)
pip install xrapi

# With web dashboard support
pip install xrapi[web]

# Development installation
pip install xrapi[dev]

# Full installation
pip install xrapi[web,dev]
```

### From Source

```bash
git clone https://github.com/xrapi/xrapi.git
cd xrapi
pip install -e .
```

### Verify Installation

```bash
xrapi --version
```

---

## 🎯 Quick Start

### 1. Basic Scan (CLI)

```bash
# Scan a target (replace with your authorized target)
xrapi scan https://api.example.com

# With custom wordlist
xrapi scan https://api.example.com -w /path/to/wordlist.txt

# With authentication
xrapi scan https://api.example.com -a "Bearer YOUR_TOKEN"

# Save results to file
xrapi scan https://api.example.com -o results.json
```

### 2. Launch Web Dashboard

```bash
# Launch dashboard with current scan
xrapi scan https://api.example.com --web

# Or launch dashboard separately
xrapi dashboard

# Load specific scan in dashboard
xrapi dashboard --scan scan_20240115_120000
```

### 3. View Results

```bash
# List all saved scans
xrapi list

# Show scan details
xrapi show scan_20240115_120000

# Export to different formats
xrapi export scan_20240115_120000 -o report.html -f html
xrapi export scan_20240115_120000 -o spec.json -f openapi
```

---

## 💻 Usage

### CLI Commands

```
xrapi [COMMAND] [OPTIONS]

Commands:
  scan        Start a new API endpoint discovery scan
  list        List all locally saved scans
  show        Show detailed information about a scan
  export      Export a scan to various formats
  diff        Compare two scans
  dashboard   Launch the web dashboard
  wordlist    Manage custom wordlists
  logs        View xrAPI logs
```

### Scan Options

```bash
xrapi scan TARGET [OPTIONS]

Options:
  -n, --name TEXT           Scan name for identification
  -w, --wordlist PATH       Wordlist file(s) [multiple allowed]
  -c, --concurrency INT     Concurrent requests (1-200) [default: 50]
  -r, --rate-limit FLOAT    Requests per second (0=unlimited) [default: 0]
  -t, --timeout INT         Request timeout in seconds [default: 30]
  -e, --extensions TEXT     File extensions to try [default: ",.json,.xml"]
  -H, --header TEXT         Custom headers (Key:Value) [multiple allowed]
  --cookie TEXT             Cookie string
  -a, --auth TEXT           Authorization token (Bearer/API key)
  -p, --proxy TEXT          Proxy URL (http://proxy:8080)
  --http2 / --no-http2      Enable HTTP/2 support [default: http2]
  -o, --output PATH         Output file path
  -f, --format TEXT         Output format: json, csv, html [default: json]
  --hide-codes TEXT         Hide status codes (404,500,502)
  -W, --web                 Launch web dashboard after scan
  -v, --verbose             Verbose output
  --no-save                 Don't save scan to local storage
  --help                    Show this message and exit
```

### Examples

```bash
# Basic scan with high concurrency
xrapi scan https://api.example.com -c 100

# Scan with rate limiting
xrapi scan https://api.example.com -r 50

# Authenticated scan with custom headers
xrapi scan https://api.example.com \
  -a "Bearer TOKEN" \
  -H "X-API-Version: 2" \
  -H "X-Custom-Header: value"

# Scan through proxy
xrapi scan https://api.example.com -p http://127.0.0.1:8080

# Export as HTML report
xrapi scan https://api.example.com -o report.html -f html

# Compare two scans
xrapi diff scan_20240114 scan_20240115
```

---

## 🌐 Dashboard

### Launching the Dashboard

```bash
# Launch standalone dashboard
xrapi dashboard

# Launch on custom port
xrapi dashboard -p 3000

# Load specific scan
xrapi dashboard --scan scan_id

# Don't open browser automatically
xrapi dashboard --no-browser
```

### Dashboard Features

| View | Description |
|------|-------------|
| **Overview** | Real-time metrics, API surface graph, recent activity |
| **New Scan** | Configure and start scans with full options |
| **Results** | Browse clusters, view transactions, timeline |
| **Changelog** | Compare scans, track API changes over time |

### Dashboard Screenshots

<p align="center">
  <img src="https://raw.githubusercontent.com/xrapi/xrapi/main/assets/dashboard-overview.png" alt="Dashboard Overview" width="800">
</p>

---

## 📚 Documentation

### Data Storage

xrAPI stores data locally in your home directory:

```
~/.xrapi/
├── scans/          # Saved scan results (JSON)
├── logs/           # Application logs
├── wordlists/      # Custom wordlists
└── exports/        # Exported reports
```

Change data directory:
```bash
xrapi --data-dir /custom/path scan https://api.example.com
```

### Wordlists

```bash
# List custom wordlists
xrapi wordlist list

# Add a wordlist
xrapi wordlist add -n mylist -f /path/to/wordlist.txt

# Remove a wordlist
xrapi wordlist remove -n mylist
```

### Logging

```bash
# View recent logs
xrapi logs

# View last 100 lines
xrapi logs -n 100

# Follow logs in real-time
xrapi logs -f
```

---

## 🔌 API Reference

When running the dashboard, the following API endpoints are available:

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/health` | GET | Health check |
| `/api/scans` | GET | List all scans |
| `/api/scans/{id}` | GET | Get scan details |
| `/api/scans/{id}/clusters` | GET | Get scan clusters |
| `/api/scans/{id}/transactions` | GET | Get scan transactions |
| `/api/dashboard/metrics` | GET | Dashboard metrics |
| `/api/dashboard/graph` | GET | Graph data for visualization |
| `/ws` | WebSocket | Real-time updates |

---

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Development Setup

```bash
git clone https://github.com/xrapi/xrapi.git
cd xrapi
pip install -e ".[dev]"
```

### Running Tests

```bash
pytest
```

### Code Style

```bash
# Format code
black backend/xrapi

# Run linter
ruff check backend/xrapi

# Type checking
mypy backend/xrapi
```

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

### Third-Party Licenses

xrAPI uses the following open-source packages:
- FastAPI (MIT License)
- httpx (BSD License)
- Typer (MIT License)
- Rich (MIT License)
- D3.js (BSD License)

---
---

## 🙏 Acknowledgments

- Thanks to all contributors who have helped shape xrAPI
- Inspired by tools like Gobuster, Dirb, and FFUF
- Built with modern Python async patterns

---

## ⚡ Performance

Benchmarks on a typical VPS (2 vCPU, 4GB RAM):

| Metric | Value |
|--------|-------|
| Requests/sec | ~500-1000 (HTTP/2) |
| Concurrent connections | Up to 200 |
| Memory usage | ~100-300MB |
| Scan 10,000 paths | ~20-30 seconds |

---


## ⚠️ LEGAL DISCLAIMER & TERMS OF USE

**BY INSTALLING, ACCESSING, OR USING XRAPI, YOU AGREE TO BE BOUND BY THESE TERMS.**

### Authorized Use Only

**xrAPI is a security testing tool intended EXCLUSIVELY for:**
- ✅ Security professionals conducting authorized penetration tests
- ✅ Bug bounty hunters on programs they are authorized to test
- ✅ DevSecOps engineers testing their own applications
- ✅ Security researchers with explicit written permission
- ✅ Educational purposes in controlled environments

### Prohibited Use

**YOU ARE STRICTLY PROHIBITED FROM USING XRAPI TO:**
- ❌ Scan, test, or probe any system without explicit written authorization
- ❌ Access, attempt to access, or interfere with systems you do not own
- ❌ Violate any local, state, national, or international laws
- ❌ Infringe upon the privacy rights of individuals or organizations
- ❌ Cause damage, disruption, or denial of service to any system
- ❌ Use for malicious, fraudulent, or criminal purposes
- ❌ Remove or alter copyright notices or proprietary labels

### Legal Responsibility

**THE USER ASSUMES ALL LEGAL RESPONSIBILITY:**
- The authors and contributors of xrAPI accept NO liability for misuse
- You are solely responsible for ensuring your use complies with all applicable laws
- You must obtain proper authorization before scanning any target
- Unauthorized access to computer systems is a criminal offense in most jurisdictions
- Violation of these terms may result in civil and criminal penalties

### No Warranty

**XRAPI IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND:**
- No guarantee of fitness for any particular purpose
- No guarantee of accuracy or completeness of results
- Use at your own risk - the tool may cause unintended effects on target systems

---


<p align="center">
  <b>I don't know , What the Fu#k is going on.</p>

---
