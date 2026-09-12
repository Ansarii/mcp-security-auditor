# MCP Security Scanner & Vulnerability Auditor | Zero-Trust AI Agent Shield

[![Run on Apify](https://apify.com/actor-badge?actor=neon_innovation_lab/mcp-security-auditor)](https://apify.com/neon_innovation_lab/mcp-security-auditor)

⚡ **Run directly on Apify Cloud**: [MCP Security Auditor](https://apify.com/neon_innovation_lab/mcp-security-auditor)  
👉 **Companion Open-Source Repo**: [github.com/Ansarii/mcp-security-auditor](https://github.com/Ansarii/mcp-security-auditor)

[![Model Context Protocol](https://img.shields.io/badge/MCP-Security_Auditor-red.svg)](https://modelcontextprotocol.io)
[![CWE Standards](https://img.shields.io/badge/CWE-Top_25_Covered-blue.svg)](https://cwe.mitre.org/)
[![Static AST Analysis](https://img.shields.io/badge/Analysis-Pure_AST_Zero_Execution-brightgreen.svg)](https://neoninnovationlab.com/tools/mcp-security-scanner)

Giving an autonomous AI agent access to a third-party **Model Context Protocol (MCP)** server provides enormous productivity—and severe security risk. Vulnerable MCP tools expose agent runtimes to remote code execution (RCE), arbitrary file exfiltration, and prompt injection attacks.

This Actor is a **zero-execution static AST security scanner** for MCP servers. Before connecting any public or private server to **Claude Desktop**, **Cursor IDE**, or an enterprise agent fleet, scan it here to obtain a verified **Trust Score (0–100)**, Letter Grade, and line-by-line CWE remediation plan.

---

## 🛡️ Why Use This Actor?

Following 30+ CVE disclosures against community MCP servers in 2025 and 2026, enterprise security standards strictly prohibit connecting unverified servers to developer environments or corporate databases.

Unlike dangerous runtime scanners that execute arbitrary `stdio` binaries (creating direct RCE risks on the scanning host), this Actor uses **pure static Abstract Syntax Tree (AST) analysis**. It inspects Python, TypeScript, and JSON-RPC implementations without ever executing untrusted code.

---

## 🌍 Global Enterprise & Regional Compliance (GEO Targeting)

### 🇺🇸 North America (Silicon Valley, New York, Seattle, Austin)
- **SOC 2 Type II Compliance**: Satisfies CC6.6 and CC6.8 controls by maintaining an immutable static audit log before onboarding third-party agent tools.
- **Enterprise Secret Shield**: Discovers hardcoded AWS keys (`AKIA...`), OpenAI API keys, and corporate service credentials buried in tool repos.

### 🇪🇺 Europe & 🇬🇧 United Kingdom (London, Berlin, Paris, Amsterdam)
- **EU AI Act Article 15 (Cybersecurity & Resilience)**: Generates the mandatory risk assessment documentation and static vulnerability reports required for enterprise AI software.
- **GDPR Article 32 (Security of Processing)**: Ensures MCP servers connecting to European customer data do not expose unauthorized file system read/write primitives.

### 🌏 Asia-Pacific & 🇮🇳 India (Singapore, Tokyo, Sydney, Bengaluru, Hyderabad)
- **Offshore Development Quality Gate**: Audits tools developed by third-party contractors before deployment to production agent clusters.
- **Cross-Border Security Verification**: Guarantees external MCP tools do not transmit internal environment variables or telemetry to untrusted third-party hosts.

---

## 🔍 Vulnerability Checks Covered

| Rule ID | Severity | CWE | Vulnerability Category | Description & Impact |
| :--- | :--- | :--- | :--- | :--- |
| **MCP-SEC-001** | CRITICAL | **CWE-78** | **Command Injection** | Detects `subprocess.run(shell=True)`, `os.system()`, and unsanitized string formatting passed to shell interpreters. |
| **MCP-SEC-002** | HIGH | **CWE-22** | **Path Traversal / Arbitrary File Access** | Detects file tools lacking strict path canonicalization (`Path.is_relative_to()`), ZipSlip/TarSlip vulnerabilities, and unbounded directory crawls. |
| **MCP-SEC-003** | CRITICAL | **CWE-798** | **Hardcoded Secrets & API Keys** | Identifies leaked OpenAI, Anthropic, AWS, GitHub, Stripe, and Slack tokens, and flags tools exporting raw `os.environ`. |
| **MCP-SEC-004** | HIGH | **CWE-306** | **Unauthenticated Remote Transport** | Flags SSE or HTTP servers binding to `0.0.0.0` without bearer tokens or mutual TLS authentication. |
| **MCP-SEC-005** | HIGH | **CWE-1384** | **Tool Poisoning & Prompt Injection** | Flags adversarial instructions, hidden unicode homoglyphs, and jailbreaks embedded inside tool docstrings or schemas. |

---

## 📥 Input Configuration

```json
{
  "repositoryUrls": [
    "https://github.com/modelcontextprotocol/servers"
  ],
  "subDirectories": [
    "src/everything",
    "src/fetch"
  ],
  "minimumPassScore": 80
}
```

- **`repositoryUrls`** *(Required, Array)*: List of public Git URLs for the MCP server repositories to audit.
- **`subDirectories`** *(Optional, Array)*: Specific subdirectories to audit individually (ideal for monorepos).
- **`minimumPassScore`** *(Optional, Integer, Default: `80`)*: The minimum score (0–100) required to achieve a passing audit grade.

---

## 📤 Output & Audit Artifacts

### 1. Structured Dataset Records
Each audited server produces a structured JSON record containing:
- `trust_score`: Score from `0` to `100`.
- `grade`: Security letter grade (`A+`, `A`, `B`, `C`, `F`).
- `passed`: Boolean indicating whether the score meets `minimumPassScore`.
- `findings`: Array of detailed findings with file paths, line numbers, CWE IDs, code snippets, and remediation steps.

### 2. Downloadable Markdown Report
The full human-readable audit report is saved to the run's default Key-Value store under **`OUTPUT_REPORT.md`**.

---

## 🌐 Free Online Interactive Scanner
Test single repositories in real time using our free interactive tool:
**[https://neoninnovationlab.com/tools/mcp-security-scanner](https://neoninnovationlab.com/tools/mcp-security-scanner)**
