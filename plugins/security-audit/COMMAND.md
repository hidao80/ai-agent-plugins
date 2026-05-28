---
name: security-audit
allowed-tools: Bash(find:*), Bash(cat:*), Read, Grep, Glob, WebFetch, Agent
description: "Checks the security of a repository. Use when investigating external communications, data storage, third-party libraries, or security risks. Use cases: security audits, vulnerability checks, privacy verification, or investigating external communications."
---

# Security Audit Skill

## Overview

This skill comprehensively investigates the security of a repository, checking for any unintended external communications or data retention that the user may not be aware of.

## Investigation Items

### 1. External Communication Check
- Usage of network APIs (fetch, XMLHttpRequest, WebSocket)
- Data transmission to external services
- Presence of analytics or tracking code
- Investigation of HTTP/HTTPS communications

### 2. Data Persistence and Storage
- Usage of localStorage, IndexedDB, and Cookies
- What data is being stored
- Handling of personal information and sensitive data
- Confirmation of data retention policies

### 3. Third-Party Libraries
- Dependency analysis of package.json
- Detection of suspicious external libraries
- Library trustworthiness evaluation
- Known vulnerability checks

### 4. PWA/Service Worker
- Service Worker behavior and content
- Cache strategy verification
- Background process investigation
- Offline behavior analysis

### 5. Other Security Risks
- eval and dangerous dynamic code execution
- XSS vulnerability possibilities
- SQL injection
- Command injection
- Path traversal
- Risk of data exfiltration
- Security of child process execution
- Safety of environment variable access

## Execution Steps

### Step 1: Codebase Exploration

Use the Task tool with the Explore agent to investigate:

```
- Search for external communication-related code
- Usage of data storage-related APIs
- Dependency analysis of package.json
- Detection of security risk patterns
```

### Step 2: Pattern Search

Use the Grep tool to search for the following patterns:

**External Communications:**
- `fetch(`
- `XMLHttpRequest`
- `WebSocket`
- `axios`
- `http.get`
- `https.request`

**Data Storage:**
- `localStorage`
- `sessionStorage`
- `indexedDB`
- `openDatabase`
- `cookie`
- `Dexie`

**Dangerous Code:**
- `eval(`
- `Function(`
- `exec(`
- `spawn(`
- `child_process`
- `innerHTML`
- `dangerouslySetInnerHTML`

### Step 3: Detailed File Analysis

Read detected files using the Read tool to confirm context:

- Actual purpose of usage
- Handling of user input
- Implementation status of security measures
- Potential risk assessment

### Step 4: Report Generation

Create a report in the following format:

```markdown
## Security Audit Report

### ✅ Safe Points
[List of items confirmed safe]

### ⚠️ Points Requiring Attention
[Potential risks and recommended countermeasures]

### 📊 Overall Assessment
[Risk level and overall evaluation]

### 💡 Improvement Recommendations
[Specific improvement proposals]
```

## Risk Level Definitions

| Level | Description | Example |
|-------|-------------|---------|
| **Very Low** | Almost no risk | Local processing only |
| **Low** | Minimal risk | Only trusted libraries used |
| **Medium** | Attention required | Child process execution, environment variable access |
| **High** | Significant risk | Unvalidated external input, dangerous code execution |
| **Critical** | Immediate action required | Credential exposure, SQL injection |

## Best Practices

1. **Incremental Investigation**: Search broadly first, then perform detailed analysis
2. **Context-Focused**: Understand the intent of the code before making judgments
3. **Eliminate False Positives**: Distinguish between safe and dangerous usage
4. **Specific Citations**: Include file names and line numbers
5. **Provide Recommendations**: Present solutions, not just problems

## Output Format

Each security issue should include the following information:

- **Issue Type**: "External Communication", "Data Storage", "Vulnerability", "etc"
- **Risk Level**: "Very Low", "Low", "Medium", "High", "Critical"
- **Location**: "file/path:line_number"
- **Details**: "Specific issue content"
- **Impact**: "Impact on security"
- **Recommended Action**: "How to improve"

## References

For a detailed checklist and list of patterns, refer to `@./reference.md`.

## Notes

- This skill performs static analysis. Runtime behavior must be verified separately.
- Internal code of third-party libraries is not fully analyzed.
- There is a possibility of false positives; final judgments should be made by humans.
