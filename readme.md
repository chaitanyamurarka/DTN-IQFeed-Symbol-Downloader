# DTN IQFeed Symbol Downloader

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/python-v3.10+-blue.svg)](https://www.python.org/)
[![Automated Downloads](https://img.shields.io/badge/automated-downloads-green.svg)](https://github.com/features/actions)
[![Data Size](https://img.shields.io/badge/data%20size-~173MB-orange.svg)](https://github.com/)

A robust Python-based tool for downloading comprehensive symbol data from DTN IQFeed, featuring automatic pagination, error handling, resume capability, and GitHub Actions automation for scheduled downloads.

## 🎯 Overview

The DTN Symbol Downloader is an open-source solution designed to automatically fetch and maintain an up-to-date repository of financial instrument symbols from DTN IQFeed's public API. This tool addresses the need for researchers, traders, and financial analysts to have access to a comprehensive list of tradable symbols across multiple exchanges and security types.

### Why This Tool Exists

- **Data Accessibility**: DTN IQFeed provides extensive market data, but accessing their complete symbol universe programmatically can be challenging
- **Automation**: Manual symbol downloads are time-consuming and prone to errors
- **Reliability**: Built-in retry mechanisms and resume capabilities ensure complete data downloads
- **Open Source**: Free alternative to commercial symbol databases
- **Maintenance-Free**: GitHub Actions automation keeps your symbol database current

## 📊 What is DTN IQFeed?

DTN IQFeed is a professional-grade market data feed service that provides real-time and historical data for:
- Stocks (NYSE, NASDAQ, AMEX, etc.)
- Futures contracts
- Options
- Forex pairs
- Indices
- Crypto assets

This tool specifically targets their symbol search API to create a comprehensive database of available instruments.

## 🚀 Features

### Core Functionality
- **Complete Symbol Universe**: Downloads ALL available symbols from DTN IQFeed (~500,000+ instruments)
- **Smart Pagination**: Handles DTN's pagination limits (4,998 symbols per request)
- **Robust Error Handling**: Automatic retry with exponential backoff for network issues
- **Resume Capability**: Can resume interrupted downloads from the last successful batch
- **Progress Tracking**: Real-time progress updates with ETA calculations
- **Duplicate Removal**: Automatically removes duplicate symbols
- **Batch Processing**: Saves data in batches to prevent data loss

### Data Included
Each symbol record contains:
- `symbol`: The trading symbol (e.g., AAPL, @ES#, EUR.USD)
- `description`: Full name/description of the instrument
- `exchange`: Trading exchange or market
- `securityType`: Type of security (Stock, Future, Option, etc.)
- Additional metadata fields

### Automation Features
- **GitHub Actions Integration**: Fully automated scheduled downloads
- **Scheduled Runs**: Default runs every 12 hours (customizable)
- **Artifact Storage**: Compressed data stored as GitHub artifacts
- **Automatic Releases**: Optional GitHub releases with versioned data
- **Failure Notifications**: Automatic issue creation on download failures

## 📋 Requirements

### Local Execution
- Python 3.10 or higher
- `requests` library for HTTP operations
- `pandas` for data manipulation
- ~200MB free disk space for data storage

### GitHub Actions (Automated)
- GitHub repository with Actions enabled
- No additional requirements (runs on GitHub's infrastructure)

## 🛠️ Installation

### Quick Start (Local)

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/dtn-symbol-downloader.git
cd dtn-symbol-downloader
```

2. **Install dependencies**
```bash
pip install -r requirements.txt
```

3. **Run the downloader**
```bash
python dtn_symbol_downloader.py
```

### Advanced Usage

```bash
# Resume from a specific batch (e.g., batch 41)
python dtn_symbol_downloader.py --resume 41

# Custom delay between batches (default: 2 seconds)
python dtn_symbol_downloader.py --delay 5

# Combine both options
python dtn_symbol_downloader.py --resume 41 --delay 3
```

## 🤖 GitHub Actions Automation

### Setting Up Automated Downloads

1. **Fork or create a new repository** with the provided files

2. **Enable GitHub Actions**:
   - Go to Settings → Actions → General
   - Select "Allow all actions and reusable workflows"
   - Enable "Read and write permissions"

3. **The workflow will automatically**:
   - Run every 12 hours (00:00 and 12:00 UTC)
   - Download all symbols with retry logic
   - Create compressed artifacts (`.gz` and `.zip`)
   - Optionally create GitHub releases
   - Handle failures gracefully with notifications

### Customizing the Schedule

Edit `.github/workflows/download-symbols.yml`:

```yaml
schedule:
  # Run every 6 hours
  - cron: '0 */6 * * *'
  
  # Run once daily at 3 AM UTC
  - cron: '0 3 * * *'
  
  # Run on weekdays at 9 AM UTC
  - cron: '0 9 * * 1-5'
```

## 📁 Output Format

### File Structure
```
dtn_symbols/
├── all_symbols_latest.csv       # Most recent complete download
├── all_symbols_YYYYMMDD_HHMMSS.csv  # Timestamped versions
├── batch_*.csv                  # Temporary batch files (cleaned up)
└── download_state.json          # Resume state (if interrupted)
```

### Sample Data
```csv
symbol,description,exchange,securityType,lastPrice
AAPL,APPLE INC,NASDAQ,STOCK,189.95
@ES#,E-MINI S&P 500,CME,FUTURE,5123.25
EUR.USD,EURO/US DOLLAR,FOREX,FOREX,1.0875
```

## 📈 Statistics

Based on typical downloads:
- **Total Symbols**: ~27,00,000+
- **File Size**: ~173 MB (uncompressed)
- **Download Time**: 30-60 minutes
- **Exchanges Covered**: 50+
- **Security Types**: Stocks, Futures, Options, Forex, Crypto, Indices

## 🔧 Technical Implementation

### API Integration
The tool interfaces with DTN's public API endpoints:
- Symbol Search: `https://ws1.dtn.com/SymbolSearch/QuerySymbolsDD`
- Categories: `https://ws1.dtn.com/SymbolSearch/GetSymbolCategories`

### Key Design Decisions

1. **Pagination Handling**: DTN limits responses to 4,998 symbols per request. The tool automatically handles pagination using the `nextKey` parameter.

2. **Error Recovery**: Implements exponential backoff for transient errors and saves state for resume capability.

3. **Memory Efficiency**: Processes data in batches to handle large datasets without memory issues.

4. **Data Integrity**: Validates responses and removes duplicates to ensure data quality.

## 🤝 Contributing

We welcome contributions! Please feel free to submit issues, fork the repository, and create pull requests.

### Areas for Contribution
- Additional output formats (JSON, Parquet)
- Database integration (PostgreSQL, MySQL)
- Symbol filtering options
- Performance optimizations
- Additional data enrichment

### Development Setup
```bash
# Clone your fork
git clone https://github.com/yourusername/dtn-symbol-downloader.git

# Create a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install in development mode
pip install -r requirements.txt
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details. This means you can:
- Use the code commercially
- Modify and distribute
- Use privately
- But must include the license and copyright notice

## 🚨 Disclaimer

This tool accesses publicly available data from DTN IQFeed. Users are responsible for:
- Complying with DTN's terms of service
- Understanding any rate limits or usage restrictions
- Verifying data accuracy for their use case

The authors are not affiliated with DTN and provide no warranties regarding data accuracy or availability.

## 🙏 Acknowledgments

- DTN IQFeed for providing the public API
- GitHub Actions for free automation infrastructure
- The Python community for excellent libraries
- All contributors and users of this tool

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/yourusername/dtn-symbol-downloader/issues)
- **Discussions**: [GitHub Discussions](https://github.com/yourusername/dtn-symbol-downloader/discussions)
- **Wiki**: [Project Wiki](https://github.com/yourusername/dtn-symbol-downloader/wiki)

## 🔗 Related Projects

- [IQFeed Python Client](https://github.com/example/iqfeed-python)
- [Market Data Collectors](https://github.com/topics/market-data)
- [Financial Data APIs](https://github.com/topics/financial-data)

## 📊 Keywords for Search

`dtn-iqfeed` `symbol-downloader` `market-data` `financial-data` `stock-symbols` `futures-symbols` `options-data` `forex-symbols` `automated-download` `python-scraper` `github-actions` `trading-symbols` `market-symbols` `iqfeed-api` `financial-instruments` `symbol-database` `market-data-feed` `trading-data` `quantitative-finance` `algo-trading`

---

**⭐ If you find this project useful, please consider giving it a star! It helps others discover the tool.**

**🐛 Found a bug or have a feature request? [Open an issue](https://github.com/yourusername/dtn-symbol-downloader/issues/new)**
