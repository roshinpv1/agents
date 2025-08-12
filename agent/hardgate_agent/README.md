# HardGate Agent

Enterprise-grade code security analysis using Google ADK with LiteLLM integration.

## Overview

The HardGate Agent is a comprehensive security analysis tool that combines the power of Google's Agent Development Kit (ADK) with LiteLLM for intelligent code security analysis. It provides automated validation of hard gates, security scanning, compliance checking, and detailed reporting.

## Features

### 🔍 **Comprehensive Analysis**
- Repository structure analysis and technology detection
- Security vulnerability scanning
- Hard gate validation against enterprise standards
- Evidence collection from external sources
- Risk assessment and threat modeling

### 🛡️ **Security Tools**
- **Repository Analysis**: Understand codebase structure and technologies
- **Security Scanning**: Detect vulnerabilities, secrets, and security issues
- **Gate Validation**: Validate against 15+ enterprise security gates
- **Evidence Collection**: Gather data from Splunk, AppDynamics, and web portals
- **Security Analysis**: Comprehensive risk assessment and threat modeling
- **Compliance Checking**: Validate against SOC2, ISO27001, NIST, and enterprise standards
- **LLM Analysis**: AI-powered insights and recommendations
- **Report Generation**: Professional security and compliance reports

### 🎯 **Hard Gates Supported**
- **ALERTING_ACTIONABLE**: Ensure all alerting integrations are present and actionable
- **STRUCTURED_LOGS**: Ensure logs are structured, searchable, and properly formatted
- **AVOID_LOGGING_SECRETS**: Prevent sensitive data and secrets from being logged
- **AUDIT_TRAIL**: Ensure comprehensive audit trails for all critical operations
- **CORRELATION_ID**: Ensure correlation IDs for distributed tracing
- **LOG_API_CALLS**: Ensure all API calls are properly logged
- **CLIENT_UI_ERRORS**: Ensure client-side error tracking and monitoring
- **RETRY_LOGIC**: Ensure proper retry mechanisms with exponential backoff
- **TIMEOUT_IO**: Ensure proper timeout handling for all I/O operations
- **THROTTLING**: Ensure rate limiting and throttling implementation
- **CIRCUIT_BREAKERS**: Ensure circuit breaker patterns for fault tolerance
- **HTTP_ERROR_CODES**: Ensure proper HTTP error handling and status codes
- **URL_MONITORING**: Ensure URL monitoring and health checks
- **AUTOMATED_TESTS**: Ensure comprehensive automated test coverage
- **AUTO_SCALE**: Ensure auto-scaling capabilities and configuration

## Installation

### Prerequisites

1. **Python 3.8+**
2. **Google ADK**: `pip install google-adk`
3. **LiteLLM**: `pip install litellm`
4. **Additional dependencies**: See `requirements.txt`

### Quick Setup

```bash
# Clone the repository
git clone <repository-url>
cd agent/hardgate_agent

# Install dependencies
pip install -r requirements.txt

# Set up environment variables
export LITELLM_MODEL="gpt-3.5-turbo"
export LITELLM_BASE_URL="http://localhost:1234/v1"
export LITELLM_API_KEY="your-api-key"
export LITELLM_PROVIDER="openai"
```

## Configuration

### Environment Variables

```bash
# LiteLLM Configuration
export LITELLM_MODEL="gpt-3.5-turbo"
export LITELLM_BASE_URL="http://localhost:1234/v1"
export LITELLM_API_KEY="your-api-key"
export LITELLM_PROVIDER="openai"
export LITELLM_TEMPERATURE="0.3"
export LITELLM_MAX_TOKENS="2000"

# Agent Configuration
export HARDGATE_MAX_CONCURRENT="5"
export HARDGATE_ANALYSIS_TIMEOUT="300"
export HARDGATE_SCAN_DEPTH="comprehensive"

# External Integrations
export SPLUNK_ENABLED="true"
export SPLUNK_URL="https://your-splunk-instance"
export SPLUNK_USERNAME="your-username"
export SPLUNK_PASSWORD="your-password"

export APPDYNAMICS_ENABLED="true"
export APPDYNAMICS_URL="https://your-appdynamics-instance"
export APPDYNAMICS_USERNAME="your-username"
export APPDYNAMICS_PASSWORD="your-password"

export JIRA_ENABLED="true"
export JIRA_URL="https://your-jira-instance"
export JIRA_USERNAME="your-username"
export JIRA_API_TOKEN="your-api-token"

# Report Configuration
export HARDGATE_REPORT_FORMAT="json"
export HARDGATE_OUTPUT_DIR="./reports"
export HARDGATE_INCLUDE_APPENDIX="true"
export HARDGATE_INCLUDE_EVIDENCE="true"
```

## Usage

### Basic Usage

```python
import asyncio
from hardgate_agent import hardgate_agent

async def main():
    # Run complete analysis
    result = await hardgate_agent.run_complete_analysis(
        repository_url="https://github.com/example/security-app",
        branch="main",
        app_id="my-app-123"
    )
    
    if result["success"]:
        print("✅ Analysis completed successfully")
        print(f"📊 Results: {result['workflow_results']}")
    else:
        print(f"❌ Analysis failed: {result['error']}")

# Run the analysis
asyncio.run(main())
```

### Individual Tool Usage

```python
import asyncio
from hardgate_agent import (
    analyze_repository,
    validate_gates,
    perform_security_scan,
    collect_evidence,
    run_complete_analysis
)

async def individual_tools():
    # 1. Analyze repository
    repo_analysis = await analyze_repository(
        repository_url="https://github.com/example/app",
        branch="main"
    )
    
    # 2. Validate specific gates
    gate_validation = await validate_gates(
        repository_path="/path/to/repo",
        gates=["STRUCTURED_LOGS", "AUTHENTICATION", "AUTHORIZATION"]
    )
    
    # 3. Perform security scan
    security_scan = await perform_security_scan(
        repository_path="/path/to/repo",
        scan_type="comprehensive"
    )
    
    # 4. Collect evidence
    evidence = await collect_evidence(
        app_id="my-app-123",
        sources=["splunk", "appdynamics"],
        time_range="24h"
    )

asyncio.run(individual_tools())
```

### Command Line Usage

```bash
# Run complete analysis
python -m hardgate_agent.agent --repository https://github.com/example/app --branch main

# Validate specific gates
python -m hardgate_agent.agent --validate-gates --repository /path/to/repo --gates STRUCTURED_LOGS,AUTHENTICATION

# Generate report only
python -m hardgate_agent.agent --generate-report --analysis-data /path/to/data.json
```

## Architecture

### Components

```
hardgate_agent/
├── agent.py              # Main agent implementation
├── config.py             # Configuration management
├── prompt.py             # Agent prompts and instructions
├── tools/                # Tool implementations
│   ├── repository_analysis.py
│   ├── gate_validation.py
│   ├── code_scanning.py
│   ├── evidence_collection.py
│   ├── security_analysis.py
│   ├── compliance_check.py
│   ├── llm_analysis.py
│   └── report_generation.py
└── README.md
```

### Tool Architecture

Each tool follows the Google ADK pattern:

```python
class ToolName(BaseTool):
    name = "tool_name"
    description = "Tool description"
    
    async def run_async(self, args: dict, tool_context: ToolContext) -> dict:
        # Tool implementation
        pass
```

## Integration Examples

### CI/CD Integration

```yaml
# GitHub Actions example
name: Security Analysis
on: [push, pull_request]

jobs:
  security-analysis:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.9'
      
      - name: Install dependencies
        run: |
          pip install google-adk litellm
          pip install -r agent/hardgate_agent/requirements.txt
      
      - name: Run HardGate Analysis
        run: |
          python -m hardgate_agent.agent \
            --repository ${{ github.repository }} \
            --branch ${{ github.ref }} \
            --output-format json \
            --output-path ./security-report.json
      
      - name: Upload Security Report
        uses: actions/upload-artifact@v2
        with:
          name: security-report
          path: ./security-report.json
```

### API Integration

```python
from fastapi import FastAPI
from hardgate_agent import hardgate_agent

app = FastAPI()

@app.post("/analyze")
async def analyze_repository(request: dict):
    result = await hardgate_agent.run_complete_analysis(
        repository_url=request["repository_url"],
        branch=request.get("branch", "main"),
        app_id=request.get("app_id")
    )
    return result

@app.get("/health")
async def health_check():
    return {"status": "healthy", "agent": "HardGate Agent"}
```

## Reports

### Report Types

1. **Comprehensive Report**: Full analysis with all details
2. **Executive Report**: High-level summary for stakeholders
3. **Technical Report**: Detailed technical analysis
4. **Compliance Report**: Compliance-focused analysis

### Report Formats

- **JSON**: Machine-readable format
- **HTML**: Web-friendly format with styling
- **Markdown**: Documentation-friendly format

### Sample Report Structure

```json
{
  "report_metadata": {
    "report_id": "SEC-20241201-143022",
    "generated_at": "2024-12-01T14:30:22",
    "version": "1.0"
  },
  "executive_summary": {
    "overall_security_score": 85,
    "risk_level": "Medium",
    "gate_compliance_rate": 80.0,
    "total_vulnerabilities": 5,
    "critical_vulnerabilities": 1
  },
  "detailed_analysis": {
    "repository_analysis": {...},
    "security_scan": {...},
    "gate_validation": {...},
    "evidence_collection": {...}
  },
  "recommendations": [...],
  "action_items": [...]
}
```

## Troubleshooting

### Common Issues

1. **Google ADK Not Available**
   ```bash
   pip install google-adk
   ```

2. **LiteLLM Connection Issues**
   - Verify LiteLLM server is running
   - Check API key and base URL configuration
   - Ensure network connectivity

3. **Repository Access Issues**
   - Verify repository URL is correct
   - Check authentication tokens
   - Ensure repository is accessible

4. **External Integration Failures**
   - Verify integration credentials
   - Check network connectivity
   - Ensure services are running

### Debug Mode

```bash
export HARDGATE_DEBUG="true"
python -m hardgate_agent.agent --debug
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

### Development Setup

```bash
# Clone repository
git clone <repository-url>
cd agent/hardgate_agent

# Install development dependencies
pip install -r requirements-dev.txt

# Run tests
pytest tests/

# Run linting
flake8 .
black .
```

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For support and questions:

1. Check the documentation
2. Review troubleshooting section
3. Open an issue on GitHub
4. Contact the development team

## Roadmap

### Upcoming Features

- [ ] **Advanced LLM Integration**: Support for multiple LLM providers
- [ ] **Custom Gate Definitions**: User-defined security gates
- [ ] **Real-time Monitoring**: Continuous security monitoring
- [ ] **Integration APIs**: REST APIs for external integrations
- [ ] **Dashboard**: Web-based dashboard for results visualization
- [ ] **Team Collaboration**: Multi-user support and role-based access
- [ ] **Automated Remediation**: Automated fix suggestions and implementation
- [ ] **Compliance Automation**: Automated compliance reporting and tracking

### Version History

- **v1.0.0**: Initial release with core functionality
- **v1.1.0**: Enhanced LLM integration and reporting
- **v1.2.0**: Additional compliance frameworks and tools 