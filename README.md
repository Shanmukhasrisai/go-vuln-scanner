# GoVulnScanner

A professional, enterprise-grade vulnerability scanner written in Go, inspired by [Nuclei](https://github.com/projectdiscovery/nuclei). This tool enables security professionals to scan web applications and network infrastructure for common vulnerabilities using customizable templates.

## Features

- 🚀 **Fast Multi-threaded Scanning**: Concurrent scanning with configurable thread count
- 🎯 **Template-based Detection**: Built-in vulnerability templates for common issues
- 🔍 **Flexible Target Input**: Scan single targets or multiple targets from a file
- ⏱️ **Customizable Timeout**: Configure request timeout to suit your needs
- 📊 **Results Export**: Save scan results to a file for later analysis
- 🔒 **TLS Support**: Handles HTTPS connections with insecure certificate support

## Usage

### Basic Scanning

#### Scan a Single Target

```bash
# Basic web application scan
./go-vuln-scanner -target https://example.com

# Scan with custom timeout (in seconds)
./go-vuln-scanner -target https://example.com -timeout 10
```

#### Scan Multiple Targets

```bash
# Scan targets from a file (one URL per line)
./go-vuln-scanner -list targets.txt

# Scan with custom thread count for faster execution
./go-vuln-scanner -list targets.txt -threads 20
```

### Networking and Port Scanning

```bash
# Scan network range for vulnerabilities
./go-vuln-scanner -target https://192.168.1.0/24

# Scan specific host with custom port
./go-vuln-scanner -target https://example.com:8443

# Scan with extended timeout for slow networks
./go-vuln-scanner -target https://example.com -timeout 30
```

### Web Application Scanning

```bash
# Scan web application with specific templates
./go-vuln-scanner -target https://webapp.example.com -templates ./templates/web/

# Scan for common web vulnerabilities (XSS, SQLi, etc.)
./go-vuln-scanner -target https://webapp.example.com -severity high,critical

# Export results to JSON file
./go-vuln-scanner -target https://webapp.example.com -output results.json

# Scan web application with custom headers
./go-vuln-scanner -target https://webapp.example.com -headers "Authorization: Bearer token123"
```

### Advanced Options

```bash
# Combine multiple options for comprehensive scanning
./go-vuln-scanner -list targets.txt -threads 15 -timeout 20 -output scan_results.json

# Scan with insecure TLS (skip certificate verification)
./go-vuln-scanner -target https://self-signed.example.com -insecure

# Verbose output for debugging
./go-vuln-scanner -target https://example.com -verbose

# Scan specific vulnerability categories
./go-vuln-scanner -target https://example.com -tags cve,misconfig,exposure
```

### Command-Line Flags

| Flag | Description | Default | Example |
|------|-------------|---------|--------|
| `-target` | Single target URL to scan | - | `-target https://example.com` |
| `-list` | File containing list of targets (one per line) | - | `-list targets.txt` |
| `-threads` | Number of concurrent threads | 10 | `-threads 20` |
| `-timeout` | Request timeout in seconds | 5 | `-timeout 15` |
| `-output` | Output file for results | stdout | `-output results.json` |
| `-templates` | Path to custom template directory | ./templates | `-templates ./my-templates/` |
| `-severity` | Filter by severity (low, medium, high, critical) | all | `-severity high,critical` |
| `-tags` | Filter templates by tags | all | `-tags cve,misconfig` |
| `-insecure` | Skip TLS certificate verification | false | `-insecure` |
| `-verbose` | Enable verbose output | false | `-verbose` |
| `-headers` | Custom HTTP headers | - | `-headers "Key: Value"` |

## API Usage

GoVulnScanner provides a programmatic API for integration into your own Go applications.

### Basic API Example

```go
package main

import (
    "fmt"
    "github.com/Shanmukhasrisai/go-vuln-scanner/scanner"
)

func main() {
    // Create a new scanner instance
    s := scanner.NewScanner()
    
    // Configure scanner options
    s.SetThreads(15)
    s.SetTimeout(10)
    s.SetVerbose(true)
    
    // Scan a single target
    results, err := s.ScanTarget("https://example.com")
    if err != nil {
        fmt.Printf("Error scanning target: %v\n", err)
        return
    }
    
    // Process results
    for _, result := range results {
        fmt.Printf("Found: %s - Severity: %s\n", result.Name, result.Severity)
    }
}
```

### Advanced API Usage

```go
package main

import (
    "fmt"
    "github.com/Shanmukhasrisai/go-vuln-scanner/scanner"
    "github.com/Shanmukhasrisai/go-vuln-scanner/templates"
)

func main() {
    // Create scanner with custom configuration
    config := scanner.Config{
        Threads:    20,
        Timeout:    15,
        Insecure:   false,
        Verbose:    true,
        OutputFile: "results.json",
    }
    
    s := scanner.NewScannerWithConfig(config)
    
    // Load custom templates
    tmplLoader := templates.NewLoader()
    err := tmplLoader.LoadFromDirectory("./custom-templates")
    if err != nil {
        fmt.Printf("Error loading templates: %v\n", err)
        return
    }
    
    s.SetTemplates(tmplLoader.GetTemplates())
    
    // Scan multiple targets
    targets := []string{
        "https://example1.com",
        "https://example2.com",
        "https://example3.com",
    }
    
    results, err := s.ScanTargets(targets)
    if err != nil {
        fmt.Printf("Error during scan: %v\n", err)
        return
    }
    
    // Filter results by severity
    highSeverity := results.FilterBySeverity([]string{"high", "critical"})
    
    fmt.Printf("Found %d high/critical vulnerabilities\n", len(highSeverity))
    
    // Export results
    if err := results.ExportToJSON("vulnerabilities.json"); err != nil {
        fmt.Printf("Error exporting results: %v\n", err)
    }
}
```

### API Reference

#### Scanner Methods

- `NewScanner()` - Create a new scanner instance with default configuration
- `NewScannerWithConfig(config Config)` - Create a scanner with custom configuration
- `ScanTarget(target string)` - Scan a single target and return results
- `ScanTargets(targets []string)` - Scan multiple targets concurrently
- `SetThreads(count int)` - Set number of concurrent threads
- `SetTimeout(seconds int)` - Set request timeout
- `SetVerbose(enabled bool)` - Enable/disable verbose logging
- `SetTemplates(templates []Template)` - Load custom vulnerability templates
- `SetHeaders(headers map[string]string)` - Set custom HTTP headers

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

# Build the project
go build -o go-vuln-scanner

# Run the scanner
./go-vuln-scanner -target https://example.com
```

## Examples

### Example 1: Quick Web Application Scan

```bash
./go-vuln-scanner -target https://mywebapp.com -output report.json
```

### Example 2: Comprehensive Network Scan

```bash
./go-vuln-scanner -list network_hosts.txt -threads 25 -timeout 30 -verbose
```

### Example 3: Targeted Vulnerability Assessment

```bash
./go-vuln-scanner -target https://api.example.com -tags cve,exposure -severity critical -output critical_vulns.json
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is open source and available under the MIT License.
