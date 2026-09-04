# 📐 Awesome REST API Design

![Status](https://img.shields.io/badge/Status-Active_Curation-brightgreen?style=for-the-badge&logo=github&logoColor=white)
![Maintainer](https://img.shields.io/badge/Maintainer-SunnyJayaRaju-orange?style=for-the-badge)

> **Personal, opinionated notes on REST API design — resources, patterns, and tools I've actually applied.**  
> This started as a fork of [Kikobeats/awesome-api](https://github.com/Kikobeats/awesome-api) — now maintained as my own curated reference.

---

## 🎯 Why This Exists

The original list is a broad directory of everything API-related. That's great for browsing, not for daily decisions.  
I needed a focused set: design guidelines I've followed, status codes I actually return, auth flows I've implemented, tools I reach for.

> **"If I've written it, reviewed it, or debugged it in prod, it's here. If not, it's not."**

---

## 🗂️ Table of Contents

- [📋 Design Principles & Guidelines](#-design-principles--guidelines)
- [🔢 HTTP Status Codes](#-http-status-codes)
- [🔐 Authentication & Authorization](#-authentication--authorization)
- [📝 Specification & Documentation](#-specification--documentation)
- [🧪 Testing & Debugging](#-testing--debugging)
- [🛡️ Security & Governance](#%EF%B8%8F-security--governance)
- [⚙️ CLI & Automation](#%EF%B8%8F-cli--automation)
- [🔗 Connected Repos](#-connected-repos)

---

## 📋 Design Principles & Guidelines

| Resource | Category | My Take |
|----------|----------|---------|
| **[Google API Design Guide](https://cloud.google.com/apis/design/)** | Enterprise Standard | My north star. Resource-oriented, predictable patterns, versioning via URL. Apigee enforces this by default. |
| **[Microsoft REST API Guidelines](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)** | Enterprise Standard | Comprehensive. Naming, versioning, pagination, error formats — maps cleanly to OpenAPI. |
| **[Zalando RESTful API Guidelines](https://zalando.github.io/restful-api-guidelines/)** | Open Standard | Concise, opinionated. Great for teams wanting a lighter baseline than Google/Microsoft. |
| **[Heroku HTTP API Design](https://github.com/interagent/http-api-design)** | Practical Patterns | Battle-tested conventions. Request IDs, ETags, rate-limit headers — the "real world" details. |
| **[Vinay Sahni's Pragmatic REST](https://www.vinaysahni.com/best-practices-for-a-pragmatic-restful-api)** | Quick Reference | One-page checklist. Good for code reviews: plural nouns, proper nesting, verbs in HTTP not URLs. |

---

## 🔢 HTTP Status Codes

| Resource | Category | My Take |
|----------|----------|---------|
| **[HTTP Status Codes Reference](https://httpstatuses.com/)** | Quick Lookup | Clean, searchable. My go-to when I forget 422 vs 400 or 409 vs 412. |
| **[REST API Tutorial Status Codes](https://www.restapitutorial.com/httpstatuscodes.html)** | Context + Examples | Groups by semantics (success, redirect, client error, server error) with REST-specific guidance. |
| **[MDN HTTP Status](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)** | Authoritative | Browser-vendor backed. Use when you need the exact RFC wording. |

**Codes I actually return:** `200`, `201`, `204`, `400`, `401`, `403`, `404`, `409`, `422`, `429`, `500`, `503` — the rest are edge cases.

---

## 🔐 Authentication & Authorization

| Resource | Category | My Take |
|----------|----------|---------|
| **[OAuth 2.0 RFC 6749](https://datatracker.ietf.org/doc/html/rfc6749)** | Spec | Dense but definitive. Authorization Code + PKCE for SPAs, Client Credentials for M2M, Device Code for CLIs. |
| **[OpenID Connect Core](https://openid.net/specs/openid-connect-core-1_0.html)** | Identity Layer | Adds `id_token`, `UserInfo` endpoint, discovery. Use when you need identity, not just access. |
| **[JWT RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519)** | Token Format | Stateless claims. Validate `exp`, `nbf`, `iss`, `aud` in gateway — never trust blindly. |
| **[OWASP Authentication Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html)** | Security Baseline | Password rules, MFA, session mgmt, brute-force protection. Checklist for every auth review. |
| **[Apigee OAuthV2 Policy](https://cloud.google.com/apigee/docs/reference/policies/oauth-v2-policy)** | Gateway Implementation | Native token mint/validate/introspect. Revokes, TTLs, scopes — all declarative in the proxy. |

---

## 📝 Specification & Documentation

| Tool | Category | My Take |
|------|----------|---------|
| **[OpenAPI 3.1](https://spec.openapis.org/oas/v3.1.0)** | Spec Standard | Single source of truth. Generates clients, docs, mocks, tests, gateway config. Don't hand-write — design in Stoplight, export. |
| **[Stoplight Studio](https://stoplight.io/open-source/studio)** | Visual Editor | Git-backed, real-time linting (Spectral), renders beautifully. Best authoring experience I've found. |
| **[Spectral](https://meta.stoplight.io/spectral/)** | Linter | Enforce style (naming, required fields, examples) in CI. Extensible rulesets — add your org conventions. |
| **[Redoc](https://redocly.github.io/redoc/)** | Reference Docs | Three-panel, searchable, code samples in 10+ langs. `redocly preview-docs` = instant local feedback. |
| **[Postman Collections](https://www.postman.com/)** | Runnable Docs | Share executable examples with non-engineers. `newman run` in CI = contract tests. |

---

## 🧪 Testing & Debugging

| Tool | Category | My Take |
|------|----------|---------|
| **[HTTPie](https://httpie.io/)** | CLI Client | `http GET :8080/api/users Authorization:"Bearer $TOKEN"` — readable, colorized, sensible defaults. |
| **[Postman](https://www.postman.com/)** | Collection Runner | Variables, environments, pre-request scripts. Newman + JUnit reporter = pipeline gate. |
| **[Mockoon](https://mockoon.com/)** | Local Mock Server | Import OpenAPI, customize responses, proxy mode for partial mocks. Zero config, runs offline. |
| **[Reqres](https://reqres.in/)** | Hosted Sandbox | Instant live endpoint for smoke tests. No auth, no setup, CORS enabled. |
| **[OWASP ZAP](https://www.zaproxy.org/)** | Security Scanner | Baseline scan on every PR. Finds injection, broken auth, sensitive data exposure. |

---

## 🛡️ Security & Governance

| Tool | Category | My Take |
|------|----------|---------|
| **[Spectral](https://meta.stoplight.io/spectral/)** | Design Governance | Enforce OpenAPI rules (required `examples`, no `anyOf`, proper `error` schemas) in PR checks. |
| **[OWASP API Security Top 10](https://owasp.org/www-project-api-security/)** | Threat Model | BOLA, broken auth, excessive data exposure — map each to gateway policies (rate limit, field masking, scope checks). |
| **[Apigee SpikeArrest / Quota](https://cloud.google.com/apigee/docs/reference/policies/spike-arrest-policy)** | Runtime Protection | SpikeArrest = smooth traffic (per-second), Quota = business limits (per-minute/hour/day). Use both. |
| **[mTLS](https://cloud.google.com/apigee/docs/api-platform/security/mtls)** | Zero-Trust Transport | Service-to-service cert validation. Apigee `TargetEndpoint` `SSLInfo` + KVM certs = no shared secrets. |

---

## ⚙️ CLI & Automation

| Tool | Category | My Take |
|------|----------|---------|
| **[jq](https://stedolan.github.io/jq/)** | JSON Processor | `curl -s $URL | jq '.data[] | select(.status=="active") | .id'` — the universal API data knife. |
| **[httpie](https://httpie.io/)** | HTTP CLI | `http --print=hb POST $URL token=$TOKEN` — cleaner than curl for headers + body. |
| **[newman](https://github.com/postmanlabs/newman)** | CI Runner | `newman run collection.json -e env.json --reporters cli,junit` — JUnit XML for any CI. |
| **[redocly-cli](https://redocly.com/docs/cli/)** | Spec Pipeline | `redocly lint spec.yaml && redocly bundle spec.yaml -o bundle.yaml` — validate + bundle in CI. |
| **[apigeecli](https://github.com/apigee/apigeecli)** | Apigee Automation | Deploy proxies, manage KVMs, create products — all from terminal. Replaces Maven plugin. |

---

## 🔗 Connected Repos

This list lives alongside my hands-on work:

| Repo | Purpose |
|------|---------|
| **[Apigee-Lab](https://github.com/SunnyJayaRaju/Apigee-Lab)** | Production-style Apigee proxies (JWT, Caching, FaultRules, ServiceCallouts) |
| **[Curious-Explorer](https://github.com/SunnyJayaRaju/Curious-Explorer)** | Concept index: "Waiter vs Kitchen" (proxies), "Hotel Key Card" (OAuth), "Bouncer vs Bartender" (SpikeArrest vs Quota) |
| **[Awesome-Api-Management-Tools](https://github.com/SunnyJayaRaju/Awesome-Api-Management-Tools)** | Curated gateway/tooling list: Apigee, Kong, Spectral, apigeelint, Redoc, etc. |

---

## 🧭 How I Evaluate Resources

1. **Does it solve a real design decision I face?** (Not academic purity)
2. **Can I apply it in a gateway/proxy today?** (Not "someday")
3. **Is the mental model learnable in an afternoon?** (If not, it's a liability)
4. **Does it survive a 2 AM debug session?** (Clear errors, escape hatches, good logs)
5. **Does it play nice with GitOps?** (Declarative, diffable, reviewable)

---

## 📝 Changelog

- **2026-09-04** — Initial rewrite: fork → personal curated REST API design notes. Trimmed 180+ lines to ~160 focused entries.

---

## 🙏 Attribution

Original list © 2016+ [Kikobeats](https://github.com/Kikobeats) (MIT).  
This curated version © 2026 [SunnyJayaRaju](https://github.com/SunnyJayaRaju) — same license, new voice.

> *Curated with 🧠 by someone who learns by breaking things in staging first.*