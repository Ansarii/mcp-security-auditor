# MCP Security Auditor Server

[![Model Context Protocol](https://img.shields.io/badge/MCP-Server-blue.svg)](https://modelcontextprotocol.io)
[![Glama Quality Score](https://glama.ai/mcp/servers/Ansarii/mcp-security-auditor/badges/score.svg)](https://glama.ai/mcp/servers/Ansarii/mcp-security-auditor)
[![Python 3.12](https://img.shields.io/badge/Python-3.12-brightgreen.svg)](https://python.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Transport: stdio](https://img.shields.io/badge/Transport-stdio-orange.svg)](https://modelcontextprotocol.io/docs/concepts/transports)

A production-ready **Model Context Protocol (MCP) Server** providing AI coding assistants (Claude Desktop, Cursor, Windsurf, Cline) with native security analysis tools. It enables AI agents to statically audit other MCP servers, tool repositories, and local codebases for critical security vulnerabilities before onboarding or executing them.

---

## 🛠️ MCP Tools Exposed

This server implements the official Model Context Protocol specification (`tools/list` and `tools/call`):

### 1. `audit_mcp_repository`
Performs an automated, zero-execution static Abstract Syntax Tree (AST) security audit on a Git repository containing an MCP server implementation.

* **Parameters:**
  * `repository_url` (string, required): Public Git URL of the MCP server repository to audit (e.g. `https://github.com/example/mcp-server`).
  * `sub_directory` (string, optional): Specific subdirectory within the repository to inspect (useful for monorepos).
* **Vulnerability Checks Conducted:**
  * **CWE-78 (Command Injection):** Detects unsanitized `subprocess.run`, `child_process.exec`, shell invocation in tool execution blocks.
  * **CWE-22 (Path Traversal):** Flags unrestricted file read/write operations lacking path canonicalization (`os.path.realpath`, boundary checks).
  * **CWE-798 (Hardcoded Secrets):** Identifies leaked API keys, tokens, and corporate credentials in tool schemas or server definitions.
  * **CWE-306 (Missing Authentication):** Flags public HTTP/SSE transports exposing destructive capabilities without bearer auth.
  * **CWE-1336 (Tool & Prompt Injection):** Flags tool descriptions susceptible to prompt hijacking.
* **Returns:**
  * `trust_score` (0–100): Weighted security index.
  * `grade` ("A+", "A", "B", "C", "F"): Security letter grade.
  * `findings` (array): Line-by-line vulnerability records with CWE identifiers and remediation instructions.
  * `markdown_report` (string): Pre-formatted human-readable audit report.

### 2. `audit_local_directory`
Audits a local filesystem directory containing MCP server source code before deployment.

* **Parameters:**
  * `directory_path` (string, required): Absolute or relative filesystem path to the target directory.
* **Returns:** Structured JSON audit record and remediation checklist.

---

## 🚀 Client Configuration & Quickstart

### 1. Claude Desktop Setup
Add the server to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "mcp-security-auditor": {
      "command": "python3",
      "args": ["/path/to/mcp-security-auditor/server.py"]
    }
  }
}
```

### 2. Cursor IDE / Windsurf Setup
In Cursor, go to **Settings → Features → MCP → Add New MCP Server**:
* **Name:** `mcp-security-auditor`
* **Type:** `command`
* **Command:** `python3 /absolute/path/to/server.py`

### 3. Docker Container Execution
Run isolated via Docker using the pre-configured `Dockerfile`:

```json
{
  "mcpServers": {
    "mcp-security-auditor": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "ghcr.io/ansarii/mcp-security-auditor"
      ]
    }
  }
}
```

---

## 💻 Example Agent Usage

Once connected, your AI assistant can execute security pre-flight checks before installing tools:

```text
User: "Audit the MCP server at https://github.com/modelcontextprotocol/servers before I install it."

Claude (Tool Call):
audit_mcp_repository({
  "repository_url": "https://github.com/modelcontextprotocol/servers",
  "sub_directory": "src/fetch"
})

Result:
{
  "status": "success",
  "target_name": "servers:src/fetch",
  "trust_score": 95,
  "grade": "A+",
  "findings_count": 0,
  "recommendation": "SAFE_TO_CONNECT"
}
```

---

## 🔒 Security Guarantee: Zero Execution

Unlike runtime scanners that execute arbitrary `stdio` binaries (creating direct Remote Code Execution risks on the host), this MCP server uses **pure static Abstract Syntax Tree (AST) analysis**. It inspects Python, TypeScript, and JSON-RPC implementations without ever executing untrusted code.

---

## 📜 License
MIT License — Copyright (c) 2026 Neon Innovation Lab.
