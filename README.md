# New Relic MCP Server

The New Relic MCP Server allows AI agents and tools to securely access your New Relic observability data using the Model Context Protocol (MCP). Use it to query metrics, analyze performance, investigate alerts, and gain insights from your telemetry data.

## Features

- **Query observability data** - Execute NRQL queries and fetch metrics directly from New Relic

- **Alert management** - List open alerts, fetch alert policies, and analyze incident data  

- **Entity discovery** - Find and explore applications, services, hosts, and other monitored entities

- **Performance analysis** - Analyze golden metrics, logs, and system health data

- **AI-powered insights** - Generate intelligent reports and perform root cause analysis

## Installation & Setup

### Prerequisites

- New Relic account with appropriate permissions
- API key or OAuth2.0 credentials
- MCP-compatible client (VS Code with GitHub Copilot, Cursor, Claude Code, etc.)

### Step 1: Get New Relic Credentials

#### Option A: API Key Authentication (Recommended)
1. Log into your New Relic account
2. Go to **API Keys** in your account settings
3. Create a new **User API Key**
4. Copy the key (it should start with `NRAK-`)

#### Option B: OAuth2.0 Authentication  
No setup required. Authenticate with your New Relic account when prompted.

### Step 2: Set up your MCP client

#### VS Code

1. Use `⌘ Shift P` to search for `MCP: Add Server`
2. Select `HTTP`
3. Enter the server URL: `{mcp-server-production-url}`
4. When prompted for a server ID, enter: `new-relic-mcp-server`
5. Add your API key configuration to `.vscode/mcp.json`:

```json
{
    "inputs": [
        {
            "type": "promptString",
            "id": "nr-api-key",
            "description": "New Relic API Key",
            "password": true
        }
    ],
    "servers": {
        "new-relic-mcp-server": {
            "url": "{mcp-server-production-url}",
            "type": "http",
            "headers": {
                "api-key": "${input:nr-api-key}"
            }
        }
    }
}
```

For OAuth2.0:
```json
{
  "servers": {
    "new-relic-mcp-server": {
      "url": "{mcp-server-production-url}"
    }
  }
}
```

6. Open the chat toolbar using `⌥⌘B` or `⌃⌘I` and switch to Agent mode


> **Note**: You must have [GitHub Copilot](https://github.com/features/copilot) enabled to use MCP in VS Code.

#### Cursor

1. Open Cursor → Settings → Cursor Settings
2. Go to the MCP tab
3. Click "+ Add new global MCP server"
4. Enter the configuration:

```json
{
    "mcpServers": {
        "new-relic-mcp-server": {
            "url": "{mcp-server-production-url}",
            "headers": {
                "api-key": "YOUR_NEW_RELIC_API_KEY"
            }
        }
    }
}
```

#### Claude Code

```bash
claude mcp add --transport http new-relic-mcp-server {mcp-server-production-url}
```

#### Other editors

For editors that support Streamable HTTP, use this configuration:

```json
{
    "mcpServers": {
        "new-relic-mcp-server": {
            "url": "{mcp-server-production-url}",
            "headers": {
                "api-key": "YOUR_NEW_RELIC_API_KEY"
            }
        }
    }
}
```

## Available Tools

The New Relic MCP server provides tools organized by functional categories:

### Discovery Tools
- `list_available_new_relic_accounts` - List accessible New Relic accounts
- `lookup_entity` - Find entities by name or GUID
- `fetch_entity` - Get detailed entity information
- `get_entity_call_graph` - Explore service dependencies

### Alerting Tools  
- `list_all_open_alerts` - View currently active alerts
- `fetch_alert_policies` - Get alert policy configurations
- `create_alert_policy` - Create new alert policies
- `lookup_vulnerabilities` - Check security vulnerabilities

### Performance Analytics Tools
- `analyze_golden_metrics` - Analyze key performance indicators
- `list_recent_logs` - Fetch recent log entries
- `fetch_and_analyze_threads` - Analyze application thread dumps
- `analyze_kafka_metrics` - Monitor Kafka cluster performance

### Data Access Tools
- `execute_nrql_query` - Run custom NRQL queries
- `run_nerdgraph_query` - Execute GraphQL queries
- `natural_language_to_nrql_query` - Convert natural language to NRQL

### AI-Powered Analysis Tools
- `generate_alert_insights_report` - AI-powered incident analysis
- `analyze_deployment_impact` - Assess deployment effects
- `sre_causal_analysis` - Root cause analysis
- `analyze_entity_logs` - Intelligent log analysis

## Usage Examples

### Basic Queries

```
"Show me all open alerts for my production environment"
"What are the golden metrics for my main application?"
"List all hosts in account 12345"
```

### Performance Analysis

```
"Analyze the performance impact of the deployment from 2 hours ago"
"Show me error rates and response times for my API service"
"Generate an insights report for the database alerts from last night"
```

### Custom Data Exploration

```
"Query transaction traces where duration > 5 seconds in the last hour"
"Show me all synthetic monitor failures from today"
"Convert this to NRQL: show me average CPU usage by host"
```

### AI-Powered Insights

```
"Perform root cause analysis on the high memory usage alerts"
"Analyze recent log patterns for errors in my microservices"
"Generate a summary of system health across all my applications"
```

## Tool Filtering

You can filter available tools by adding the `include-tags` header to focus on specific use cases:

### By Use Case
```json
{
    "headers": {
        "api-key": "YOUR_API_KEY",
        "include-tags": "alerting,incident-response"
    }
}
```

### By Safety
```json
{
    "headers": {
        "api-key": "YOUR_API_KEY", 
        "include-tags": "read-only"
    }
}
```

### Available Tags
- `discovery` - Finding and exploring entities
- `alerting` - Alert and policy management  
- `incident-response` - Troubleshooting and investigation
- `performance-analytics` - Metrics and system health
- `data-access` - Custom queries and exploration
- `advanced-analysis` - AI-powered analysis
- `read-only` - Tools that only read data

## Best Practices

### Structuring Effective Queries

- **Be specific about time ranges**: "in the last hour", "since yesterday", "from 2-4 PM today"
- **Include context**: Mention environment, application names, or specific services
- **Use entity names**: Reference applications, hosts, or services by their New Relic names

### Example Effective Prompts

- "Show me error rates for the checkout-service in production over the last 2 hours"
- "Analyze golden metrics for my e-commerce application stack since the 3 PM deployment"
- "Generate an incident report for the database alerts that started this morning"

### Working with Large Datasets

- Start with summary queries before diving into detailed analysis
- Use time-based filtering to focus on relevant periods  
- Combine multiple tools for comprehensive investigation

## Authentication & Security

- **API Keys**: Store securely and rotate regularly
- **Permissions**: The server respects your New Relic account permissions
- **Rate Limits**: Queries are subject to New Relic API rate limits
- **Read-Only**: Most tools are read-only and won't modify your New Relic configuration

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `401 Unauthorized` | Check that your API key starts with `NRAK-` and has proper permissions |
| `No accounts found` | Verify your API key has access to New Relic accounts |
| `Tool not found` | Ensure your MCP client supports the latest protocol version |
| `Rate limit exceeded` | Wait a few minutes before retrying queries |

## Support & Contributing

- **Documentation**: [Model Context Protocol Specification](https://modelcontextprotocol.io/)
- **Issues**: Report bugs and feature requests via GitHub Issues
- **Community**: Join discussions in GitHub Discussions


--- 
---

**New Relic MCP Server** - Bringing observability data directly to your AI workflow.