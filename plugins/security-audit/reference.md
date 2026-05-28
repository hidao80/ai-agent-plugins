# Security Audit - Reference Document

## Search Pattern List

### 1. External Communication Patterns

#### HTTP/HTTPS Communication
```regex
fetch\s*\(
XMLHttpRequest
axios\.(get|post|put|delete|patch)
http\.request
https\.request
request\(
got\(
superagent
needle
```

#### WebSocket Communication
```regex
new\s+WebSocket\(
ws://
wss://
socket\.io
```

#### Other Network Communication
```regex
net\.connect
dgram\.
dns\.resolve
```

### 2. Data Storage Patterns

#### Browser Storage
```regex
localStorage\.(setItem|getItem)
sessionStorage\.(setItem|getItem)
document\.cookie
```

#### Databases
```regex
indexedDB\.open
openDatabase\(
new\s+Dexie\(
mongoose\.connect
sequelize
knex
```

#### File System (Node.js)
```regex
fs\.writeFile
fs\.appendFile
fs\.createWriteStream
writeFileSync
```

### 3. Dangerous Code Execution Patterns

#### Dynamic Code Execution
```regex
eval\s*\(
new\s+Function\(
setTimeout\s*\(\s*["']
setInterval\s*\(\s*["']
```

#### Command Execution
```regex
child_process\.exec
child_process\.spawn
child_process\.execFile
shell\.exec
require\(['"]child_process['"]\)
```

#### Template Injection
```regex
<script>
innerHTML\s*=
dangerouslySetInnerHTML
v-html=
```

### 4. Authentication and Authorization Patterns

#### Sensitive Information Patterns
```regex
password\s*[:=]
api[_-]?key\s*[:=]
secret\s*[:=]
token\s*[:=]
private[_-]?key\s*[:=]
access[_-]?token\s*[:=]
bearer\s+
authorization:\s*
```

#### Environment Variable Access
```regex
process\.env\.
\$\{env:
ENV\[['"]
os\.environ
```

### 5. SQL Injection

```regex
SELECT\s+.*?\$\{
INSERT\s+INTO\s+.*?\$\{
UPDATE\s+.*?SET\s+.*?\$\{
DELETE\s+FROM\s+.*?\$\{
"SELECT\s+.*?"\s*\+\s*
'SELECT\s+.*?'\s*\+\s*
query\(.*?\+.*?\)
```

### 6. XSS (Cross-Site Scripting)

```regex
innerHTML\s*=\s*.*?\+
\.html\(.*?\+
document\.write\(
eval\(.*?request
\$\{.*?req\.
dangerouslySetInnerHTML.*?\{
v-html=
```

### 7. Path Traversal

```regex
\.\./
\.\.\\
path\.join\(.*?req\.
fs\.readFile\(.*?req\.
```

### 8. CSRF (Cross-Site Request Forgery)

```regex
cors\(
Access-Control-Allow-Origin:\s*\*
withCredentials:\s*true
```

## Checklists

### External Communication Checklist

- [ ] Verify the destination of all fetch calls
- [ ] Identify locations where XMLHttpRequest is used
- [ ] Verify WebSocket connection targets
- [ ] Presence of analytics libraries (Google Analytics, etc.)
- [ ] Presence of tracking code (Facebook Pixel, etc.)
- [ ] Verify the content of data sent to external APIs
- [ ] Check for data transmission without user consent

### Data Storage Checklist

- [ ] Content of data stored in localStorage
- [ ] Purpose of sessionStorage usage
- [ ] Information stored in Cookies
- [ ] Status of IndexedDB usage
- [ ] Whether personal information is encrypted
- [ ] Data retention period settings
- [ ] Implementation of data deletion functionality

### Vulnerability Checklist

#### Code Execution
- [ ] No use of eval()
- [ ] No use of new Function()
- [ ] Input validation when executing child processes
- [ ] Safety of dynamic require()

#### Injection
- [ ] SQL query parameterization
- [ ] Sanitization of user input
- [ ] HTML escaping
- [ ] Validation of command-line arguments

#### Authentication and Authorization
- [ ] Password hashing
- [ ] Secure storage of tokens
- [ ] Session management implementation
- [ ] Permission check implementation

#### Data Exposure
- [ ] Level of detail in error messages
- [ ] Removal of debug code
- [ ] Proper management of environment variables
- [ ] .gitignore configuration

### Third-Party Library Checklist

- [ ] Verify the purpose of all dependent libraries
- [ ] Check the last update date (maintenance status)
- [ ] Check for known vulnerabilities (npm audit, etc.)
- [ ] Verify licenses
- [ ] Evaluate download count and trustworthiness
- [ ] Remove unnecessary dependencies

## Risk Assessment Matrix

### Risk Level Criteria

| Item | Very Low | Low | Medium | High | Critical |
|------|----------|-----|--------|------|----------|
| **External Communication** | None | Trusted API | Third-party API | Unknown endpoint | Unencrypted transmission |
| **Data Storage** | None | Config values only | Non-sensitive data | Personal info (encrypted) | Personal info (plaintext) |
| **Code Execution** | None | Static code only | Validated dynamic execution | Restricted dynamic execution | Unvalidated dynamic execution |
| **Input Validation** | N/A | Full validation | Basic validation | Partial validation | No validation |
| **Authentication** | N/A | Multi-factor auth | Basic auth | Weak auth | No auth |

### Vulnerability Severity

| CVSS | Level | Response Deadline |
|------|-------|-------------------|
| 9.0–10.0 | Critical | Immediately |
| 7.0–8.9 | High | Within 1 week |
| 4.0–6.9 | Medium | Within 1 month |
| 0.1–3.9 | Low | Next release |

## Security Best Practices

### 1. Input Validation

```typescript
// ❌ Bad example
const userId = req.query.id;
db.query(`SELECT * FROM users WHERE id = ${userId}`);

// ✅ Good example
const userId = parseInt(req.query.id);
if (isNaN(userId) || userId < 0) {
  throw new Error('Invalid user ID');
}
db.query('SELECT * FROM users WHERE id = ?', [userId]);
```

### 2. Output Escaping

```javascript
// ❌ Bad example
element.innerHTML = userInput;

// ✅ Good example
element.textContent = userInput;
// or
element.innerHTML = DOMPurify.sanitize(userInput);
```

### 3. Sensitive Information Management

```bash
# ❌ Bad example
API_KEY=abc123  # Hardcoded directly in the code

# ✅ Good example
# Store in a .env file
API_KEY=abc123

# Add to .gitignore
echo ".env" >> .gitignore
```

### 4. Child Process Execution

```typescript
// ❌ Bad example
const { exec } = require('child_process');
exec(`ls ${userInput}`);

// ✅ Good example
const { spawn } = require('child_process');
spawn('ls', [userInput]);
```

### 5. CORS Configuration

```javascript
// ❌ Bad example
app.use(cors({ origin: '*' }));

// ✅ Good example
app.use(cors({
  origin: 'https://trusted-domain.com',
  credentials: true
}));
```

## Criteria for Evaluating External Communications

### Conditions for Trustworthy External Communication

1. **Necessity**: Required for feature implementation
2. **Transparency**: Clearly disclosed to users
3. **Consent**: User consent has been obtained
4. **Encryption**: Uses HTTPS/TLS
5. **Minimality**: Only the minimum necessary data is transmitted
6. **Privacy**: Personal information is handled appropriately

### Characteristics of Problematic External Communication

1. **Hidden**: No notification to users
2. **Excessive**: Unnecessary data transmission
3. **Plaintext**: No encryption
4. **Tracking**: Tracking without consent
5. **Unknown**: Destination is unclear
6. **Leakage**: Transmission of sensitive information

## Common False Positive Patterns

### 1. Code in Comments

```javascript
// Do not use eval() in this function
function safeExecute(code) {
  // Safe implementation
}
```

→ `eval()` inside a comment is never actually executed

### 2. String Literals

```javascript
const warning = "eval() is dangerous";
console.log(warning);
```

→ A mention as a string is not a problem

### 3. Test Code

```javascript
// test/security.spec.ts
it('should prevent eval injection', () => {
  expect(() => eval(userInput)).toThrow();
});
```

→ Usage for testing purposes may be acceptable

### 4. Development Debug Code

```javascript
if (process.env.NODE_ENV === 'development') {
  console.log('Debug:', sensitiveData);
}
```

→ Not executed in production environments

## Tools and Automation

### Recommended Tools

1. **npm audit**: Vulnerability check for npm dependencies
2. **Snyk**: Continuous security monitoring
3. **ESLint security plugins**: Static analysis
4. **OWASP ZAP**: Dynamic security testing
5. **SonarQube**: Code quality and security analysis

### Command Examples

```bash
# Check npm dependencies for vulnerabilities
npm audit

# Detailed report
npm audit --json

# Auto-fix (use with caution)
npm audit fix

# Snyk scan
npx snyk test

# ESLint security check
npx eslint --plugin security .
```

## Report Template

```markdown
## Security Audit Report

**Audit Date**: YYYY-MM-DD
**Target Repository**: [Repository Name]
**Audit Scope**: [Full Scan / Partial Scan]

### Executive Summary

[Provide an overall assessment in 2–3 sentences]

### Findings Summary

| Category | Risk Level | Count |
|----------|------------|-------|
| External Communication | [Level] | [Count] |
| Data Storage | [Level] | [Count] |
| Vulnerabilities | [Level] | [Count] |
| Libraries | [Level] | [Count] |

### Detailed Findings

#### 1. External Communication

**Findings**:
- [Specific content]

**Risk Assessment**: [Level]

**Recommended Actions**:
- [Action 1]
- [Action 2]

#### 2. Data Storage

[Same format as above]

#### 3. Security Vulnerabilities

[Same format as above]

#### 4. Third-Party Libraries

[Same format as above]

### Overall Assessment

**Risk Level**: [Very Low / Low / Medium / High / Critical]

**Observations**:
[Overall evaluation and summary]

### Prioritized Recommendations

#### Immediate Action
- [ ] [Item 1]
- [ ] [Item 2]

#### Within 1 Week
- [ ] [Item 1]
- [ ] [Item 2]

#### Within 1 Month
- [ ] [Item 1]
- [ ] [Item 2]

#### At Next Release
- [ ] [Item 1]
- [ ] [Item 2]

### References

- [Related documentation]
- [Reference URL]
```

## Related Links

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [CWE Top 25](https://cwe.mitre.org/top25/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [Node.js Security Best Practices](https://nodejs.org/en/docs/guides/security/)
- [MDN Web Security](https://developer.mozilla.org/en-US/docs/Web/Security)
