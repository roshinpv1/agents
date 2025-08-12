# 🚀 HardGate Agent Usage Guide

## ✅ Status: FULLY OPERATIONAL

The HardGate Agent is now working correctly with Google ADK and LiteLLM integration. This guide explains how to properly import and use the agent.

## 🔧 Import Solutions

### Option 1: Direct Import (Recommended)
```python
import sys
from pathlib import Path

# Add the hardgate_agent directory to the path
sys.path.insert(0, str(Path(__file__).parent / "agent" / "hardgate_agent"))

# Import the agent
from agent import HardGateAgent

# Create agent instance
agent = HardGateAgent()
```

### Option 2: Using the Dedicated Import Script
```python
# Use the provided import script
from agent.hardgate_agent.import_hardgate_agent import create_hardgate_agent

# Create agent instance
agent = create_hardgate_agent()
```

### Option 3: Working Directory Method
```bash
# Navigate to the hardgate_agent directory
cd agent/hardgate_agent

# Run scripts from this directory
python3 test_agent_loading.py
python3 example_usage.py
python3 import_hardgate_agent.py
```

## 🎯 Quick Start

### 1. Prerequisites
```bash
# Install required dependencies
pip3 install google-adk
pip3 install litellm

# Start LiteLLM server (if not already running)
# The agent is configured to use: http://localhost:1234/v1
```

### 2. Basic Usage
```python
import asyncio
import sys
from pathlib import Path

# Add hardgate_agent to path
sys.path.insert(0, str(Path(__file__).parent / "agent" / "hardgate_agent"))

from agent import HardGateAgent

async def main():
    # Initialize agent
    agent = HardGateAgent()
    
    if not agent.agent:
        print("❌ Agent initialization failed")
        return
    
    print("✅ HardGate Agent ready!")
    print(f"📊 {len(agent.agent.tools)} tools configured")
    
    # Use the agent for analysis
    result = await agent.run_complete_analysis(
        repository_url="https://github.com/your-repo/example",
        branch="main",
        app_id="your-app-123"
    )
    
    if result.get("success"):
        print("✅ Analysis completed!")
        print(f"Security Score: {result.get('summary', {}).get('security_score', 0)}%")
    else:
        print(f"❌ Analysis failed: {result.get('error')}")

# Run the example
asyncio.run(main())
```

## 🔧 Available Tools

The HardGate Agent includes 8 specialized tools:

1. **RepositoryAnalysisTool** (`analyze_repository`)
   - Analyzes repository structure and identifies technologies
   - Determines applicable hard gates

2. **GateValidationTool** (`validate_gates`)
   - Validates hard gates against the codebase
   - Uses pattern matching and analysis

3. **EvidenceCollectionTool** (`collect_evidence`)
   - Collects evidence from external sources
   - Supports Splunk, AppDynamics, web portals

4. **ReportGenerationTool** (`generate_report`)
   - Generates comprehensive security reports
   - Multiple formats: JSON, HTML, Markdown

5. **CodeScanningTool** (`scan_code`)
   - Performs security vulnerability scanning
   - Checks for secrets, dependencies, configuration issues

6. **SecurityAnalysisTool** (`analyze_security`)
   - Performs risk assessment and threat modeling
   - Generates security recommendations

7. **ComplianceCheckTool** (`check_compliance`)
   - Validates against security standards
   - SOC2, ISO27001, NIST, Enterprise policies

8. **LLMAnalysisTool** (`analyze_with_llm`)
   - Uses LiteLLM for intelligent analysis
   - Generates AI-powered recommendations

## 🎯 LiteLLM Configuration

The agent is configured with:
- **Model**: `gpt-3.5-turbo`
- **Base URL**: `http://localhost:1234/v1`
- **API Key**: `sdsd`
- **Provider**: `openai`

## 📋 API Methods

### Main Analysis Methods
```python
# Complete repository analysis
result = await agent.run_complete_analysis(
    repository_url="https://github.com/example/repo",
    branch="main",
    github_token="your-token",  # Optional
    app_id="your-app-id"        # Optional
)

# Individual tool methods
result = await agent.analyze_repository(repository_url, branch, github_token)
result = await agent.validate_gates(repository_path, gates_list)
result = await agent.perform_security_scan(repository_path, scan_type)
result = await agent.collect_evidence(app_id, sources, time_range)
result = await agent.perform_security_analysis(analysis_data, analysis_type)
result = await agent.check_compliance(analysis_data, frameworks)
result = await agent.generate_report(analysis_data, report_type, output_format)
```

## 🚨 Troubleshooting

### Import Errors
**Error**: `No module named 'prompt'`
**Solution**: Use the direct import method or run from the `hardgate_agent` directory

**Error**: `BaseTool.__init__() missing 2 required keyword-only arguments`
**Solution**: This is fixed in the current implementation. All tools have proper `__init__` methods.

### LiteLLM Connection Issues
**Error**: Connection refused to `http://localhost:1234/v1`
**Solution**: Start your LiteLLM server on the specified port

### Google ADK Issues
**Error**: `google.adk` module not found
**Solution**: Install the Google ADK library: `pip3 install google-adk`

## 📁 File Structure
```
agent/hardgate_agent/
├── agent.py                 # Main agent orchestrator
├── config.py               # Configuration management
├── prompt.py               # Agent instructions and prompts
├── requirements.txt        # Dependencies
├── test_agent_loading.py   # Test suite
├── example_usage.py        # Usage examples
├── import_hardgate_agent.py # Dedicated import script
├── USAGE_GUIDE.md          # This guide
├── HARDGATE_AGENT_SUCCESS.md # Success documentation
└── tools/                  # 8 specialized tools
    ├── repository_analysis.py
    ├── gate_validation.py
    ├── evidence_collection.py
    ├── report_generation.py
    ├── code_scanning.py
    ├── security_analysis.py
    ├── compliance_check.py
    └── llm_analysis.py
```

## 🎉 Success Verification

To verify everything is working:

```bash
cd agent/hardgate_agent
python3 test_agent_loading.py
```

Expected output:
```
🎉 All tests passed! HardGate Agent is ready for use.
✅ LiteLLM configured with:
   - Model: gpt-3.5-turbo
   - Base URL: http://localhost:1234/v1
   - Provider: openai
```

## 🚀 Next Steps

1. **Start your LiteLLM server** on `http://localhost:1234/v1`
2. **Use the import methods** described above
3. **Run security analysis** on your repositories
4. **Extend functionality** by adding custom tools
5. **Integrate with CI/CD** for automated security analysis

---

**Status**: 🟢 **OPERATIONAL** - Ready for enterprise use! 