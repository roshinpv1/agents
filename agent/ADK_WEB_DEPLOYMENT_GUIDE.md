# 🚀 **CodeGates Agent - ADK Web Deployment Guide**

## 📋 **Overview**

This guide provides step-by-step instructions for deploying and running the **CodeGates Validation Agent** using **Google ADK Web**. The agent is designed to validate enterprise codebases against compliance standards using LiteLLM integration.

## 🎯 **Prerequisites**

### **Required Software**
- Python 3.9+
- [Google ADK](https://github.com/google/adk-python) (Python Agent Development Kit)
- [LiteLLM](https://github.com/BerriAI/litellm) for LLM integration
- Git (for repository access)
- Access to CodeGates functionality

### **Required Accounts & API Keys**
- **LLM Provider**: Choose one of the following:
  - **Ollama** (Local LLM) - No API key required
  - **OpenAI** - API key from [OpenAI Platform](https://platform.openai.com/api-keys)
  - **Anthropic** - API key from [Anthropic Console](https://console.anthropic.com/)
  - **Google** - API key from [Google AI Studio](https://aistudio.google.com/apikey)
  - **Enterprise LLM** - Your enterprise LLM credentials

## 🔧 **Installation & Setup**

### **Step 1: Install Dependencies**

```bash
# Navigate to the agent directory
cd agent

# Install ADK and LiteLLM
pip install google-adk litellm

# Install additional dependencies
pip install requests pydantic python-dotenv
```

### **Step 2: Configure Environment Variables**

Create a `.env` file in the agent directory:

```bash
# Create .env file
touch .env
```

#### **For Local LLM (Ollama) - Recommended for Development**
```bash
# Add to .env file
CODEGATES_LLM_PROVIDER=ollama
CODEGATES_LLM_MODEL=llama2
CODEGATES_LLM_BASE_URL=http://localhost:11434
```

#### **For Cloud LLM (OpenAI)**
```bash
# Add to .env file
CODEGATES_LLM_PROVIDER=openai
CODEGATES_LLM_MODEL=gpt-3.5-turbo
CODEGATES_LLM_API_KEY=sk-your-openai-api-key
CODEGATES_LLM_BASE_URL=https://api.openai.com/v1
```

#### **For Cloud LLM (Anthropic)**
```bash
# Add to .env file
CODEGATES_LLM_PROVIDER=anthropic
CODEGATES_LLM_MODEL=claude-3-sonnet-20240229
CODEGATES_LLM_API_KEY=sk-ant-your-anthropic-api-key
```

#### **For Cloud LLM (Google)**
```bash
# Add to .env file
CODEGATES_LLM_PROVIDER=google
CODEGATES_LLM_MODEL=gemini-1.5-flash
CODEGATES_LLM_API_KEY=your-google-api-key
```

### **Step 3: Load Environment Variables**

```bash
# Load environment variables
set -o allexport && source .env && set +o allexport
```

### **Step 4: Verify Installation**

```bash
# Test the agent setup
python test_agent.py
```

## 🚀 **Running with ADK Web**

### **Method 1: Using ADK CLI (Recommended)**

#### **Step 1: Navigate to Agent Directory**
```bash
cd agent
```

#### **Step 2: Run with ADK Web**
```bash
# Start ADK web interface
adk web .

# Or run directly
adk run .
```

#### **Step 3: Access Web Interface**
- Open your browser and go to: `http://localhost:8080`
- You should see the ADK web interface with your CodeGates agent

### **Method 2: Using Python Directly**

#### **Step 1: Create a Runner Script**
Create `run_agent.py` in the agent directory:

```python
#!/usr/bin/env python3
"""
CodeGates Agent Runner for ADK Web
"""

import asyncio
import os
from pathlib import Path

# Add parent directory to path for imports
import sys
sys.path.append(str(Path(__file__).parent.parent))

from agent import create_codegates_runner

async def main():
    """Main function to run the agent"""
    
    # Create runner
    runner = create_codegates_runner()
    
    print("🚀 CodeGates Validation Agent Started")
    print("=" * 50)
    print("Agent is ready to accept validation requests!")
    print("Available commands:")
    print("- Validate repository: 'Validate the repository at https://github.com/company/myapp'")
    print("- List gates: 'What gates are available for validation?'")
    print("- Get help: 'Help me understand the validation process'")
    print("=" * 50)
    
    # Example interaction
    user_message = {
        "parts": [{
            "text": "Hello! Can you help me understand what gates are available for validation?"
        }]
    }
    
    # Run the agent
    async for event in runner.run_async(
        user_id="user123",
        session_id="session456",
        new_message=user_message
    ):
        print(f"Event: {event.type}")
        if hasattr(event, 'content') and event.content:
            print(f"Response: {event.content}")

if __name__ == "__main__":
    asyncio.run(main())
```

#### **Step 2: Run the Agent**
```bash
python run_agent.py
```

### **Method 3: Using ADK Web with Custom Configuration**

#### **Step 1: Create ADK Configuration**
Create `adk.yaml` in the agent directory:

```yaml
# ADK Configuration for CodeGates Agent
name: "codegates-validation-agent"
description: "Intelligent agent for enterprise gate validation"

# Agent configuration
agent:
  model: "${CODEGATES_LLM_MODEL:-gpt-3.5-turbo}"
  name: "codegates_validation_agent"
  description: "Intelligent agent for enterprise gate validation and compliance"

# Web interface configuration
web:
  port: 8080
  host: "0.0.0.0"
  debug: true

# Environment variables
env:
  - CODEGATES_LLM_PROVIDER
  - CODEGATES_LLM_MODEL
  - CODEGATES_LLM_API_KEY
  - CODEGATES_LLM_BASE_URL
```

#### **Step 2: Run with Configuration**
```bash
adk web . --config adk.yaml
```

## 🌐 **Web Interface Usage**

### **Accessing the Web Interface**

1. **Start the agent** using one of the methods above
2. **Open your browser** and navigate to: `http://localhost:8080`
3. **You'll see the ADK web interface** with your CodeGates agent

### **Using the Web Interface**

#### **1. Chat Interface**
- **Type your request** in the chat input
- **Examples**:
  - `"Validate the repository at https://github.com/company/myapp"`
  - `"What gates are applicable for a Python web application?"`
  - `"Generate a report for the validation results"`
  - `"Collect evidence from Splunk and AppDynamics"`

#### **2. Tool Selection**
- The agent will **automatically select appropriate tools**
- You can see which tools are being used in the interface
- **Available tools**:
  - `analyze_repository`: Clone and analyze repository structure
  - `validate_gates`: Validate specific gates against the codebase
  - `collect_evidence`: Collect evidence from external sources
  - `generate_report`: Generate comprehensive validation reports

#### **3. Real-time Feedback**
- **Live progress updates** as the agent works
- **Tool execution status** displayed in real-time
- **Results and recommendations** presented clearly

## 🔧 **Configuration Options**

### **Environment Variables Reference**

| Variable | Description | Example |
|----------|-------------|---------|
| `CODEGATES_LLM_PROVIDER` | LLM provider to use | `ollama`, `openai`, `anthropic`, `google` |
| `CODEGATES_LLM_MODEL` | Model name | `llama2`, `gpt-3.5-turbo`, `claude-3-sonnet` |
| `CODEGATES_LLM_API_KEY` | API key for cloud providers | `sk-your-api-key` |
| `CODEGATES_LLM_BASE_URL` | Base URL for LLM service | `http://localhost:11434` |

### **Advanced Configuration**

#### **Custom Model Configuration**
```bash
# Add to .env file
LITELLM_MODEL=gpt-4
LITELLM_TEMPERATURE=0.7
LITELLM_MAX_TOKENS=2000
LITELLM_TIMEOUT=60
```

#### **Provider-Specific Configuration**
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

## 🧪 **Testing the Agent**

### **Test Scripts**

#### **1. Basic Functionality Test**
```bash
# Run the test script
python test_agent.py
```

#### **2. Manual Testing**
```bash
# Start the agent
adk web .

# In the web interface, try these commands:
# - "Hello! Can you help me with repository validation?"
# - "What gates are available for validation?"
# - "Explain the ALERTING_ACTIONABLE gate requirements"
```

#### **3. Repository Validation Test**
```bash
# In the web interface, try:
# "Validate the repository at https://github.com/company/myapp"
```

## 🚀 **Deployment Options**

### **Local Development**
```bash
# Start Ollama (if using local LLM)
ollama serve

# Run agent
adk web .
```

### **Docker Deployment**
```dockerfile
# Dockerfile
FROM python:3.9

# Install dependencies
RUN pip install google-adk litellm

# Install Ollama (if using local LLM)
RUN curl -fsSL https://ollama.ai/install.sh | sh

# Copy application
COPY . /app
WORKDIR /app

# Expose port
EXPOSE 8080

# Start services
CMD ["adk", "web", "."]
```

### **Cloud Deployment**
```bash
# Deploy to Google Cloud Run
gcloud run deploy codegates-agent \
  --source . \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --port 8080
```

## 🔍 **Troubleshooting**

### **Common Issues**

#### **1. ADK Not Found**
```bash
# Install ADK
pip install google-adk

# Verify installation
adk --version
```

#### **2. LiteLLM Not Available**
```bash
# Install LiteLLM
pip install litellm

# Verify installation
python -c "import litellm; print(litellm.__version__)"
```

#### **3. Environment Variables Not Loaded**
```bash
# Load environment variables
set -o allexport && source .env && set +o allexport

# Verify variables
echo $CODEGATES_LLM_PROVIDER
echo $CODEGATES_LLM_MODEL
```

#### **4. Ollama Not Running**
```bash
# Start Ollama
ollama serve

# Verify Ollama is running
curl http://localhost:11434/api/tags
```

#### **5. Port Already in Use**
```bash
# Check what's using port 8080
lsof -i :8080

# Kill the process or use a different port
adk web . --port 8081
```

### **Debug Mode**

#### **Enable Debug Logging**
```bash
# Set debug environment variable
export ADK_DEBUG=true

# Run with debug output
adk web . --debug
```

#### **Verbose Output**
```bash
# Run with verbose output
adk web . --verbose
```

## 📚 **Example Interactions**

### **Repository Validation**
```
User: "Validate the repository at https://github.com/company/myapp"
Agent: "I'll help you validate that repository. Let me start by analyzing the codebase structure and determining which gates are applicable..."
```

### **Gate Information**
```
User: "What gates are available for validation?"
Agent: "I can validate your codebase against 15 enterprise gates including ALERTING_ACTIONABLE, STRUCTURED_LOGS, AVOID_LOGGING_SECRETS..."
```

### **Evidence Collection**
```
User: "Collect evidence from Splunk and AppDynamics for my application"
Agent: "I'll collect evidence from Splunk and AppDynamics to support the validation. Let me query these systems..."
```

## 🎯 **Next Steps**

### **Immediate Actions**
1. **Install Dependencies**: Follow the installation steps above
2. **Configure Environment**: Set up your preferred LLM provider
3. **Test the Agent**: Run the test scripts and try the web interface
4. **Validate a Repository**: Test with a real repository

### **Advanced Usage**
1. **Custom Tools**: Add your own validation tools
2. **Integration**: Integrate with your existing CI/CD pipeline
3. **Deployment**: Deploy to production environment
4. **Monitoring**: Set up monitoring and logging

## 📞 **Support**

For issues and questions:
- **Check the troubleshooting section** above
- **Review the agent logs** for error messages
- **Test with a simple repository** first
- **Verify your LLM configuration** is correct

## 🎉 **Success Indicators**

You'll know the agent is working correctly when:
- ✅ **Web interface loads** at `http://localhost:8080`
- ✅ **Agent responds** to basic queries
- ✅ **Tools execute** without errors
- ✅ **LLM integration** works with your configured provider
- ✅ **Repository validation** completes successfully

---

**Happy validating! 🚀** 