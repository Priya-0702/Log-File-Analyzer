# Log-File-Analyzer
import re
from collections import Counter

def analyze_logs(log_file_path):
    with open(log_file_path, 'r') as file:
        logs = file.readlines()

    # Regex patterns for analysis
    pattern_status_code = r'"\s(\d{3})\s'
    pattern_requested_page = r'"GET\s(.*?)\sHTTP'
    pattern_ip_address = r'^(\d+\.\d+\.\d+\.\d+)'

    # Counters for analysis
    status_codes = Counter()
    requested_pages = Counter()
    ip_addresses = Counter()

    for log in logs:
        # Extract and count status codes
        status_code_match = re.search(pattern_status_code, log)
        if status_code_match:
            status_codes[status_code_match.group(1)] += 1

        # Extract and count requested pages
        page_match = re.search(pattern_requested_page, log)
        if page_match:
            requested_pages[page_match.group(1)] += 1

        # Extract and count IP addresses
        ip_match = re.search(pattern_ip_address, log)
        if ip_match:
            ip_addresses[ip_match.group(1)] += 1

    # Generate summarized report
    report = {
        "404 Errors": status_codes.get("404", 0),
        "Most Requested Pages": requested_pages.most_common(5),
        "Top IP Addresses": ip_addresses.most_common(5)
    }

    return report

# Specify the log file path
log_file = "web_server.log"

# Analyze the logs and generate the report
result = analyze_logs(log_file)

# Print the summarized report
print("Summary Report:")

print(f"Total 404 Errors: {result['404 Errors']}")

print("Most Requested Pages:")
for page, count in result['Most Requested Pages']:
    print(f"{page}: {count} requests")
    
print("Top IP Addresses:")
for ip, count in result['Top IP Addresses']:

    print(f"{ip}: {count} requests")
