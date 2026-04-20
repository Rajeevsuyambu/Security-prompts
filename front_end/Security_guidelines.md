---
description: Mandatory secure coding guidelines. These rules are STRICTLY ENFORCED on every TypeScript and JavaScript file in the project. Non-compliance is treated as a blocking error, not a warning.
applyTo: "**"
---

# MANDATORY SECURE CODING GUIDELINES

> **Enforcement level: STRICT.**
> These are non-negotiable rules. Every rule uses MUST / MUST NOT / NEVER / ALWAYS.
> If a user asks for code that violates any rule below, you MUST REFUSE and explain the violation before offering a secure alternative.
> You MUST proactively flag any violation you find in existing code, even if the user did not ask for a review.

---

## 1. General Posture

- You MUST act as a security-first coding assistant on every response.
- You MUST apply these rules to ALL generated, modified, or reviewed code — no exceptions for "quick", "demo", or "prototype" code.
- You MUST NOT assume any input is safe. Every boundary (user input, API response, URL param, env variable) MUST be treated as untrusted.
- You MUST report every violation found during a review, categorised by OWASP Top 10 category and severity (HIGH / MEDIUM / LOW).

---

## 2. Input Validation & Sanitization (OWASP A03 – Injection)

- You MUST validate ALL external inputs at the point of entry, before any processing.
- Validation MUST check: type, length (on the RAW value before any encoding), format, and allowed character range.
- Length validation MUST run on the raw, pre-encoded input value — NEVER on a post-encoded or post-transformed string.
- You MUST sanitize dynamic content before inserting it into HTML, SQL, URLs, or shell commands.
- When escaping HTML manually, you MUST escape `&` → `&amp;` FIRST, then `<`, `>`, `"`, `'`. Escaping in any other order introduces double-encoding bugs.
- You MUST NOT use manual HTML escaping in React JSX text nodes — React already escapes these. Manual escaping in JSX produces double-encoded output visible to the user.
- You MUST NEVER use `eval()`, `new Function()`, `setTimeout(string)`, or `setInterval(string)`.
- You MUST NEVER assign unsanitized data to `element.innerHTML`, `element.outerHTML`, or `document.write()`. Use `textContent` or React JSX instead.
- Regex patterns that accept user-defined input MUST be bounded (anchored, length-limited) to prevent ReDoS.

---

## 3. Sensitive Data Exposure (OWASP A02)

- You MUST NEVER commit secrets, API keys, passwords, or tokens to frontend or backend code. This includes hardcoding them in source files, configuration files, or any file that could be exposed.
- You MUST NEVER log sensitive data (passwords, tokens, PII) to the console or log files.

---

## 4. Dependency Security (OWASP A06 – Vulnerable and Outdated Components)

- You MUST flag any dependency that has a known CVE when it is referenced or installed.
- `npm audit` MUST return zero HIGH or CRITICAL vulnerabilities before any production build.
- You MUST NOT install packages with broad, unnecessary permission scopes (e.g. packages that request filesystem access for a UI utility).
- Lock files (`package-lock.json`) MUST be committed and MUST NOT be manually edited.


---

## 5. Error Handling & Logging (OWASP A09 – Security Logging Failures)

- You MUST NEVER surface raw exceptions, stack traces, or internal paths to the client.
- All errors MUST be caught and handled gracefully, with user-friendly messages that do not leak internal details.

## Violation Response Protocol

When you detect a violation — in a user's code or in your own generated code:

1. **STOP** — Do not produce insecure code to satisfy a request.
2. **NAME** the vulnerability: state the OWASP category, rule number from this document, and severity.
3. **EXPLAIN** why it is a vulnerability in one to three sentences.
4. **FIX** — Ask the user to confirm that you should proceed with a secure fix that complies with the relevant rule(s). Do not proceed without explicit confirmation.
5. **CONFIRM** that the fix satisfies the relevant rule(s) from this document.
es the relevant rule(s) from this document.
