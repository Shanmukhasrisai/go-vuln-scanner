# GoVulnScanner
## Overview
GoVulnScanner is a fast and flexible web application penetration testing tool designed for cybersecurity professionals, similar to Nuclei. It enables automated security testing by scanning web applications, APIs, and network infrastructure for known vulnerabilities, misconfigurations, and security exposures. Like Nuclei, it uses template-based detection to identify security issues quickly and efficiently, making it an essential tool for penetration testers, security researchers, and DevSecOps teams.
This tool is specifically designed for:
- Web application security testing and vulnerability assessment
- API security scanning and endpoint testing
- Automated penetration testing workflows
- Security research and bug bounty hunting
- DevSecOps integration and continuous security monitoring
## Key Features
- `ScanTargets(targets []string)` - Scan multiple targets concurrently
- `SetThreads(count int)` - Set number of concurrent threads
- `SetTimeout(seconds int)` - Set request timeout
- `SetVerbose(enabled bool)` - Enable/disable verbose logging
- `SetTemplates(templates []Template)` - Load custom vulnerability templates
- `SetHeaders(headers map[string]string)` - Set custom HTTP headers

### Python Support
- **Python Integration** - Full support for Python-based security scripts and automation
- **Python API Wrapper** - Native Python bindings for seamless integration
- **Script Extensibility** - Create custom vulnerability checks using Python

### Script Examples

#### Python Example: Basic Vulnerability Scan
```python
from govulnscanner import Scanner

# Initialize scanner
scanner = Scanner()
scanner.set_threads(10)
scanner.set_timeout(30)

# Scan targets
targets = ['https://example.com', 'https://test.com']
results = scanner.scan_targets(targets)

# Filter and export results
results.filter_by_severity(['high', 'critical'])
results.export_to_json('vulnerabilities.json')
```

#### Python Example: Custom Template Scan
```python
from govulnscanner import Scanner, Template

# Load custom templates
scanner = Scanner()
scanner.load_templates_from_directory('./templates')
scanner.set_verbose(True)

# Execute scan with custom headers
headers = {'User-Agent': 'GoVulnScanner/1.0'}
scanner.set_headers(headers)

results = scanner.scan('https://target.com')
print(f"Found {results.count()} vulnerabilities")
```

#### Result Methods
- `FilterBySeverity(levels []string)` - Filter results by severity level
- `FilterByTag(tags []string)` - Filter results by template tags
- `ExportToJSON(filename string)` - Export results to JSON file
- `ExportToHTML(filename string)` - Export results to HTML report
- `GetStatistics()` - Get scan statistics and summary
## Installation
```bash
# Clone the repository
git clone https://github.com/Shanmukhasrisai/go-vuln-scanner.git
# Navigate to the project directory
cd go-vuln-scanner
# Build the project (binary will be created in current directory)
go build -o govulnscanner cmd/govulnscanner/main.go
# Optional: Install to GOPATH/bin for system-wide access
go install ./cmd/govulnscanner
# Run the scanner from the project directory
./govulnscanner --target https://example.com
# Or run from anywhere if installed to GOPATH/bin
govulnscanner --target https://example.com
```
### Installation Path Structure
```
go-vuln-scanner/
├── cmd/
│   └── govulnscanner/
│       └── main.go          # Main entry point
├── pkg/                      # Package source code
