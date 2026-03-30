#!/usr/bin/env python3

import argparse
import urllib.request
import urllib.parse
import json
import sys

def search_employers(query, page=1):
    url = "https://www.immigration.govt.nz/list-api/getAPIResults/"
    data = urllib.parse.urlencode({
        "query": query,
        "collection": "2",
        "page": str(page)
    }).encode("utf-8")
    
    req = urllib.request.Request(url, data=data)
    req.add_header("User-Agent", "Mozilla/5.0")
    
    try:
        with urllib.request.urlopen(req) as response:
            return json.loads(response.read().decode('utf-8'))
    except Exception as e:
        print(f"Error fetching data: {e}", file=sys.stderr)
        sys.exit(1)

def print_table(items):
    cols = ["Employer Name", "Trading Name", "NZBN", "Expiry Date"]
    rows = []
    
    for item in items:
        fields = {f["APIColumn"]: f["Value"] for f in item.get("field_schema", {}).get("raw", [])}
        expiry = fields.get("expiryDateOfAccreditation", "")
        if expiry and "T" in expiry:
            expiry = expiry.split("T")[0]
            
        rows.append([
            str(fields.get("employerName") or "")[:40],
            str(fields.get("tradingName") or "")[:40],
            str(fields.get("nzbn") or ""),
            expiry
        ])
    
    # Prepend column headers to rows for width calculation
    all_rows = [cols] + rows
    widths = [max(len(str(item)) for item in col) for col in zip(*all_rows)]
    format_str = " | ".join(f"{{:<{w}}}" for w in widths)
    
    print(format_str.format(*cols))
    print("-+-".join("-" * w for w in widths))
    for row in rows:
        print(format_str.format(*row))

def main():
    parser = argparse.ArgumentParser(description="Search for INZ Accredited Employers")
    parser.add_argument("query", help="Employer name, trading name, or NZBN to search for")
    parser.add_argument("--all", action="store_true", help="Fetch all pages of results")
    args = parser.parse_args()
    
    print(f"Searching INZ Accredited Employers for '{args.query}'...\n")
    res = search_employers(args.query, 1)
    
    total = res.get("totalResults", 0)
    total_pages = res.get("totalPages", 0)
    
    if total == 0:
        print("No accredited employers found.")
        return
        
    print(f"Found {total} results.\n")
    
    try:
        items = json.loads(res.get("results", "[]"))
    except json.JSONDecodeError:
        items = []
        
    all_items = items
    
    if args.all and total_pages > 1:
        for p in range(2, total_pages + 1):
            res_page = search_employers(args.query, p)
            try:
                all_items.extend(json.loads(res_page.get("results", "[]")))
            except json.JSONDecodeError:
                pass

    if all_items:
        print_table(all_items)
        if not args.all and total_pages > 1:
            print(f"\nShowing page 1 of {total_pages}. Use --all to see all results.")

if __name__ == "__main__":
    main()
