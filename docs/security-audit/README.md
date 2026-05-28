# security-audit

A Claude Code skill that comprehensively investigates the security of a repository, checking for unintended external communications, data retention, third-party library risks, and common vulnerabilities.

## Usage

```
/security-audit
```

## What It Investigates

### 1. External Communication
- Usage of network APIs (fetch, XMLHttpRequest, WebSocket)
- Data transmission to external services
- Presence of analytics or tracking code
- HTTP/HTTPS communication patterns

### 2. Data Persistence and Storage
- Usage of localStorage, IndexedDB, and Cookies
- What data is being stored
- Handling of personal information and sensitive data
- Data retention policies

### 3. Third-Party Libraries
- Dependency analysis of package.json
- Detection of suspicious external libraries
- Library trustworthiness evaluation
- Known vulnerability checks

### 4. PWA / Service Worker
- Service Worker behavior and content
- Cache strategy
- Background processes
- Offline behavior

### 5. Other Security Risks
- `eval` and dangerous dynamic code execution
- XSS vulnerabilities
- SQL injection
- Command injection
- Path traversal
- Data exfiltration risk
- Child process execution safety
- Environment variable access safety

## Execution Steps

| Step | Action |
|------|--------|
| 1 | **Codebase Exploration** — use the Agent tool to search for external communication code, data storage APIs, and security risk patterns |
| 2 | **Pattern Search** — use Grep to scan for known dangerous patterns (see below) |
| 3 | **Detailed File Analysis** — read flagged files with the Read tool to assess context and actual risk |
| 4 | **Report Generation** — produce a structured report with findings and recommendations |

### Key Patterns Searched

**External Communications:** `fetch(`, `XMLHttpRequest`, `WebSocket`, `axios`, `http.get`, `https.request`

**Data Storage:** `localStorage`, `sessionStorage`, `indexedDB`, `openDatabase`, `cookie`, `Dexie`

**Dangerous Code:** `eval(`, `Function(`, `exec(`, `spawn(`, `child_process`, `innerHTML`, `dangerouslySetInnerHTML`

## Report Format

```markdown
## Security Audit Report

### ✅ Safe Points
[Items confirmed safe]

### ⚠️ Points Requiring Attention
[Potential risks and recommended countermeasures]

### 📊 Overall Assessment
[Risk level and overall evaluation]

### 💡 Improvement Recommendations
[Specific improvement proposals]
```

Each finding includes:

| Field | Content |
|-------|---------|
| **Issue Type** | External Communication / Data Storage / Vulnerability / etc. |
| **Risk Level** | Very Low / Low / Medium / High / Critical |
| **Location** | `file/path:line_number` |
| **Details** | Specific issue content |
| **Impact** | Impact on security |
| **Recommended Action** | How to improve |

## Risk Levels

| Level | Description | Example |
|-------|-------------|---------|
| **Very Low** | Almost no risk | Local processing only |
| **Low** | Minimal risk | Only trusted libraries used |
| **Medium** | Attention required | Child process execution, environment variable access |
| **High** | Significant risk | Unvalidated external input, dangerous code execution |
| **Critical** | Immediate action required | Credential exposure, SQL injection |

## Best Practices

1. **Incremental Investigation** — search broadly first, then perform detailed analysis
2. **Context-Focused** — understand the intent of code before making judgments
3. **Eliminate False Positives** — distinguish between safe and dangerous usage
4. **Specific Citations** — include file names and line numbers
5. **Provide Recommendations** — present solutions, not just problems

## References

For a detailed checklist and full pattern list, see [`reference.md`](../security-audit/reference.md).

## Notes

- This skill performs **static analysis** only. Runtime behavior must be verified separately.
- Internal code of third-party libraries is not fully analyzed.
- False positives are possible; final judgments should be made by a human.

## Allowed Tools

| Tool | Scope |
|------|-------|
| `Read` | Any file |
| `Grep` | Content search |
| `Glob` | File pattern matching |
| `Bash` | `find`, `cat` |
| `WebFetch` | External URLs |
| `Agent` | Subagent delegation |
