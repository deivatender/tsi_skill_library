# MCP Database Connectors for Claude Desktop

Connecting Claude to Microsoft SQL Server and MySQL

**Tender Software India (TSI)** — Internal Developer Documentation · Confidential

| Prepared By | Version | Last Updated | Audience |
|---|---|---|---|
| Deiva R     | 1.0 | 05 June 2026 | Internal Developers |

## 1. Overview

This guide explains how to configure Model Context Protocol (MCP) database connectors in Claude Desktop so Claude can securely query Microsoft SQL Server and MySQL databases.

Both connectors run locally through the `uvx` tool launcher and are registered in a single Claude Desktop configuration file. Once configured, you can use natural-language prompts such as:

- "List all tables in the SALES_REPORTS database."
- "Show me the top 10 orders from last month."
- "Write and run a query to calculate monthly revenue by product category."

### What is MCP?

The Model Context Protocol is an open standard that lets AI models such as Claude connect to external data sources and tools.

An MCP server (for example `mssql_mcp_server` or `mysql_mcp_server`) acts as a controlled bridge between Claude and your database — Claude never connects to the database directly.

### Scope

This setup is intended for local developer machines. Review Section 10 (Security Considerations) before pointing it at any shared, staging, or production database.

## 2. Prerequisites

Make sure the following are installed and available on the developer machine before you begin.

| Requirement | Version / Notes | How to Verify |
|---|---|---|
| Claude Desktop | Latest version | claude.ai/download |
| uv / uvx | Latest (installs the MCP servers on demand) | `uv --version` |
| Network access | Reachability to the DB host and port | `Test-NetConnection` |
| Database credentials | A least-privilege login (see Section 10) | Provided by your team lead |

## 3. Install uv / uvx

Both MCP servers are launched with `uvx`, which is part of the `uv` toolchain. Open PowerShell as Administrator and run:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Close and reopen the terminal, then confirm the install with `uv --version`.

## 4. Locate the Configuration File

Claude Desktop reads MCP server definitions from a single JSON file:

| Operating System | Configuration File Path |
|---|---|
| Windows | `%APPDATA%\Claude\claude_desktop_config.json` |

> **Tip**
> If the file does not exist yet, create it. The top-level object must contain a single `mcpServers` key. You can register both the SQL Server and MySQL connectors inside it — see the combined example in Section 7.

## 5. Microsoft SQL Server Connector

### 5.1 Add the MCP Server Entry

Open the configuration file in a text editor and add (or merge) the block below. Replace the example values with the connection details for your environment.

```json
{
  "mcpServers": {
    "mssql": {
      "command": "uvx",
      "args": [
        "--from",
        "microsoft-sql-server-mcp",
        "mssql_mcp_server.exe"
      ],
      "env": {
        "MSSQL_SERVER": "your-sql-host",
        "MSSQL_DATABASE": "SALES_REPORTS",
        "MSSQL_USER": "db_app_user",
        "MSSQL_PASSWORD": "<YOUR_PASSWORD>",
        "MSSQL_ENCRYPT": "false",
        "MSSQL_TRUST_SERVER_CERTIFICATE": "true"
      }
    }
  }
}
```

### 5.2 Configuration Reference

Each value below is set under the `env` object shown above. Variable names must match exactly.

| Variable | Example Value | Description |
|---|---|---|
| `MSSQL_SERVER` | `your-sql-host` | Server hostname or IP. Use `HOST\INSTANCE` for named instances. |
| `MSSQL_DATABASE` | `SALES_REPORTS` | Name of the database to connect to. |
| `MSSQL_USER` | `db_app_user` | SQL login username. Omit for Windows Authentication. |
| `MSSQL_PASSWORD` | `<YOUR_PASSWORD>` | SQL login password. Omit for Windows Authentication. |
| `MSSQL_ENCRYPT` | `false` | Set to `true` to require an encrypted connection. |
| `MSSQL_TRUST_SERVER_CERTIFICATE` | `true` | `true` for self-signed certificates. Use `false` in production. |

### 5.3 Verify the Connection

Fully quit and restart Claude Desktop (File → Quit, or Cmd+Q on macOS). Claude reads the MCP configuration only at startup.

1. Open Claude Desktop.
2. Go to Settings → Connectors → Customize.
3. Confirm that `mssql` is listed as a connected tool.
4. If it shows an error, open Settings → Developer → MCP Logs to review the details.

## 6. MySQL Connector

### 6.1 Add the MCP Server Entry

Add (or merge) the following block into the same configuration file. Replace the example values with your environment's connection details.

```json
{
  "mcpServers": {
    "mysql": {
      "command": "uvx",
      "args": [
        "--from",
        "mysql-mcp-server",
        "mysql_mcp_server"
      ],
      "env": {
        "MYSQL_HOST": "your-mysql-host",
        "MYSQL_PORT": "3306",
        "MYSQL_USER": "db_app_user",
        "MYSQL_PASSWORD": "<YOUR_PASSWORD>",
        "MYSQL_DATABASE": "development_database"
      }
    }
  }
}
```

### 6.2 Configuration Reference

| Variable | Example Value | Description |
|---|---|---|
| `MYSQL_HOST` | `127.0.0.1` | MySQL server hostname or IP address. |
| `MYSQL_PORT` | `3306` | TCP port MySQL is listening on (default 3306). |
| `MYSQL_USER` | `db_app_user` | MySQL login username. |
| `MYSQL_PASSWORD` | `<YOUR_PASSWORD>` | MySQL login password. |
| `MYSQL_DATABASE` | `development_database` | Name of the database to connect to. |

### 6.3 Verify the Connection

Restart Claude Desktop, then confirm the connector loaded:

1. Open Claude Desktop and go to Settings → Connectors → Customize.
2. Confirm that `mysql` is listed as a connected tool.
3. On error, review Settings → Developer → MCP Logs.

## 7. Registering Both Connectors Together

The `mcpServers` object can hold multiple servers. To enable SQL Server and MySQL at the same time, place both entries side by side as sibling keys:

```json
{
  "mcpServers": {
    "mssql": {
      "command": "uvx",
      "args": ["--from", "microsoft-sql-server-mcp", "mssql_mcp_server.exe"],
      "env": {
        "MSSQL_SERVER": "your-sql-host\\SQLEXPRESS",
        "MSSQL_DATABASE": "SALES_REPORTS",
        "MSSQL_USER": "db_app_user",
        "MSSQL_PASSWORD": "<YOUR_PASSWORD>",
        "MSSQL_ENCRYPT": "false",
        "MSSQL_TRUST_SERVER_CERTIFICATE": "true"
      }
    },
    "mysql": {
      "command": "uvx",
      "args": ["--from", "mysql-mcp-server", "mysql_mcp_server"],
      "env": {
        "MYSQL_HOST": "your-mysql-host",
        "MYSQL_PORT": "3306",
        "MYSQL_USER": "db_app_user",
        "MYSQL_PASSWORD": "<YOUR_PASSWORD>",
        "MYSQL_DATABASE": "development_database"
      }
    }
  }
}
```

> **Valid JSON only**
> Run the file through a JSON validator before saving. A trailing comma or a missing brace stops *all* MCP servers from loading — not just the one you edited.

## 8. Using the Connectors with Claude

### 8.1 Available Tools

Once connected, Claude can call the following tools exposed by each MCP server:

| Tool Name | Description |
|---|---|
| `execute_query` | Run a SELECT, INSERT, UPDATE, or DELETE statement against the configured database and return the results. |
| `list_tables` | List all user tables in the connected database. |
| `describe_table` | Return the schema (columns, types, and constraints) of a specific table. |
| `get_database_info` | Return metadata about the connected database instance. |

### 8.2 Example Prompts

Use prompts like these to interact with either database through Claude:

- "Show me all tables in the database."
- "Describe the schema of the Orders table."
- "Write and run a query to get total sales by month for 2024."
- "Find all customers who have not placed an order in the last 90 days."
- "What are the top 5 products by revenue this year?"

## 9. Troubleshooting

| Symptom | Resolution |
|---|---|
| Connector not listed in Claude tools | Fully restart Claude Desktop, validate the JSON for syntax errors, and confirm the file is at the correct path. |
| Login failed for user (MSSQL) | Recheck `MSSQL_USER` and `MSSQL_PASSWORD`. Ensure SQL Server Authentication is enabled in the server properties. |
| Access denied for user (MySQL) | Recheck `MYSQL_USER` and `MYSQL_PASSWORD`, and confirm the user is allowed to connect from this host. |
| Connection timeout / cannot connect (MSSQL) | Confirm SQL Server is running and TCP/IP is enabled in SQL Server Configuration Manager. Check firewall rules. |
| Connection timeout / cannot connect (MySQL) | Confirm MySQL is running and reachable on port 3306. Check firewall rules and the bind-address setting. |

## 10. Security Considerations

Before using these connectors against any shared, staging, or production database, apply the following:

- Use a dedicated, least-privilege login (for example, a read-only role such as `db_datareader` in SQL Server) rather than a broad administrative account.
- Never use the `sa` (SQL Server) or `root` (MySQL) account for MCP connections.
- Keep tool calls under manual review. Leave the **Needs Approval** option enabled and never turn on auto-accept.

## 11. References

- mssql_mcp_server repository: https://github.com/RichardHan/mssql_mcp_server
- mysql_mcp_server repository: https://github.com/designcomputer/mysql_mcp_server
- Model Context Protocol specification: https://modelcontextprotocol.io
- Claude Desktop download: https://claude.ai/download
