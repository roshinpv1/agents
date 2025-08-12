# CodeGates Validation Agent

Intelligent agent for enterprise gate validation using Google ADK with LiteLLM integration.

## Overview

The CodeGates Validation Agent is a sophisticated AI-powered system that helps developers and organizations validate their codebases against enterprise standards and compliance requirements. Built using Google's Agent Development Kit (ADK) and integrated with LiteLLM, it provides comprehensive validation capabilities with support for local and cloud LLMs.

## Features

### 🎯 **Core Capabilities**
- **Repository Analysis**: Clone and analyze codebase structure and technologies
- **Gate Validation**: Validate 15+ enterprise hard gates against codebases
- **Evidence Collection**: Gather evidence from Splunk, AppDynamics, and web portals
- **Report Generation**: Create comprehensive validation reports with recommendations
- **Multi-LLM Support**: Works with local LLMs (Ollama) and cloud providers

### 🔧 **Supported LLM Providers**
- **Local LLMs**: Ollama (llama2, mistral, etc.)
- **Cloud Providers**: OpenAI, Anthropic, Google
- **Enterprise**: Custom enterprise LLM deployments

### 🏗️ **Enterprise Gates**
- **ALERTING_ACTIONABLE**: Ensure all alerting integrations are present
- **STRUCTURED_LOGS**: Ensure logs are structured and searchable
- **AVOID_LOGGING_SECRETS**: Prevent sensitive data logging
- **AUDIT_TRAIL**: Ensure proper audit trails
- **CORRELATION_ID**: Ensure correlation IDs for tracing
- **LOG_API_CALLS**: Ensure API calls are logged
- **CLIENT_UI_ERRORS**: Ensure client-side error tracking
- **RETRY_LOGIC**: Ensure proper retry mechanisms
- **TIMEOUT_IO**: Ensure proper timeout handling
- **THROTTLING**: Ensure rate limiting implementation
- **CIRCUIT_BREAKERS**: Ensure circuit breaker patterns
- **HTTP_ERROR_CODES**: Ensure proper HTTP error handling
- **URL_MONITORING**: Ensure URL monitoring
- **AUTOMATED_TESTS**: Ensure comprehensive test coverage
- **AUTO_SCALE**: Ensure auto-scaling capabilities

## Installation

### Prerequisites

- Python 3.9+
- Google ADK
- LiteLLM
- Access to CodeGates functionality

### Quick Start

1. **Install Dependencies**
   ```bash
   pip install google-adk litellm
   ```

2. **Configure Environment**
   ```bash
   # For local LLM (Ollama)
   export CODEGATES_LLM_PROVIDER=ollama
   export CODEGATES_LLM_MODEL=llama2
   export CODEGATES_LLM_BASE_URL=http://localhost:11434
   
   # For cloud LLM (OpenAI)
   export CODEGATES_LLM_PROVIDER=openai
   export CODEGATES_LLM_MODEL=gpt-3.5-turbo
   export CODEGATES_LLM_API_KEY=your-api-key
   ```

3. **Run the Agent**
   ```bash
   python agent/codegates_agent.py
   ```

## Usage

### Basic Usage

```python
from agent import root_agent, create_codegates_runner

# Create runner
runner = create_codegates_runner()

# Example validation request
user_message = {
    "parts": [{
        "text": "Validate the repository at https://github.com/company/myapp"
    }]
}

# Run validation
async for event in runner.run_async(
    user_id="user123",
    session_id="session456",
    new_message=user_message
):
    print(f"Event: {event.type}")
    if hasattr(event, 'content'):
        print(f"Content: {event.content}")
```

### Advanced Usage

```python
import asyncio
from agent.codegates_agent import RepositoryAnalysisTool, GateValidationTool

async def custom_validation():
    # Analyze repository
    repo_tool = RepositoryAnalysisTool()
    repo_result = await repo_tool.run_async({
        "repository_url": "https://github.com/company/myapp",
        "branch": "main"
    }, None)
    
    # Validate specific gates
    validation_tool = GateValidationTool()
    validation_result = await validation_tool.run_async({
        "gate_names": ["ALERTING_ACTIONABLE", "STRUCTURED_LOGS"],
        "repository_path": repo_result["repository_path"],
        "app_id": "myapp"
    }, None)
    
    print(f"Validation Results: {validation_result}")

# Run custom validation
asyncio.run(custom_validation())
```

## Configuration

### Environment Variables

#### CodeGates LLM Configuration
```bash
# Provider selection
CODEGATES_LLM_PROVIDER=ollama|openai|anthropic|google|enterprise

# Model configuration
CODEGATES_LLM_MODEL=llama2|gpt-3.5-turbo|claude-3-sonnet|gemini-1.5-flash

# API configuration
CODEGATES_LLM_API_KEY=your-api-key
CODEGATES_LLM_BASE_URL=http://localhost:11434|https://api.openai.com/v1
```

#### LiteLLM Configuration
```bash
# LiteLLM-specific settings
LITELLM_MODEL=gpt-3.5-turbo
LITELLM_API_KEY=your-api-key
LITELLM_BASE_URL=https://api.openai.com/v1
LITELLM_TEMPERATURE=0.7
LITELLM_MAX_TOKENS=2000
LITELLM_TIMEOUT=60
```

#### Provider-Specific Configuration
```bash
# Ollama
OLLAMA_BASE_URL=http://localhost:11434

# OpenAI
OPENAI_API_KEY=your-openai-key
OPENAI_API_BASE=https://api.openai.com/v1

# Anthropic
ANTHROPIC_API_KEY=your-anthropic-key

# Google
GOOGLE_API_KEY=your-google-key
```

### Configuration Examples

#### Local Development with Ollama
```bash
export CODEGATES_LLM_PROVIDER=ollama
export CODEGATES_LLM_MODEL=llama2
export CODEGATES_LLM_BASE_URL=http://localhost:11434
```

#### Production with OpenAI
```bash
export CODEGATES_LLM_PROVIDER=openai
export CODEGATES_LLM_MODEL=gpt-4
export CODEGATES_LLM_API_KEY=sk-your-openai-key
export CODEGATES_LLM_BASE_URL=https://api.openai.com/v1
```

#### Enterprise Deployment
```bash
export CODEGATES_LLM_PROVIDER=enterprise
export CODEGATES_LLM_MODEL=enterprise-model-v1
export CODEGATES_LLM_BASE_URL=https://enterprise-llm.company.com
export CODEGATES_LLM_API_KEY=your-enterprise-key
```

## Architecture

### Agent Structure

```
CodeGates Agent (ADK)
├── Repository Analysis Tool
├── Gate Validation Tool
├── Evidence Collection Tool
└── Report Generation Tool
```

### Integration Layers

```
User Request
    ↓
CodeGates Agent
    ↓
LiteLLM Integration
    ↓
CodeGates LLM Configuration
    ↓
LLM Provider (Local/Cloud)
```

### Tool Flow

```
1. Repository Analysis
   ├── Clone repository
   ├── Scan file structure
   ├── Analyze technologies
   └── Determine applicable gates

2. Gate Validation
   ├── Execute pattern matching
   ├── Run criteria evaluation
   ├── Collect validation results
   └── Generate recommendations

3. Evidence Collection
   ├── Splunk integration
   ├── AppDynamics integration
   ├── Web portal evidence
   └── External data aggregation

4. Report Generation
   ├── Create summary
   ├── Generate recommendations
   ├── Format detailed report
   └── Provide actionable insights
```

## Development

### Project Structure

```
agent/
├── __init__.py              # Main module exports
├── codegates_agent.py       # Main agent implementation
├── litellm_config.py        # LiteLLM configuration
├── pyproject.toml          # Project configuration
└── README.md               # This file
```

### Adding New Tools

1. **Create Tool Class**
   ```python
   class NewTool(BaseTool):
       name = "new_tool"
       description = "Description of the new tool"
       
       async def run_async(self, args: dict, tool_context) -> dict:
           # Tool implementation
           return {"status": "success", "data": result}
   ```

2. **Register Tool**
   ```python
   # In create_codegates_agent()
   tools = [
       RepositoryAnalysisTool(),
       GateValidationTool(),
       EvidenceCollectionTool(),
       ReportGenerationTool(),
       NewTool()  # Add new tool
   ]
   ```

3. **Update Instructions**
   ```python
   # In AGENT_INSTRUCTION
   **TOOLS AVAILABLE:**
   1. **analyze_repository**: Clone and analyze repository structure
   2. **validate_gates**: Validate specific gates against the codebase
   3. **collect_evidence**: Collect evidence from external sources
   4. **generate_report**: Generate comprehensive validation reports
   5. **new_tool**: Description of the new tool  # Add new tool
   ```

### Testing

```bash
# Run tests
pytest tests/

# Run with coverage
pytest --cov=agent tests/

# Run specific test
pytest tests/test_agent.py::test_repository_analysis
```

## Deployment

### Local Development

```bash
# Start Ollama
ollama serve

# Run agent
python agent/codegates_agent.py
```

### Docker Deployment

```dockerfile
FROM python:3.9

# Install dependencies
RUN pip install google-adk litellm

# Install Ollama
RUN curl -fsSL https://ollama.ai/install.sh | sh

# Copy application
COPY . /app
WORKDIR /app

# Start services
CMD ["python", "agent/codegates_agent.py"]
```

### Kubernetes Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: codegates-agent
spec:
  replicas: 3
  selector:
    matchLabels:
      app: codegates-agent
  template:
    metadata:
      labels:
        app: codegates-agent
    spec:
      containers:
      - name: codegates-agent
        image: codegates/agent:latest
        env:
        - name: CODEGATES_LLM_PROVIDER
          value: "enterprise"
        - name: CODEGATES_LLM_BASE_URL
          value: "https://enterprise-llm.company.com"
```

## Troubleshooting

### Common Issues

#### LiteLLM Not Available
```bash
# Install LiteLLM
pip install litellm
```

#### Google ADK Not Available
```bash
# Install Google ADK
pip install google-adk
```

#### CodeGates Functionality Not Available
```bash
# Ensure CodeGates is properly installed
pip install -r gates/requirements.txt
```

#### LLM Configuration Issues
```bash
# Check environment variables
echo $CODEGATES_LLM_PROVIDER
echo $CODEGATES_LLM_MODEL
echo $CODEGATES_LLM_BASE_URL
```

### Debug Mode

```python
import logging

# Enable debug logging
logging.basicConfig(level=logging.DEBUG)

# Run agent with debug output
from agent import root_agent
print(f"Agent created: {root_agent.name}")
print(f"Available tools: {len(root_agent.tools)}")
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests
5. Submit a pull request

## License

This project is licensed under the Apache License 2.0 - see the LICENSE file for details.

## Support

For support and questions:
- Create an issue on GitHub
- Contact the CodeGates team
- Check the documentation at https://codegates.com/docs 