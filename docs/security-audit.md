# Security Audit — DentBot

Scope: the patient chatbot and the modules it uses (appointments, patient data, configuration).
Method: manual code review of controllers, models and views, guided by the OWASP Top 10, plus manual tests in the browser.

Result: **3 vulnerabilities found.**

| # | Vulnerability | OWASP category | Risk | Mitigation |
|---|---|---|---|---|
| 1 | API key hardcoded in source code | A02 Cryptographic Failures / secrets management | High | Recommended: move to a configuration file excluded from Git and rotate the key |
| 2 | IDOR: changing an ID in the request returned another patient's data | A01 Broken Access Control | High | Server checks that the record belongs to the logged-in user before returning it |
| 3 | XSS in chatbot content rendered as HTML | A03 Injection | High | Output escaped with `htmlspecialchars` before rendering |



## Lessons

- Secrets never go in the code; once committed, a key must be considered leaked and rotated.
- Authorization is checked on the server for every request, not only by hiding buttons in the UI.
- Anything that comes from a user or from an AI model is untrusted input and is escaped before rendering.
