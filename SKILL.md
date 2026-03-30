---
name: inz-cli
description: Search for New Zealand Immigration Accredited Employers using the INZ internal API. Use when asked to check if a specific company is an accredited employer, find employer NZBNs, or browse the accredited employer list by name, trading name, or 13-digit NZBN.
---

# INZ Accredited Employer Search

Search the New Zealand Immigration (INZ) database for accredited employers.

## Usage

Use the provided script to query the database. It handles API requests, pagination, and tabular formatting.

### Search by Keyword (Name, Trading Name, or NZBN)

Run the script with the query as an argument:

```bash
python3 scripts/inz-cli.py "Company Name"
```

### Fetch All Results

If there are multiple pages of results, use the `--all` flag to fetch and display everything in one table:

```bash
python3 scripts/inz-cli.py "Search Query" --all
```

## Resources

### scripts/
- `inz-cli.py`: The main search script. It uses standard Python libraries and hits `https://www.immigration.govt.nz/list-api/getAPIResults/`.
