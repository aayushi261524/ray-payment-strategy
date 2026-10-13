# Architecture

## Current implementation
A static browser application, without a backend or installed dependencies.

```text
Merchant action / instruction
          ↓
Conversation state and structured decision cards
          ↓
Local term interpretation / provider-aware intent parsing
          ↓
Merchant clarification and confirmation
          ↓
Deterministic readiness, eligibility and contractual checks
          ↓
Routing simulation and contribution calculations
          ↓
Scoped proposal bound to saved configuration revision
          ↓
Explicit merchant approval
          ↓
Simulated apply and field-level read-back
          ↓
Configuration verified, or safe stop
```

## Files
| File | Responsibility |
|---|---|
| `dist/index.html` | Static entry point |
| `dist/app.js` | Conversation state, scenarios, parsing, deterministic calculations, rendering and local adapter |
| `dist/style.css` | Merchant dashboard, conversation cards, evidence panel and light/dark themes |
| `qa/agent-check.cjs` | Automated state and calculation assertions using Node’s VM |
| `qa/agent-results.json` | Recorded check outcomes |

## Data and state
Saved demo configuration, choices and preferences live in browser localStorage. No customer data is sent to an application server. Scenario selection and Reset initialise controlled demonstration states. The saved revision identifies the configuration against which a merchant reviewed a proposal.

## Rules and economics
Eligibility checks cover supported issuer, card/method, amount, EMI tenure and campaign dates. Required offer routing is separate from ordinary-card allocation and fallback permission. Volume commitments account for traffic already allocated exclusively; an 84/16 ordinary split is a scenario result, not a universal recommendation.

Expected contribution combines supplied margin, fees and completion rates. The current base fixture gives Northstar ₹15.96 more expected contribution per eligible attempt. Campaign exposure uses historical matching volume and an assumed benefit. Neither calculation proves realised recovery.

## Trust boundaries
- Unknown or contradictory terms require confirmation.
- Document upload is evidence, not independent bank verification.
- Missing mandatory capability and infeasible agreements block application.
- Editing relevant decisions invalidates affected checks and approval.
- Rejecting a proposal leaves saved routing unchanged.
- Approval must match the current saved revision.
- Uncertain application or read-back mismatch stops further writes.
- Configuration verification does not verify financial outcomes.
- Expiry proposals preserve unrelated later edits.

## Intended production integration
An authenticated merchant dashboard would invoke a constrained interpretation service, capability and configuration readers, deterministic validators, and an authorised Optimizer adapter. MCP tools could expose those operations if supported and permissioned; no such connection is implemented here. Production needs merchant-scoped access, auditable approvals, secure document storage, idempotent operations, current configuration reads, notification consent and settlement evidence. No model belongs in the synchronous payment path.

## Deployment
`dist` is served as static content on Vercel. Deployment metadata and authentication are excluded from this source package.
