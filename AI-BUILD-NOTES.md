# AI assistance during the build

I used GitHub Copilot for coding assistance and AI as a supporting tool to explore the problem, compare research, challenge assumptions, refine the merchant journey, and assist with implementation and testing. I set the product direction and reviewed the recommendations: merchant control, optional commercial arrangements, explicit approval, source-linked clarification and safe failure states.

The iterative review helped identify percentage-to-provider parsing errors, volume-floor denominator issues, and wording that confused protecting commercial value with recovered revenue. These were addressed through changes and repeatable checks.

The current demo uses scripted conversations and local parsing rather than a live language model. AI assistance during development is separate from the model integration proposed for a production version.

Supporting evidence: application source in `dist/`, executable checks in `qa/agent-check.cjs`, and recorded results in `qa/agent-results.json`. No unsupported speed multiplier or production integration is claimed.
