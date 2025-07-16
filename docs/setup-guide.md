# n8n-MCP Setup Guide

## 🔧 Current Configuration Status

✅ **GitHub Integration**: Connected and working  
✅ **n8n-MCP Server**: Connected and working  
⚠️ **n8n API Connection**: Needs troubleshooting  

## 🚀 Quick Setup Checklist

### 1. Verify n8n Server is Running

Make sure your n8n server is running on `localhost:5678`:

```bash
# Check if n8n is running
curl http://localhost:5678/healthz
```

Expected response: `{"status":"ok"}`

### 2. Enable n8n API Access

In your n8n instance:

1. Go to **Settings** → **API**
2. Make sure **API Access** is enabled
3. Verify your API key is correct: `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...`

### 3. Test API Connection

Test the API connection manually:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" http://localhost:5678/api/v1/workflows
```

### 4. Docker Network Troubleshooting

If the MCP server can't connect to your local n8n:

#### Option A: Use Host Network (Recommended)
Update your MCP configuration to use host networking:

```json
"args": [
  "run",
  "-i",
  "--rm",
  "--network=host",
  "-e", "N8N_API_URL=http://localhost:5678",
  "-e", "N8N_API_KEY=YOUR_KEY",
  "ghcr.io/czlonkowski/n8n-mcp:latest"
]
```

#### Option B: Use Docker Desktop Internal IP
Find your Docker Desktop internal IP:

```bash
# On Windows
ipconfig | findstr "Docker"

# Then use that IP instead of localhost
# Example: http://192.168.65.1:5678
```

## 🧪 Testing the Integration

Once connected, you can test with these commands:

```
# Check n8n health
n8n_health_check()

# List existing workflows
n8n_list_workflows()

# Create a simple workflow
n8n_create_workflow({
  "name": "Test Workflow",
  "nodes": [...],
  "connections": {...}
})
```

## 🔄 Workflow Automation Process

Once everything is connected:

1. **Create**: Ask AI to create a workflow
2. **Validate**: n8n-MCP validates the configuration
3. **Deploy**: Workflow is deployed to your n8n server
4. **Backup**: Workflow JSON is saved to this GitHub repository
5. **Version Control**: All changes are tracked with proper commit messages

## 📁 Repository Structure

```
n8n-workflows/
├── workflows/              # Deployed workflow JSON files
│   └── demo-hello-world.json
├── templates/              # Reusable workflow templates
├── docs/                   # Documentation
│   └── setup-guide.md     # This file
└── README.md              # Main documentation
```

## 🆘 Troubleshooting

### Common Issues

1. **"Unable to connect to n8n"**
   - Check if n8n is running: `curl http://localhost:5678/healthz`
   - Verify API is enabled in n8n settings
   - Check Docker network configuration

2. **"Invalid API key"**
   - Regenerate API key in n8n settings
   - Update the key in your MCP configuration

3. **"Permission denied"**
   - Ensure your API key has sufficient permissions
   - Check if your n8n user has workflow creation rights

### Getting Help

If you need assistance:

1. Check the [n8n documentation](https://docs.n8n.io/)
2. Review the [n8n-MCP server docs](https://github.com/czlonkowski/n8n-mcp)
3. Test the connection manually using curl commands above

## 🎯 Next Steps

1. Fix the n8n API connection using the troubleshooting steps above
2. Test workflow creation and deployment
3. Start building your automation workflows!

---

*Last updated: July 16, 2025*