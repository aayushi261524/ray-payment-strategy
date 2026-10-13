# AI Build Log

**Ray, Payment Strategy for Razorpay**
Aayushi · PGP 2027 · Razorpay x ISB AI PM Build Challenge, Track 2 (Recover and Grow with AI)
Prototype: https://ray-payment-routing.vercel.app/

The problem behind Ray came from my years on PayPal's Merchant Pricing and Revenue Operations team, where I saw how much money depends on which gateway a payment goes through. I set the direction and made the product decisions. AI helped me explore more evidence, challenge my assumptions, implement my specifications and test more possibilities than I could have alone.

---

## 1. AI tools used

| Tool | How it supported the work |
|---|---|
| GitHub Copilot | Coding assistance during prototype development, guided by my product specification |
| Vercel | Public hosting; not an AI tool |
| GitHub | Read-only source sharing for reviewers; not an AI tool |

The named coding tool was GitHub Copilot; its exact model version was not recorded. The broader research, critique and design workflow below is described as AI-assisted. The tool list is selective rather than an exhaustive inventory.

---

## 2. Key prompts and workflows

### The workflow I repeated

For every major piece of work I followed the same loop. I proposed the idea or wrote the spec, AI challenged or implemented it, I decided what to keep, and AI helped me check the result before I moved on.

### Key prompts, in the order the work happened

These are reconstructed, condensed prompts representing the questions and iterations discussed during the build, not verbatim transcripts or evidence of separate reruns. I brought reading, links, notes and product decisions, and asked AI to extend, challenge or implement them. Statements inside prompts represent questions or hypotheses at that stage, not validated research findings.

**1. Reading Razorpay's direction.** I tore down the Agent Studio page and its sibling pages (Ray on WhatsApp, Agentic Business Banking, Agentic Payments) myself, then shared my teardown with my secondary research notes.
> "Here is my teardown of Agent Studio. My reading is that Razorpay is moving from processing payments to owning what happens after them. The journey tabs run as one lifecycle, from cart recovery to disputes to feedback. Every industry vertical reuses the same four building blocks: drop-off recovery, failed-payment recovery, dispute defence and reporting. The enterprise case studies quote results like '10+ pp added to recovery' with no baseline or holdout. And there is no pricing, no self-serve builder, no SME story and no link to Ray. Add anything I've missed, challenge my reading where you disagree, and go through my research synthesis claim by claim. Tell me which numbers I must verify against a primary source before I put them in front of judges, and where my persona is still undefined."

**2. Grounding it in what a merchant actually sees.** I opened a Razorpay test account, walked the dashboard, and wrote down what a self-serve merchant gets.
> "This is what I found in the test account. I get a failed-payment count with no explanation. Ecosystem Health shows a live SBI downtime, but nothing links it to my affected customers. There are about 40 CSV reports, a reminder on/off toggle, and manual payment links. Agent Studio redirects to the home page, and Ray shows up only as empty cards. So the merchant gets signals but no decisions. Compare this with the Stripe, Adyen and PayPal pages I've linked, and sort each idea into 'genuine white space' or 'someone already does this'. Be strict. I'd rather drop an idea now than have a judge tell me Stripe shipped it last year."

**3. Bringing in my own domain hypothesis.** From my pricing and revenue work, I proposed routing as the lever.
> "I want to test an idea from my pricing work. Merchants with more than one gateway sign commercial deals: bank offers that only work on one gateway, EMI partners, negotiated fees, and volume commitments like 'at least 15% of card payments to this gateway'. My hypothesis is that nothing checks routing against those deals, so money leaks silently, because the payment still succeeds. First, separate this from payment splits, which is a different problem. Then check it against Optimizer's docs and the orchestrator material I've shared. Tell me honestly whether it is revenue recovery or just cost saving, and who the merchant is. I suspect it is a multi-gateway, mid-size merchant, not a small seller on one gateway."

**4. Learning the basics properly before committing.**
> "Before I commit to this, I want to understand the money flow from first principles. Explain gateway versus aggregator, and the roles of acquirer, issuer and network. Then explain how a merchant makes and loses money on a payment: false declines, fee leakage, chargebacks, and unused offers and negotiated rates. Give me the actual pricing context, including card MDR caps, zero-MDR RuPay debit, the new UPI fee, and typical gateway list rates. Then go through what Optimizer can do today, rule by rule, including custom rules, priorities, percentage splits, the Smart Router and custom identifiers, so I know exactly where the gap starts."

**5. Adding primary research.** After my call with a payments practitioner, and conversations with merchants and payments professionals, I shared my notes.
> "Here are my notes from the practitioner call. The transcript is noisy, so mark anything you're unsure of. Pull out every insight and anonymise it. Then tell me which insights change the product and which belong on the roadmap. Three stood out to me. Routing patterns from similar merchants could be a starting template for new ones. Failed payments could be prevented by grouping error codes. And a failed payment earns the gateway nothing, so success is in everyone's interest. He also warned me that just because you can add something doesn't mean you need to, so keep scope discipline in mind and tell me what to cut."

**6. Asking a second model to attack it.** I took the problem statement and the full idea to ChatGPT for an independent critique, then brought its verdict back.
> "Another AI reviewed my problem statement and says several gaps are overclaimed. Optimizer does have manual cost rules. Network suggestions exist through the Performance Report. Some providers already route offers using card-range rules. And I have too many features. Go through each point and tell me where it is right. Rewrite the claims so they only say what the evidence supports. Don't defend earlier drafts. I want the version a Razorpay product lead couldn't poke holes in."

**7. Filling the evidence gap.**
> "Judges will ask for proof that deals and routing actually go out of sync. Search merchant forums, reviews, bank offer terms and payment-provider docs for direct evidence. Ignore consumer complaints, because this is a merchant problem. Also find out why so many merchants trusted the orchestrators and stayed with them, in their own words, not marketing copy. If Razorpay's own docs show deal terms being wired into routing by hand, find the exact page."

**8. Designing the merchant journeys.** Once the core idea was set, I split the experience into journeys myself (see the next section) and asked AI to pressure-test the split.
> "Here is how I want to split the merchant experience. There are two entry points, creating routing and reviewing saved routing. Tie-ups are optional, and there are three kinds: a bank offer, a negotiated fee, and a fee with a volume commitment. A merchant can also have two tie-ups that conflict. After setup, Ray keeps watching for offers ending, terms changing, a teammate editing a rule, provider disruptions and new offer opportunities. And every place a change can go wrong needs its own safe stop. Walk through each journey as Voltkart's payments manager. Tell me where a merchant would get stuck, which branches I've missed, and which journeys are really the same journey."

**9. Turning my feedback into a build spec.** I consolidated detailed feedback on the early builds and asked AI to turn it into an implementation specification.
> "Here is my feedback on build 1. It works, but it feels like a settings page, not an agent. Turn my 31 points into a build spec. Ray should lead the conversation and decision cards should appear inside it. Each of the 11 AI moments I've listed needs to be visible on screen. Spell out the branch logic for no tie-up, conflicting tie-ups and volume commitments. Keep backup permission separate from the traffic split, because they are different contractual permissions. Keep every rupee calculation deterministic, and write acceptance criteria for each screen so I can test against them."

**10. Testing against my acceptance criteria.**
> "Open the live prototype from a clean state and walk every journey in the picker. For each step, report what Ray says, which buttons appear, and anything that blocks the flow, contradicts the one-pager or shows a different number. Then check the maths independently: ₹12,400 at risk, ₹15.96 per payment, the 95/5 and 100% figures, and whether 84/16 really keeps Indus PG above its 15% floor once campaign payments go to Northstar."

### How I split the merchant journeys

I designed the experience around what a payments manager actually does, not around screens. There are two complete journeys that run from start to Verified, and 26 scenario paths that drop the merchant into a specific situation. I grouped them into four kinds.

| Group | Scenario paths | Why it exists |
|---|---|---|
| **Set up routing** (onboarding) | Create without an agreement, with the Meridian EMI offer, with negotiated fees, with Indus pricing (volume floor), with both agreements, and conflicting agreement scopes | Tie-ups are optional, and the three kinds behave differently. An offer may require a specific route, a fee deal changes cost comparisons, and a volume floor constrains permitted splits |
| **Review and maintain** | Review festive EMI, review fees and contribution, review completion nudge, post-sale routing review, provider disruption review, amended minimum purchase, teammate changed UPI, offer ended | Deals change after setup. Ray re-checks routing when terms are amended, offers expire, a teammate edits a rule or a provider has an outage, and it changes only the affected scope |
| **Grow** | New card and offer nudge, explore bank opportunities | Ray uses the merchant's payment patterns, such as Meridian cards making up 38% of card payments, to suggest an offer worth negotiating and draft a partnership brief |
| **Safe stops** | Partner activation pending, unresolved offer terms, required EMI not enabled, authorised backup not ready, unsupported requirement, missing economic inputs, already compatible, concurrent edit, read-back mismatch, application uncertain | Every point where a routing change could go wrong has a defined, safe outcome. Ray saves the draft, asks a question or blocks the write, and never guesses |

Three design choices shaped the split:
- **Backup is asked separately from the traffic split.** Sending 10% of payments to a gateway is not the same as being allowed to retry failed payments there.
- **Only confirmed choices enter the simulation.** Anything unresolved, like an unconfirmed clause, is saved as pending rather than assumed.
- **Changes are scoped.** A campaign rule touches only campaign payments, and UPI and other methods stay unchanged unless the merchant includes them.


---

## 3. AI outputs I rejected, corrected or significantly improved

### Rejected: AI scored the idea's differentiation as low

In one critique round, AI argued that Razorpay Optimizer already does routing, so the idea wasn't differentiated. I disagreed and kept the position, but I had to evidence it. Existing routing rules and simulators establish that merchants already have routing controls. My proposed differentiator is the connected workflow: interpret the merchant’s terms, clarify contradictions, test routing against confirmed obligations, and verify the approved configuration. This is a product hypothesis based on the reviewed material, not proof that competitors lack unpublished capabilities.

### Corrected: a financial example that contradicted the prototype

During a late revision, AI changed the one-pager's comparison to say Indus PG earns ₹15.96 more per payment. When I checked it against the prototype's inputs, it was wrong. Northstar charges ₹180 at 97.8% success and Indus PG ₹200 at 98%, so Northstar is the better route. I restored the correct version and added the inputs next to the figure, so a judge can check it.

### Corrected: gaps that were overclaimed in early drafts

Early drafts said Optimizer had no cost awareness and that nobody routes for offers. When the second model challenged this, I checked Razorpay's own docs. Optimizer does have manual cost rules, and some providers use card-range rules for offers. I narrowed the claim to what the evidence supports. The gap is the missing link between a merchant's agreements, its offers and its Optimizer rules, and that sharper framing is what the final product is built on.

### Significantly improved: the first three builds

AI-assisted implementation produced a working first build quickly, but it felt like a settings page, not an agent. I wrote 31 points of feedback, and after the second build still felt form-led, I required a conversation where Ray leads and decision cards appear inside it. I changed the interaction structure substantially to get there.

### Other critiques that changed the product

| What AI challenged | What I decided |
|---|---|
| "Automatic cost switching is risky" | Ray suggests and the merchant approves |
| "'Recovered revenue' isn't provable" | Reframed as protected deal value, counted only once settlement confirms it |
| "Eight views feel like a settings wizard" | Rebuilt as an agent-led conversation |
| "What if there's no tie-up, or two tie-ups conflict?" | Added optional tie-ups, three tie-up types, split rules and conflict handling |

---

## 4. What AI did versus what I decided or built

| AI did | I decided or built |
|---|---|
| Helped synthesise the public documentation and research supplied during the discussion | Chose the problem, the five research questions and where to look |
| Challenged the proposition across repeated critique rounds | Ruled on every objection, accepting most, reshaping some and overruling one |
| Drafted and tightened wording in the one-pager | Decided the content, structure and every claim |
| Assisted implementation and testing | Designed the flows, specified the 11 AI moments, branch logic and acceptance criteria, directed revisions and reviewed the resulting behaviour |
| Assisted browser walkthroughs and deterministic automated checks, reporting defects | Defined what "done" means and decided what to fix |
| Helped check the displayed financial examples | Reviewed the assumptions and decided how estimates should be presented |

### Product decisions that were mine

- Ray proposes and the merchant approves, because routing moves money.
- Protect deals first, and suggest cost improvements second.
- Make the smallest scoped change, so ordinary payments stay untouched.
- Simulate before applying anything.
- Mark a configuration Verified only after read-back matches approved fields. Read-back is simulated in this prototype; it does not verify financial recovery.
- Keep AI out of the payment path, since Optimizer already routes each payment.
- Keep AI away from the maths. Calculations, rule checks and version checks are deterministic.

### Primary research and experience

My PayPal pricing and revenue-operations experience informed the hypothesis that commercial terms and operational settings need to agree. I reported conversations with approximately 4–5 merchant teams in the PayPal context, alongside input from a payments practitioner and inspection of a Razorpay test account. These are qualitative inputs, not a representative Razorpay merchant study or a measured estimate of leakage frequency.

Practitioner input helped shape network-informed starter suggestions and the emphasis on merchant control. Public documentation and iterative critique informed the agreement-to-routing assurance workflow. The scope of primary input is kept separate from secondary research and prototype examples.


---

## 5. Known limitations, risks and verification steps

### Limitations

- **Scripted agent.** Ray's conversation is pre-scripted to show the intended experience. A production version would use a live language model.
- **Simulated Optimizer.** The connection to Razorpay Optimizer and the read-back are simulated. Approving a change in the demo doesn't alter any real account.
- **Fictional data.** Voltkart, Meridian Bank, Northstar, Indus PG and all their figures are sample data. Rupee figures are estimates, not realised benefit.
- **No automated reconciliation.** In a pilot, realised benefit would be checked against the merchant's own settlement records.

### Risks

- **Misread contract terms.** Mitigation: every term links to its source clause, and unclear terms trigger a question instead of a guess.
- **Volume commitments missed in practice.** A split like 84/16 is a forecast. Production would need to track actual monthly share, because traffic varies. The prototype checks a supplied traffic-share assumption; it does not ingest actual monthly payment records.
- **Stale or conflicting changes.** A stale approval blocks the write, and a read-back mismatch is never marked Verified.
- **Confidential contracts.** Merchant agreements are commercially sensitive. In production they would be accessible only to authorised users, kept no longer than needed, and never used for other merchants or for model training.
- **Optimizer capability gaps.** The prototype models campaign dates and an approval-led expiry review. Production would need to confirm supported Optimizer conditions and implement authorised scheduling where necessary; automatic publishing and retirement are not integrated here.

### Verification steps

- **Repeated browser walkthroughs and automated QA.** The final scenario picker contains 26 paths. The published automated suite records 175 passing checks, covering state transitions, calculations, parsing, contractual constraints and safe stops. Browser reviews identified issues including unreachable safety journeys, inverted exposure figures, static simulator results, conversation ordering and confusing completion states. These checks support the demonstration, not a production certification.
- **Safety states tested.** Concurrent edit, read-back mismatch and uncertain application each block the change or refuse Verified.
- **Every figure recalculated and checked** against the prototype:

| Figure | How it is calculated |
|---|---|
| ₹12,400 at risk | 1,240 festive EMI payments × 1 in 10 going to Indus PG × ₹100 assumed benefit each, using historical matching volume. Estimated routing exposure falls to ₹0 under a compatible tested option; this is not confirmed subsidy loss or recovery |
| ₹15.96 per payment | On a ₹10,000 order with ₹2,000 contribution, Northstar completes 97.8% at a ₹180 fee (₹1,779.96 expected) and Indus PG 98% at its standard ₹200 fee (₹1,764). With the volume deal, Indus PG's fee drops to ₹185 (₹1,778.70), so Northstar's advantage narrows to ₹1.26, illustrating why confirmed pricing terms need to be included before comparing cost |
| ₹7,980 and ₹15,960 | A cost-only comparison for a merchant with no Indus volume commitment, per 10,000 ordinary card payments starting from 90/10. Under the 15% floor, Ray would never propose these splits. Moving to 95/5 shifts 500 payments to Northstar, and moving to 100% shifts 1,000 |
| The 84/16 split | Campaign payments are 4% of card volume and must go to Northstar, so 16% of the remaining 96% gives Indus PG 15.36%, above its 15% floor |

- **Evidence boundaries.** Product discussions referred to Razorpay and competitor documentation. This build log does not substantiate a source-count claim or serve as a current regulatory/pricing reference. The repository’s architecture notes distinguish implemented behaviours from intended integrations.

---

The judgment is mine, and AI gave it range, speed and rigour.
