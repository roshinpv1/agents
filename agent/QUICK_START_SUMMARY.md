# 🚀 **CodeGates Agent - Quick Start Guide**

## 📋 **Overview**

This guide provides the fastest way to get the **CodeGates Validation Agent** running with **ADK Web**. The agent validates enterprise codebases against compliance standards using LiteLLM integration.

## ⚡ **Quick Start (5 Minutes)**

### **Step 1: Navigate to Agent Directory**
```bash
cd agent
```

### **Step 2: Run Quick Start Script**
```bash
./quick_start.sh
```

This script will:
- ✅ Install all dependencies
- ✅ Create configuration file
- ✅ Test the agent setup
- ✅ Provide next steps

### **Step 3: Configure Your LLM**

Edit the `.env` file with your preferred LLM:

#### **Option A: Local LLM (Ollama) - Recommended**
```bash
# Install Ollama
curl -fsSL https://ollama.ai/install.sh | sh

# Start Ollama
ollama serve

# Pull a model
ollama pull llama2
```

#### **Option B: Cloud LLM (OpenAI)**
```bash
# Edit .env file
CODEGATES_LLM_PROVIDER=openai
CODEGATES_LLM_MODEL=gpt-3.5-turbo
CODEGATES_LLM_API_KEY=sk-your-openai-api-key
```

### **Step 4: Run with ADK Web**
```bash
# Start the agent
adk web .

# Open browser to: http://localhost:8080
```

## 🎯 **Try These Commands**

Once the web interface is running, try these commands:

### **Basic Commands**
- `"Hello! Can you help me with repository validation?"`
- `"What gates are available for validation?"`
- `"Help me understand the validation process"`

### **Repository Validation**
- `"Validate the repository at https://github.com/company/myapp"`
- `"What gates are applicable for a Python web application?"`
- `"Generate a report for the validation results"`

### **Evidence Collection**
- `"Collect evidence from Splunk and AppDynamics"`
- `"Explain the ALERTING_ACTIONABLE gate requirements"`

## 🔧 **Configuration Options**

### **Environment Variables**
```bash
# LLM Provider
CODEGATES_LLM_PROVIDER=ollama|openai|anthropic|google

# Model
CODEGATES_LLM_MODEL=llama2|gpt-3.5-turbo|claude-3-sonnet|gemini-1.5-flash

# API Key (for cloud providers)
CODEGATES_LLM_API_KEY=your-api-key

# Base URL
CODEGATES_LLM_BASE_URL=http://localhost:11434|https://api.openai.com/v1
```

### **Supported LLM Providers**
- **Ollama** (Local) - No API key required
- **OpenAI** - GPT-3.5, GPT-4
- **Anthropic** - Claude models
- **Google** - Gemini models
- **Enterprise** - Custom LLMs

## 🛠️ **Available Tools**

The agent has these tools available:
- **analyze_repository**: Clone and analyze repository structure
- **validate_gates**: Validate specific gates against the codebase
- **collect_evidence**: Collect evidence from external sources
- **generate_report**: Generate comprehensive validation reports

## 🏗️ **Enterprise Gates**

The agent validates against 15+ enterprise gates:
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

## 🚀 **Alternative Methods**

### **Method 1: Direct Python**
```bash
python run_agent.py
```

### **Method 2: With Configuration**
```bash
adk web . --config adk.yaml
```

### **Method 3: Custom Port**
```bash
adk web . --port 8081
```

## 🔍 **Troubleshooting**

### **Common Issues**

#### **ADK Not Found**
```bash
pip install google-adk
```

#### **LiteLLM Not Available**
```bash
pip install litellm
```

#### **Ollama Not Running**
```bash
ollama serve
```

#### **Port Already in Use**
```bash
adk web . --port 8081
```

### **Debug Mode**
```bash
export ADK_DEBUG=true
adk web . --debug
```

## 📚 **Example Workflow**

### **Complete Repository Validation**
1. **Start the agent**: `adk web .`
2. **Open browser**: `http://localhost:8080`
3. **Type command**: `"Validate the repository at https://github.com/company/myapp"`
4. **Watch the agent**:
   - Clone and analyze repository
   - Determine applicable gates
   - Validate each gate
   - Collect evidence
   - Generate comprehensive report
5. **Review results** and recommendations

## 🎉 **Success Indicators**

You'll know it's working when:
- ✅ Web interface loads at `http://localhost:8080`
- ✅ Agent responds to basic queries
- ✅ Tools execute without errors
- ✅ LLM integration works
- ✅ Repository validation completes

## 📖 **More Information**

- **Full Documentation**: `README.md`
- **Deployment Guide**: `ADK_WEB_DEPLOYMENT_GUIDE.md`
- **Test Script**: `test_agent.py`
- **Configuration**: `adk.yaml`

## 🚀 **Next Steps**

1. **Test with a real repository**
2. **Customize the agent** for your needs
3. **Add custom tools** for specific validations
4. **Deploy to production** environment
5. **Integrate with CI/CD** pipeline

---

**Happy validating! 🎯** 