# 🚀 **LiteLLM Integration with CodeGates Agent**

## 📋 **Executive Summary**

This document provides a comprehensive overview of the **LiteLLM integration** with the **CodeGates agentic application**. The integration enables the agent to use the **commonly configured LLM** for the application (including local LLMs like Ollama) while maintaining compatibility with the existing CodeGates infrastructure.

## 🎯 **Integration Overview**

### **Key Benefits**
- **Unified LLM Configuration**: Uses existing CodeGates LLM configuration
- **Local LLM Support**: Supports local LLMs like Ollama
- **Provider Flexibility**: Works with OpenAI, Anthropic, Google, and custom providers
- **Seamless Integration**: Minimal changes to existing CodeGates functionality
- **Enterprise Ready**: Supports enterprise LLM deployments

### **Supported LLM Providers**
- **OpenAI**: GPT-3.5, GPT-4, and custom models
- **Anthropic**: Claude models
- **Google**: Gemini models
- **Ollama**: Local models (llama2, mistral, etc.)
- **Enterprise**: Custom enterprise LLM deployments

## 🏗️ **Architecture**

### **Integration Layers**

```
CodeGates Agent (ADK)
├── LiteLLM Integration Layer
│   ├── Configuration Manager
│   ├── Provider Mapping
│   └── Response Handler
├── CodeGates LLM Configuration
│   ├── Environment Variables
│   ├── Provider Settings
│   └── Model Configuration
└── LiteLLM Library
    ├── Provider Support
    ├── Async Completion
    └── Tool Integration
```

### **Configuration Flow**

```
1. Load CodeGates LLM Config
   ↓
2. Map Provider to LiteLLM
   ↓
3. Set Environment Variables
   ↓
4. Configure LiteLLM Client
   ↓
5. Use in Agent Tools
```

## 🔧 **Implementation Details**

### **1. LiteLLM Integration Class**

```python
class LiteLLMIntegration:
    """Integration layer for LiteLLM with CodeGates configuration"""
    
    def __init__(self):
        self.llm_client = None
        self.llm_config = None
        self._initialize_llm()
    
    def _initialize_llm(self):
        """Initialize LLM client using CodeGates configuration"""
        # Use existing CodeGates LLM configuration
        self.llm_client = create_llm_client_from_env()
        self.llm_config = LLMConfig.from_env()
        
        # Configure LiteLLM with the same settings
        self._configure_litellm()
    
    async def generate_response(self, messages: List[Dict[str, str]], tools: List[Dict] = None) -> Dict[str, Any]:
        """Generate LLM response using LiteLLM"""
        # Use LiteLLM for completion with CodeGates configuration
        response = await litellm.acompletion(**params)
        return response
```

### **2. Configuration Manager**

```python
class LiteLLMConfig:
    """Configuration manager for LiteLLM integration"""
    
    def configure_litellm_from_codegates(self):
        """Configure LiteLLM using CodeGates settings"""
        # Map CodeGates provider to LiteLLM provider
        provider_mapping = {
            LLMProvider.OPENAI: "openai",
            LLMProvider.ANTHROPIC: "anthropic", 
            LLMProvider.GOOGLE: "google",
            LLMProvider.OLLAMA: "ollama",
            LLMProvider.ENTERPRISE: "custom"
        }
        
        # Set environment variables for LiteLLM
        if self.codegates_config.api_key:
            os.environ["LITELLM_API_KEY"] = self.codegates_config.api_key
        
        if self.codegates_config.base_url:
            os.environ["LITELLM_BASE_URL"] = self.codegates_config.base_url
```

### **3. Agent Integration**

```python
class CodeGatesAgent(LlmAgent):
    """Main CodeGates validation agent with LiteLLM integration"""
    
    def __init__(self):
        # Initialize LiteLLM integration
        self.litellm_integration = LiteLLMIntegration()
        
        super().__init__(
            name="codegates_agent",
            description="Intelligent agent for enterprise gate validation",
            tools=[...]
        )
    
    async def _generate_llm_response(self, messages: List[Dict[str, str]], tools: List[Dict] = None) -> Dict[str, Any]:
        """Generate LLM response using LiteLLM"""
        return await self.litellm_integration.generate_response(messages, tools)
```

## 🔄 **Configuration Options**

### **1. CodeGates Configuration (Recommended)**

The integration automatically uses the existing CodeGates LLM configuration:

```bash
# Environment variables (existing CodeGates config)
CODEGATES_LLM_PROVIDER=ollama
CODEGATES_LLM_MODEL=llama2
CODEGATES_LLM_BASE_URL=http://localhost:11434
CODEGATES_LLM_API_KEY=your-api-key
```

### **2. LiteLLM-Specific Configuration**

You can also configure LiteLLM directly:

```bash
# LiteLLM environment variables
LITELLM_MODEL=gpt-3.5-turbo
LITELLM_API_KEY=your-api-key
LITELLM_BASE_URL=https://api.openai.com/v1
LITELLM_TEMPERATURE=0.7
LITELLM_MAX_TOKENS=2000
LITELLM_TIMEOUT=60
```

### **3. Provider-Specific Configuration**

#### **Ollama (Local LLM)**
```bash
OLLAMA_BASE_URL=http://localhost:11434
LITELLM_MODEL=llama2
LITELLM_PROVIDER=ollama
```

#### **OpenAI**
```bash
OPENAI_API_KEY=your-openai-key
OPENAI_API_BASE=https://api.openai.com/v1
LITELLM_MODEL=gpt-3.5-turbo
```

#### **Anthropic**
```bash
ANTHROPIC_API_KEY=your-anthropic-key
LITELLM_MODEL=claude-3-sonnet-20240229
```

#### **Google**
```bash
GOOGLE_API_KEY=your-google-key
LITELLM_MODEL=gemini-1.5-flash
```

## 🛠️ **Usage Examples**

### **Example 1: Using CodeGates Configuration**

```python
from agent.codegates_agent import CodeGatesAgent
from agent.litellm_config import setup_litellm_environment

# Setup LiteLLM with CodeGates configuration
config = setup_litellm_environment()

# Create agent (automatically uses configured LLM)
agent = CodeGatesAgent()

# Use agent for validation
# The agent will automatically use the configured LLM
```

### **Example 2: Custom LiteLLM Configuration**

```python
import os
from agent.litellm_config import LiteLLMConfig

# Set custom configuration
os.environ["LITELLM_PROVIDER"] = "ollama"
os.environ["LITELLM_MODEL"] = "llama2"
os.environ["OLLAMA_BASE_URL"] = "http://localhost:11434"

# Create configuration
config = LiteLLMConfig()
config.configure_litellm_from_codegates()

# Use in agent
agent = CodeGatesAgent()
```

### **Example 3: Direct LiteLLM Usage**

```python
import litellm
from agent.litellm_config import get_litellm_completion_params

# Prepare messages
messages = [
    {"role": "user", "content": "Analyze this repository for compliance"}
]

# Get completion parameters
params = get_litellm_completion_params(messages)

# Generate response
response = await litellm.acompletion(**params)
print(response.choices[0].message.content)
```

## 📊 **Provider Mapping**

| CodeGates Provider | LiteLLM Provider | Configuration |
|-------------------|------------------|---------------|
| `LLMProvider.OPENAI` | `openai` | `OPENAI_API_KEY`, `OPENAI_API_BASE` |
| `LLMProvider.ANTHROPIC` | `anthropic` | `ANTHROPIC_API_KEY` |
| `LLMProvider.GOOGLE` | `google` | `GOOGLE_API_KEY` |
| `LLMProvider.OLLAMA` | `ollama` | `OLLAMA_BASE_URL` |
| `LLMProvider.ENTERPRISE` | `custom` | Custom base URL and model |

## 🔧 **Installation and Setup**

### **1. Install Dependencies**

```bash
# Install LiteLLM
pip install litellm

# Install Google ADK (if not already installed)
pip install google-adk

# Ensure CodeGates dependencies are available
pip install -r gates/requirements.txt
```

### **2. Configure Environment**

```bash
# Set up your preferred LLM provider
export CODEGATES_LLM_PROVIDER=ollama
export CODEGATES_LLM_MODEL=llama2
export CODEGATES_LLM_BASE_URL=http://localhost:11434

# Or use LiteLLM-specific variables
export LITELLM_PROVIDER=ollama
export LITELLM_MODEL=llama2
export OLLAMA_BASE_URL=http://localhost:11434
```

### **3. Test Configuration**

```python
from agent.litellm_config import test_litellm_configuration

# Test the configuration
test_litellm_configuration()
```

## 🎯 **Benefits for Local LLM Usage**

### **1. Local Development**
- **No API Costs**: Use local LLMs without external API calls
- **Privacy**: Keep data local and private
- **Customization**: Use custom models and fine-tuned versions
- **Offline Capability**: Work without internet connectivity

### **2. Enterprise Deployment**
- **Security**: Keep sensitive data within enterprise boundaries
- **Compliance**: Meet regulatory requirements for data handling
- **Performance**: Optimize for enterprise infrastructure
- **Customization**: Use enterprise-specific models

### **3. Cost Optimization**
- **Reduced API Costs**: Minimize external API usage
- **Predictable Pricing**: Fixed costs for local infrastructure
- **Scalability**: Scale based on internal resources
- **Control**: Full control over model deployment

## 🔄 **Integration with Existing Workflows**

### **1. CodeGates Integration**
- **Existing Configuration**: Uses current CodeGates LLM settings
- **Tool Compatibility**: All existing tools work with LiteLLM
- **Report Generation**: Maintains existing report formats
- **Validation Logic**: Preserves all validation capabilities

### **2. ADK Integration**
- **Agent Framework**: Works seamlessly with Google ADK
- **Tool Integration**: All ADK tools use LiteLLM
- **Session Management**: Maintains ADK session capabilities
- **Event Streaming**: Preserves real-time event streaming

### **3. External Integrations**
- **Splunk**: Evidence collection works with any LLM
- **AppDynamics**: Monitoring integration unchanged
- **Playwright**: Web portal evidence collection preserved
- **Database**: All database operations remain the same

## 🚀 **Deployment Options**

### **1. Local Development**
```bash
# Start Ollama locally
ollama serve

# Run CodeGates agent
python agent/codegates_agent.py
```

### **2. Docker Deployment**
```dockerfile
# Dockerfile for local LLM deployment
FROM python:3.9

# Install dependencies
RUN pip install litellm google-adk

# Install Ollama
RUN curl -fsSL https://ollama.ai/install.sh | sh

# Copy application
COPY . /app
WORKDIR /app

# Start services
CMD ["python", "agent/codegates_agent.py"]
```

### **3. Kubernetes Deployment**
```yaml
# Kubernetes deployment for enterprise
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

## 📋 **Testing and Validation**

### **1. Configuration Testing**
```python
# Test LiteLLM configuration
from agent.litellm_config import test_litellm_configuration

test_litellm_configuration()
```

### **2. Agent Testing**
```python
# Test agent creation
from agent.codegates_agent import CodeGatesAgent

agent = CodeGatesAgent()
print(f"Agent created: {agent.name}")
```

### **3. Tool Testing**
```python
# Test individual tools
from agent.codegates_agent import RepositoryAnalysisTool

tool = RepositoryAnalysisTool()
print(f"Tool available: {tool.name}")
```

## 🎯 **Next Steps**

### **Immediate Actions**
1. **Install LiteLLM**: `pip install litellm`
2. **Configure Local LLM**: Set up Ollama or other local LLM
3. **Test Integration**: Run configuration tests
4. **Validate Agent**: Test agent with local LLM

### **Short-term Goals**
1. **Deploy to Development**: Test in development environment
2. **Performance Testing**: Benchmark with local LLM
3. **Integration Testing**: Test with existing CodeGates workflows
4. **Documentation**: Create user guides and examples

### **Long-term Vision**
1. **Enterprise Deployment**: Deploy to enterprise environments
2. **Custom Models**: Integrate custom enterprise models
3. **Advanced Features**: Add advanced LLM capabilities
4. **Scalability**: Optimize for large-scale deployments

## 📋 **Conclusion**

The **LiteLLM integration** with the **CodeGates agentic application** provides:

- **Seamless Integration**: Uses existing CodeGates LLM configuration
- **Local LLM Support**: Enables local LLM usage (Ollama, etc.)
- **Provider Flexibility**: Supports multiple LLM providers
- **Enterprise Ready**: Suitable for enterprise deployments
- **Cost Optimization**: Reduces external API costs
- **Privacy Enhancement**: Keeps data local and private

This integration transforms the CodeGates agent into a **flexible, cost-effective, and privacy-focused** validation system that can work with any LLM provider while maintaining all existing functionality and capabilities.

The agent can now leverage **local LLMs** for development and testing, **enterprise LLMs** for production deployments, and **cloud LLMs** for specific use cases, providing maximum flexibility and control over the validation process. 