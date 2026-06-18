# API_TESTING Skill for Sara

A general API testing skill for **Sara** — WE WILL's senior business-care quality agent.

Tests APIs directly at the API layer — no UI, no browser journey. Works on any project and any backend.

---

## Entry Point

```
/api-testing
```

---

## Three Modes

| Mode | What it does |
|---|---|
| **API_SWEEP** | Hunt for bugs endpoint by endpoint — auth, IDOR, mass assignment, schema, validation, error handling, business logic |
| **API_JOURNEY** | Validate a full business flow through ordered API calls — does the process actually succeed end to end? |
| **API_CONTRACT** | Compare live API responses against a documented spec — catch drift before it hits users |

---

## API Source Options

| Source | What happens |
|---|---|
| **URL** | Sara opens Chrome, navigates the feature flow, captures real network requests, extracts the endpoint list, closes the browser, then tests via direct API calls |
| **Collection file** | Postman, Insomnia, or HAR file — loaded directly |
| **Spec file** | OpenAPI, Swagger, or GraphQL SDL — loaded directly |
| **Manual list** | User describes or pastes the endpoints |

---

## How Intake Works

Sara uses **native UI selection** (not chat text) for all intake questions. Questions are batched to minimise round-trips:

- **Call 1** — Mode + API Source
- **Call 2** — Environment + Test Cases + Auth (only if inference fails)
- **Call 3** — Feature / Flow (only if URL is broad)

Sara infers everything she can first and only asks for what is missing.

---

## Key Features

### Security Coverage
- OWASP API Security Top 10
- BOLA / IDOR — systematic object ID substitution
- Mass Assignment — privileged field injection on POST / PUT
- BFLA — low-privilege user calling restricted endpoints
- Security misconfiguration — CORS, verbose headers, debug endpoints
- GraphQL introspection abuse detection

### GraphQL Support
- Introspection query abuse (Critical in production)
- Query depth limit enforcement
- Batching attack detection
- Field-level authorization testing
- GraphQL SDL as a valid contract source for API_CONTRACT

### Smart Intake
- Auth is inferred automatically from an unauthenticated probe before asking
- Prior run detection — offers full re-run or delta run when a previous run exists for the same feature
- All questions rendered as native Claude Code UI (radio buttons, selection chips)

### Production Safety
- Read-only by default in production
- 4-condition checklist before any mutating call
- No fuzzing, brute-force, or third-party API testing without explicit authorization
- PII redacted from all production reports

### Latency Monitoring
Stratified thresholds per endpoint type — not a single flat rule:

| Endpoint Type | Flag Threshold |
|---|---|
| Health / ping | > 500ms |
| Auth endpoints | > 1s |
| Payment / checkout | > 3s |
| Standard endpoints | > 5s |
| Report / export (async) | > 30s |

### Reporting
- Asked via native UI after execution — Markdown or HTML
- HTML report: summary cards, severity badges, color coding, collapsible evidence, RTL support, print-friendly
- Every finding includes: request, response, expected, actual, impact, reproduction steps, recommended fix

### Jira Integration
- Never files automatically
- Always asks after the report is produced
- Uses Sara's full `bug-report` skill workflow
- Security findings follow disclosure rules before filing
- Sensitive data redacted before any Jira write

### Run Memory
- Saved to `.sara/experience/runs/` with Sara's full metadata contract
- Updates `.sara/index.json` after every run
- Promotes reusable patterns to `.sara/experience/heuristics/`
- Promotes confirmed recurring failures to `.sara/experience/known-issues/`

---

## Verdict Reference

### API_SWEEP Overall Status
| Status | Condition |
|---|---|
| **Critical Issues Found** | At least one Critical or High finding |
| **Issues Found** | At least one Medium or Low finding, no Critical or High |
| **Clean** | No findings |

### API_JOURNEY Verdict
| Verdict | Condition |
|---|---|
| **Passed** | All steps passed, outcome achieved, consistency checks passed |
| **Partially Passed** | Mixed step results, final outcome not achieved |
| **Failed — Data Integrity** | All steps passed but a consistency check failed |
| **Failed** | Blocked at a critical step |

### API_CONTRACT Health
| Health | Condition |
|---|---|
| **Compliant** | No drift detected |
| **Minor Drift** | Non-breaking inconsistencies |
| **Major Drift** | Drift that may affect client behavior |
| **Breaking Drift** | Drift that breaks client integration |

### Severity Model
| Severity | Examples |
|---|---|
| **Critical** | Unauthenticated access, IDOR, financial manipulation, secret exposure, GraphQL introspection in production |
| **High** | Mass assignment accepted, major business rule violation, breaking contract drift |
| **Medium** | Incorrect validation, wrong status code, partial flow failure |
| **Low** | Unclear error message, cosmetic inconsistency, latency above threshold |
| **Needs Confirmation** | Unexpected behavior with no documented rule — held until user classifies |

---

## Installation

1. Copy `skill.md` into your Claude skills directory:
   ```
   ~/.claude/skills/api-testing/skill.md
   ```

2. Restart Claude Code in your project folder.

3. The skill appears as `/api-testing` and is listed in Sara's capability menu under **Test and verify**.

---

## Requirements

- [Sara agent](https://github.com/Eng-Mohammed-Samir) installed and configured
- `.mcp.json` with `chrome-devtools` MCP entry (required for URL-based network capture)
- `.mcp.json` with `jira` MCP entry (required for Jira filing)
- `.sara/project-context.md` (created automatically on first Sara run)

---

## Built by

**WE WILL** — Software Quality Services  
Sara is WE WILL's senior business-care quality agent.
