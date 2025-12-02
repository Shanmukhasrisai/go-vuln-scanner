# GoVulnScanner

## Overview

GoVulnScanner is a high-performance vulnerability detection framework that provides **dual operational modes**: configuration-driven scanning and programmable API integration. This hybrid approach enables both rapid deployment via configuration files and deep customization through programmatic interfaces.

## Core Capabilities

### Configuration File Support
GoVulnScanner supports declarative configuration through structured configuration files, enabling:
- **Rapid Deployment**: Define scan parameters, targets, and detection rules without code modification
- **Environment Flexibility**: Separate configuration profiles for development, staging, and production environments
- **Template Management**: Load and manage vulnerability templates via configuration directives
- **Persistent Settings**: Store and version-control scan configurations alongside your infrastructure as code

### Programming Support
Comprehensive API access provides full programmatic control over scanner functionality:
- **Template Injection**: `SetTemplates(templates []Template)` - Dynamic vulnerability signature loading at runtime
- **Header Customization**: `SetHeaders(headers map[string]string)` - Inject custom HTTP headers for authentication and request manipulation
- **Workflow Integration**: Embed scanning capabilities directly into existing security automation pipelines
- **Custom Detection Logic**: Extend built-in detection capabilities with application-specific vulnerability checks

### Advanced Features

**Multi-Threaded Scanning**
- Concurrent request processing with configurable thread pools
- Optimized resource utilization for large-scale target assessment
- Thread-safe operations ensuring data integrity across parallel scans

**Template-Based Detection**
- Extensible detection engine supporting 100+ CVE signatures
- Custom template creation for proprietary vulnerability patterns
- Hot-reload capability for template updates without scanner restart

**Flexible Target Input**
- Individual URL targets for focused assessment
- CIDR notation support for network range scanning
- File-based target lists for bulk operations
- Dynamic target generation via API integration

**Customizable Timeouts**
- Per-request timeout configuration to balance speed and reliability
- Connection timeout controls for network-constrained environments
- Read/write timeout granularity for fine-tuned performance optimization

**TLS/SSL Support**
- Full TLS 1.2 and 1.3 protocol support
- Configurable certificate validation policies
- Custom CA certificate injection for private PKI environments
- SNI (Server Name Indication) support for multi-domain servers

**Programmable APIs**
- RESTful API endpoints for remote scanner control
- Native language bindings (Python, Go) for direct library integration
- Event-driven callbacks for real-time scan progress monitoring
- Structured result objects with filtering and export capabilities

## Python Integration

Native Python bindings enable seamless workflow automation and custom security tooling development.

**Features:**
- **Python API Wrapper** - Direct access to scanner functionality via native bindings
- **Script Extensibility** - Develop custom vulnerability checks and detection logic
- **Automation Support** - Integration with existing Python-based security frameworks

### Python Examples

#### Basic Vulnerability Scan
```python
from govulnscanner import Scanner

# Initialize scanner with optimized settings
scanner = Scanner()
scanner.set_threads(10)
scanner.set_timeout(30)
scanner.set_verbose(True)

# Execute scan against multiple targets
targets = ['https://example.com', 'https://api.target.com']
results = scanner.scan_targets(targets)

# Process and export critical findings
results.filter_by_severity(['high', 'critical'])
results.export_to_json('vulnerabilities.json')
```

#### Custom Template Scan
```python
from govulnscanner import Scanner, Template

# Load custom vulnerability templates
scanner = Scanner()
scanner.load_templates_from_directory('./templates')
scanner.set_threads(20)

# Execute targeted scan with custom detection rules
targets = ['https://webapp.target.com']
results = scanner.scan_targets(targets)

# Generate executive summary report
results.generate_report('scan_report.html')
```
