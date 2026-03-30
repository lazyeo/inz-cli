# INZ Accredited Employer CLI (`inz-cli`)

A lightweight command-line tool to query the New Zealand Immigration (INZ) Accredited Employer list. 

This tool interacts with the internal INZ search API to provide a terminal-friendly view of accredited employers, their trading names, NZBNs, and accreditation expiry dates.

## Features

- **Terminal-friendly**: View results in a clean, aligned table.
- **Fast**: Directly queries the backend API used by the INZ website.
- **Zero Dependencies**: Written in pure Python using only standard libraries.
- **Portable**: No hardcoded paths; works anywhere with Python 3.
- **Supports NZBN**: Search by company name, trading name, or 13-digit NZBN.

## Installation

Simply clone the repository and ensure the script is executable:

```bash
git clone https://github.com/lazyeo/inz-cli.git
cd inz-cli
chmod +x scripts/inz-cli.py
```

## Usage

### Basic Search

Search for an employer by name:

```bash
python3 scripts/inz-cli.py "Apple"
```

### Search by NZBN

Search using the 13-digit New Zealand Business Number:

```bash
python3 scripts/inz-cli.py 9429030665699
```

### Fetch All Results

By default, the script shows the first page of results. Use the `--all` flag to fetch and display all matching records:

```bash
python3 scripts/inz-cli.py "Construction" --all
```

## How it Works

The tool sends a `POST` request to `https://www.immigration.govt.nz/list-api/getAPIResults/` with the query parameters, simulating the behavior of the official INZ search page.

## License

MIT
