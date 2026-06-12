# 共享安全规则（SHARED_SECURITY_RULES）

> 所有项目级规则文件都会自动引用本文件中的安全规则。
> 参考：OWASP Top 10 + ZapDev SHARED_RULES + x1xhlol/system-prompts-and-models-of-ai-tools

```
## 🔐 Security Best Practices (MANDATORY for every file)

1. INPUT VALIDATION & SANITIZATION:
   - ALWAYS validate and sanitize ALL user inputs. No exceptions.
   - Use proper validation libraries (zod, yup, joi, class-validator) for structured data.
   - Escape HTML content before rendering to prevent XSS attacks.
   - Never trust client-side data validation — server validation is SOURCE OF TRUTH.

2. AUTHENTICATION & AUTHORIZATION:
   - Implement proper authentication checks on every protected endpoint.
   - Never expose or hard-code credentials, API keys, or tokens in source code.
   - Never store passwords in plain text — use bcrypt/argon2 with cost factor >= 12.
   - Validate user permissions BEFORE allowing actions.

3. DATA PROTECTION:
   - NEVER log sensitive information (passwords, tokens, PII, credit card fragments).
   - Use HTTPS for all external communications.
   - Sanitize database queries — use parameterized queries or ORMs, never string concatenation.
   - Implement rate limiting on auth endpoints (10 req/min recommended).

4. COMMON VULNERABILITY PREVENTION:
   - Prevent Cross-Site Scripting (XSS) — escape user input before rendering.
   - Prevent CSRF — use CSRF tokens for state-changing operations.
   - Prevent Path Traversal — validate and normalize file paths before use.
   - Prevent SQL Injection — use ORMs / parameterized queries, NEVER concatenate SQL strings.

5. DEPENDENCY & BUILD SAFETY:
   - Keep dependencies updated. Use npm audit / snyk to check for known vulnerabilities.
   - Never trust 3rd-party script execution (eval, setTimeout(string), Function constructor).

6. ERROR HANDLING:
   - Server errors: log the full stack on the server, return user-friendly message to client.
   - Never expose stack traces, database errors, or internal paths to end-users.
```

---

## 中文补充说明

以上安全规则为**强制项**，应用于所有代码生成请求。无论用户选择的是快速原型还是生产级，安全规则都应遵循。

**注意**："快速原型"档可以简化认证实现（如使用 mock auth），但绝不能在代码中留下安全漏洞（如 `db.query("SELECT * FROM users WHERE name = '" + userInput + "'")`）。
