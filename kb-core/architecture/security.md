# Security

**Category:** Architecture

**OWASP top hits:** injection (parametrize), broken auth, broken access control (deny default), SSRF, deserialization, vuln deps, misconfig.

**AuthN:** OIDC/OAuth2, short-lived JWT + refresh, rotate keys. AuthZ separate: RBAC/ABAC, check per request, server-side.

**Secrets:** never in repo; vault/SM, env injection, rotate. Scan commits.

**Supply chain:** lockfiles, pin, SCA scan, SBOM, signed artifacts.

**Transport:** TLS everywhere, HSTS, secure cookies, CSP, sane CORS.

**Input:** validate at boundary (Zod/validator/Pydantic), output-encode, least privilege.

**See also:** kb-ops/checklists/code-review.md.
