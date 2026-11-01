AI Identity Management Lab | Keycloak, OAuth 2.0 & OIDC

![Keycloak](https://img.shields.io/badge/Keycloak-2563EB?style=for-the-badge)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![OAuth 2.0](https://img.shields.io/badge/OAuth_2.0-475569?style=for-the-badge)
![OpenID Connect](https://img.shields.io/badge/OpenID_Connect-475569?style=for-the-badge)
![JWT Validation](https://img.shields.io/badge/JWT_Validation-475569?style=for-the-badge)
![PKCE S256](https://img.shields.io/badge/PKCE_S256-0F766E?style=for-the-badge)
![RBAC](https://img.shields.io/badge/RBAC-0F766E?style=for-the-badge)
![Least Privilege](https://img.shields.io/badge/Least_Privilege-0F766E?style=for-the-badge)

Purpose

Learn how to control an AI agent's access to tools and understand what evidence is needed to identify who authorized its actions.

I built a local lab where a Python script simulated an agent calling a protected inventory API. I configured read-only access, tested permission boundaries and added a separate human browser login.

Tools: Keycloak · Docker Compose · Python · PyJWT
Concepts: OAuth 2.0 · OpenID Connect · JWT validation · RBAC · Least privilege · PKCE S256

What I Built

A service account that authenticates using OAuth client credentials.

A mock API that validates token signatures, issuer, audience and expiration before checking permissions.

Read-only access enforced through Keycloak client roles.

Human browser login using Authorization Code with PKCE S256, including state and nonce validation.


## Results

| Test | Result | Status |
| --- | --- | --- |
| Authorized inventory read | 200 — Allowed | Passed |
| Write without permission | 403 — Denied | Passed |
| Read role removed; new token requested | 403 — Denied | Passed |
| Missing token | 401 — Rejected | Passed |
| Malformed token | 401 — Rejected | Passed |
| Tampered token signature | 401 — Rejected | Passed |
| Incorrect client secret | 401 — Authentication failed | Passed |
| Incorrect token audience | 401 — Rejected | Passed |
| Token before expiration | 200 — Allowed | Passed |
| Same token after expiration | 401 — Rejected | Passed |
| Browser OIDC login with PKCE S256 | Login and API permission checks succeeded | Passed |


Human OIDC login and API checks

Completed successfully during testing

Problem I Solved

Changing the explicit audience mapper did not initially block access. Keycloak's default roles scope was also adding the correct API audience. I isolated the mappers and confirmed that a token with only the incorrect audience was rejected.

This strengthened my ability to troubleshoot token claims rather than relying on a single configuration setting.

Skills Demonstrated

Service-account and non-human identity configuration.

Role-based access control and least-privilege enforcement.

OAuth/OIDC configuration and JWT validation.

API access testing, permission removal and token-expiration testing.

Troubleshooting client scopes and audience mappings.

Docker-based lab deployment and documenting test results.

What This Taught Me About AI Identity

An agent needs its own identity and clearly limited permissions. Successful authentication alone does not prove that a human approved a particular action.

This lab tested service-account access and human login separately using a simulated agent. Connecting human approval to an agent action through delegation and audit records is the next step. No AI model or token-exchange flow was included.
