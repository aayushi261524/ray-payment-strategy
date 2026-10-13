# Ray · Payment Strategy

A proposed merchant-facing payment-routing capability inside Razorpay’s Ray experience. Independently built concept prototype; not an official Razorpay release.

**Live demo:** https://ray-payment-routing.vercel.app

## The merchant problem
A payment can succeed through a route incompatible with a merchant’s confirmed commercial arrangement. The prototype helps a merchant interpret terms, compare permitted routing choices, approve an exact change and verify the saved configuration.

## Explore
Choose **Create payment routing** or **Review my payment routing**. Create supports optional agreements and merchant preferences. Review includes campaign readiness, amendments, expiry, performance opportunities and failure scenarios. Use **Reset demo** for a clean starting state.

## Run locally
No dependency installation or build is required. Serve the `dist` directory with any static HTTP server, for example:

```sh
python3 -m http.server 8000 --directory dist
```

Open http://localhost:8000.

## Check behaviour
From this repository root, with Node.js installed:

```sh
node qa/agent-check.cjs
```

The suite covers the two journeys, provider-bound percentage parsing, eligibility boundaries, calculations, contractual constraints, readiness, approvals and failure stops. It uses a mocked browser environment; this does not replace manual browser testing.

## Supporting material
- [One-page product note (PDF)](docs/ray_one_pager_aayushi.pdf)
- [Architecture and trust boundaries](ARCHITECTURE.md)
- [Detailed AI build log](AI_BUILD_LOG.md)
- [AI assistance summary](AI-BUILD-NOTES.md)
- `qa/agent-results.json`: recorded automated results.

## Prototype boundaries
All merchant, provider and network records are fictional demonstration data. Ray uses scripted conversation and local parsing, not a live language model. Configuration writes and read-back are simulated. There are no real payment operations, partner verification, settlement ingestion or notification transmissions. Financial figures are estimates, not verified recovery. Undo is planned and is labelled Coming soon.

## Read-only sharing
The public repository lets reviewers view or download the source. No reviewer collaborator write permissions are required. Do not add credentials, customer documents, deployment tokens or browser storage exports.
