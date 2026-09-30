# Research notes

The working research behind the project: problem framing, vocabulary, searches, communities, sources, open questions, and where AI tools helped or misled.

**Provenance tags:** `[MINE]` I derived it · `[AI-DRAFT]` AI produced it, not yet checked · `[VERIFIED]` I opened it / read it myself.

---

## 1. Problem statement `[MINE]`

> **The agent observes a payment the user is about to make to an Instagram seller, along with the seller page's public signals. It must approve, question, stop, or examine because whether the seller will deliver as advertised is not known.**

**Scope.** Instagram seller pages only, at the moment of payment. Not websites, Facebook, or marketplaces. Not post-transaction dispute or recovery.

**Origin.** Real incident: ordered from an Instagram page, paid ₹1,500, received a knockoff worth roughly ₹500, about two weeks late.

**Hidden state.** Whether the seller will deliver as advertised. Deliberately framed as delivery-as-promised rather than "is this a scam" — a real business can still fail to deliver.

**Actions.**

| Action | Behaviour |
|---|---|
| Approve | Payment proceeds, no interruption |
| Question | Warning surfaced, user may proceed |
| Examine | Pause, request one specific piece of evidence, re-decide once |
| Stop | Block, advise against paying |

`examine` is capped at **one round**. If unresolved after that, escalate to `stop` — indecision is itself an answer, and every loop costs the user real effort.

**Costs (asymmetric).** Approving a non-deliverer is expensive and irreversible. Stopping a legitimate seller costs the user a purchase and the seller a sale. Questioning is cheap friction. Examining costs user effort regardless of outcome. Because the agent intervenes *at payment*, a false alarm is a blocked transaction, not just a second look.

> Superseded 2026-09-30: the earlier framing also covered linked websites and WhatsApp/Telegram chats and used `warn` instead of `question`. Section 9 below records AI prompts run against that earlier framing.

---

## 2. Project objective

> **To find out whether the public information on an Instagram seller page is enough to predict that a seller will not deliver — and whether an agent using that information can usefully step in at the moment of payment without blocking too many honest sellers.**

Three things tested:

1. **Which signals actually matter** — drawn from practitioner discussion, not decided in advance
2. **Whether the agent beats a simple rule** — belief-and-threshold policy vs. a one-line rule with no probability; both scored on decision cost, not accuracy
3. **How much interruption the accuracy is worth** — report the trade-off curve, not a single score

**On negative results.** If the signals don't separate the groups, or catching most scams means blocking too many honest sellers, that is the finding and it gets reported.

Carried over from the earlier framing: the project is not about detecting something humans can't see. **It moves a check people already know how to run from *after* the purchase to *before* it.**

> ⚠️ **OPEN — resolve before the preprint.** Bullet 1 claims to test whether signals predict non-delivery, but the data is simulated (see `HANDOFF.md` §5). Simulated data measures the simulator's own assumptions. Either narrow bullet 1 to "which signals practitioners report relying on, used as the basis for the simulation" and move the empirical claim to limitations, or add real held-out cases (5–10 hand-verified pages).

---

## 3. Technical terms `[AI-DRAFT]`

Candidates from two AI responses. **Confirm each against a real source before using it in the paper.**

### Decision problem

| Term | Plain meaning | Source that confirms it | Checked |
|---|---|---|---|
| Decision-making under uncertainty | Choosing an action without knowing the true state | | ☐ |
| Base rate / prior | How often purchases go wrong in general, before looking at this seller | | ☐ |
| Class imbalance | Bad sellers are a minority, so accuracy misleads | | ☐ |
| Cost-sensitive classification | Deciding when mistakes have unequal prices | | ☐ |
| Asymmetric information | Seller knows if they'll ship; buyer doesn't | | ☐ |
| Value of information (VoI) | Whether checking further is worth the delay — what `examine` buys | | ☐ |
| Selective labels problem | Blocked payments never produce an outcome, so the agent only learns from its own failures | | ☐ |
| POMDP | Formal frame for acting without seeing the full state | | ☐ |

> Note: POMDP is formally correct but the literature is heavy robotics maths. Probably cite once, don't build on it.

### Domain

| Term | Plain meaning | Source that confirms it | Checked |
|---|---|---|---|
| Social commerce | Buying directly through social platforms | | ☐ |
| Non-delivery fraud / purchase scam | Paid, nothing arrived | | ☐ |
| Not-as-described (SNAD) | Marketplace term for "arrived, but wrong" — my box 2 has a long paper trail under this name | | ☐ |
| Trust signals | Reviews, page age, return policy — proxies buyers use (follower count: see §7, thread says it means nothing) | | ☐ |
| Seller reputation systems | How marketplaces solved this with history. Instagram has no purchase-linked reputation — that gap is the problem | | ☐ |
| Market for lemons | Akerlof's asymmetric-information classic | | ☐ |
| Warning fatigue / habituation | People stop reading warnings they see too often | | ☐ |
| DM-to-order | Seller takes orders in Instagram DMs rather than a checkout | | ☐ |
| UPI / payment handle (VPA) | India's instant bank-transfer rail; a VPA is the `name@bank` address paid to | | ☐ |
| COD | Cash on delivery — buyer pays only when the parcel arrives | | ☐ |
| Chargeback | Card-network route to reverse a payment; UPI has no direct equivalent | | ☐ |
| Advance-payment scam | Seller takes payment up front, then never ships | | ☐ |
| Knockoff / replica | A copy sold as, or delivered instead of, the advertised item | | ☐ |
| Dropshipping | Seller holds no stock; a supplier ships direct. **From outside, a legitimate dropshipper looks nearly identical to a scammer** (long delivery, no inventory, stock photos, weak recourse). Expect this in review | | ☐ |
| Engagement pods | Groups that like/comment on each other's posts to inflate engagement | | ☐ |
| Bought followers / bot comments | Purchased audience or fake comments that fake social proof | | ☐ |
| Comment moderation | Seller hides or disables comments — can hide complaints | | ☐ |
| Account age | How long the page has existed | | ☐ |
| Verified badge | Platform's blue tick; now purchasable, so check what it actually proves | | ☐ |
| Cybercrime reporting (India) | Helpline 1930 / cybercrime.gov.in | | ☐ |

---

## 4. Search queries `[AI-DRAFT]`

| # | Query | Tried | What it turned up |
|---|---|---|---|
| 1 | `social commerce fraud detection India` | ☐ | |
| 2 | `Instagram non-delivery scam study` | ☐ | |
| 3 | `"significantly not as described" dispute rate marketplace` | ☐ | |
| 4 | `seller reputation system cold start trust` | ☐ | |
| 5 | `cost-sensitive fraud classification asymmetric costs` | ☐ | |
| 6 | `"selective labels" fraud machine learning` | ☐ | |
| 7 | `warning fatigue security prompts habituation` | ☐ | |
| 8 | `UPI merchant fraud consumer protection RBI` | ☐ | |
| 9 | `dark patterns checkout urgency Instagram sellers` | ☐ | |
| 10 | `"Partially Observable Markov Decision Process" AND "fraud detection"` | ☐ | |
| 11 | `dataset sellers non-delivery social commerce labelled` — check whether a labelled non-delivery dataset exists (as distinct from fake/bot account datasets) | ☐ | |
| 12 | `India online shopping fraud statistics non-delivery rate` — check whether any figure decomposes to a per-page non-delivery rate (needed for the prior) | ☐ | |

---

## 5. Reddit communities — CANDIDATES, none verified yet

Target: **five to ten verified** communities. Nothing below is verified yet. **Open each one, check it is alive and on topic, and read its rules before posting.** Delete any that fail.

The list is deliberately spread across three groups, because my open questions need three different kinds of person.

### Group A — buyers and victims (for the base rate, and real case patterns)

| Candidate | Why relevant | Alive? | Rules allow my post? | Verdict |
|---|---|---|---|---|
| r/Scams | Threads of Instagram non-delivery stories; grounds my case labels in real reported patterns | ☐ | ☐ | |
| r/india | Indian buyers discussing exactly this scam | ☐ | ☐ | |
| r/IndiaSocial | Same, more consumer-focused | ☐ | ☐ | |
| r/personalfinanceindia | Payment recovery and chargeback reality in India | ☐ | ☐ | |
| r/InstagramShops | ~15k members. Buyer-vs-small-store trust thread `1w5kv79` already read (see §7) | ☐ | ☐ | |

### Group B — sellers (for the false-block cost — nobody else can answer this)

| Candidate | Why relevant | Alive? | Rules allow my post? | Verdict |
|---|---|---|---|---|
| r/IndianEntrepreneur | Legitimate Instagram sellers. Ask what a lost checkout costs them | ☐ | ☐ | |
| r/smallbusiness | Same question, wider pool | ☐ | ☐ | |
| r/shopify or r/Etsy | Sellers on the receiving end of fraud holds | ☐ | ☐ | |

### Group C — builders (for deployment, legality, and design feedback)

| Candidate | Why relevant | Alive? | Rules allow my post? | Verdict |
|---|---|---|---|---|
| r/developersIndia | Can an app see an Instagram page and a pending payment on Android in India? Is it legal? | ☐ | ☐ | |
| r/androiddev | Accessibility services, overlays, what is technically possible | ☐ | ☐ | |
| r/learnmachinelearning | Design feedback on the belief and cost model, beginner-tolerant | ☐ | ☐ | |

### Flagged as doubtful — check before trusting

| Candidate | Concern |
|---|---|
| r/NLP | May be **Neuro-Linguistic Programming**, not natural language processing. The active NLP community may be r/LanguageTechnology |
| r/FraudDetection | May not exist as an active subreddit |
| r/MachineLearning | Real, but heavily moderated and often hostile to beginner design questions. Read the rules carefully |

---

## 6. X accounts — CANDIDATES, none verified yet

Target: 15–25 accounts — deliberately **not only popular AI accounts**: researchers, engineers, **users**, and **critics**.

**Verify every handle resolves to the person named.** Handles are the single most common thing AI gets wrong.

| Candidate | Why relevant | Handle resolves? | Actually posts on topic? | Verdict |
|---|---|---|---|---|
| Patrick McKenzie (patio11) | Payments, fraud, trust systems. Directly relevant to the approve/stop asymmetry | ☐ | ☐ | |
| Rachel Tobac | Social engineering — how scammers manipulate people mid-purchase | ☐ | ☐ | |
| Troy Hunt | Scam ecosystems, consumer security | ☐ | ☐ | |

**Better method than any list:** search X directly for `instagram scam`, `UPI fraud`, `trust and safety`, `social commerce India`, `D2C India`, and follow whoever posts substantive threads — fraud analysts at payment companies, Indian fintech engineers, consumer-rights critics, scam-documenting accounts. Everyone found this way is verified by construction.

Still needed: **users and critics**, not just professionals.

---

## 7. Sources — five needed, none read yet

**Rule: do not cite anything I have not read.**

Starting points surfaced during research, all **unread**:

| # | Source | Type | Why it might matter | Read | What I took from it |
|---|---|---|---|---|---|
| 0 | [r/InstagramShops thread `1w5kv79` — "Buyers: What stops you from buying from small Instagram boutique stores vs. established apps like Myntra?"](https://www.reddit.com/r/InstagramShops/comments/1w5kv79/) | forum thread (21 comments) | **Read, not participated in.** Buyers name the signals they use and kill two | ☑ | Kept: return/refund policy (most repeated), payment method (card/COD = chargeback route; UPI = none), real own photos, behind-the-scenes content, reviews in highlights, Google reviews, aggregator presence (Myntra/Ajio), pre-purchase DM tone. **Killed:** follower count, professional website. Counterexamples: 150k-follower shop with full website that scammed; branded, well-reviewed page that sent a knockoff. **Reframe:** buyers choose *recourse* ("can I get my money back") more than they detect dishonesty — may mean the agent predicts recoverability as much as delivery. Caveats: one thread, one sub, different question, self-selected toward people who were burned |
| 1 | [Shopify community — Instagram/Facebook as a platform for scammers](https://community.shopify.com/t/shopify-instagram-and-fasebook-as-a-platform-for-scammers/374119) | forum thread | Documents the storefront scam pattern; possible source of real case material | ☐ | |
| 2 | [Shopify community — one victim's case, "Freakin"](https://community.shopify.com/t/got-scammed-by-the-website-freakin/394027) | forum thread | A worked example of the exact scam shape | ☐ | |
| 3 | [Gulf News — Beware of shopping scams on Instagram](https://gulfnews.com/lifestyle/beware-of-shopping-scams-on-instagram-1.2251572) | news | General framing; check whether it cites any actual figures | ☐ | |
| 4 | [Gulf News — study claiming 16M fake Indian influencer accounts](https://gulfnews.com/world/asia/india/16-million-accounts-of-indian-instagram-influencers-fake-study-1.65208654) | news reporting a study | **Counter-evidence for my own design** — supports weighting follower count low. Trace the underlying study before citing | ☐ | |
| 5 | | | Still needed — ideally one peer-reviewed paper on social commerce trust or cost-sensitive decisions | ☐ | |

> Row 0 is the only source read so far. Forum threads and news are weak citations. At least one or two of the five should be a real paper or dataset.

---

## 8. Questions I want answered `[MINE]`

### Owed to humans — will not invent these

| # | Question | Who can answer | Asked where | Answer |
|---|---|---|---|---|
| 1 | What does a wrongly blocked sale actually cost a small Instagram seller? | sellers (Group B) | | |
| 2 | Roughly what fraction of Instagram seller purchases go wrong? | victims + platforms (Group A) | | |
| 3 | How quickly does warning fatigue set in — after how many prompts do people stop reading? | research + practitioners | | |
| 4 | Can an app realistically see an Instagram page *and* a pending payment on an Indian phone? Is it legal? | builders (Group C) | | |

### Design questions

**Hidden states**
- Is the hidden state the seller's *intention* or the eventual *outcome*? A courier can lose an honest seller's parcel. Which one can my evidence actually speak to?
- An honest-but-overwhelmed seller and a scammer both go silent after payment. What, observable *before* payment, separates them?

**Evidence**
- Which signal moves belief most per second of checking — page age, follower ratio, comments disabled, or no return policy?
- Which of my signals are cheap to fake and which are expensive? Expensive-to-fake signals should carry the weight.
- **Is "DM or WhatsApp to order" evidence of a scam, or just how small Indian sellers normally operate?** Ask sellers. (The agent no longer reads the chat — only whether the page asks buyers to order off-checkout.)
- **Is the agent predicting delivery or recoverability?** The r/InstagramShops thread suggests buyers care most about getting money back. Decide deliberately.
- **How does the agent tell a legitimate dropshipper from a scammer?** From outside they look nearly identical.

**Actions**
- What exactly does `examine` fetch, and how long does the buyer wait?
- After a `question`, what does the buyer see — a specific reason, or generic caution?
- What does `examine` ask for in its one round, and what happens if the answer doesn't resolve it? (Current rule: escalate to `stop`.)

**Errors**
- What evidence would change my ~3-blocked-sellers-per-buyer-saved tolerance?
- After how many false warnings does a buyer stop reading them?

---

## 9. AI prompts used, and important AI errors

### Prompt

> Run against the **earlier** (2026-08-22) problem framing. Re-run against the current §1 statement before relying on the answers.

The §4 research prompt, with my problem statement substituted. Run against **Gemini Spark** and **Claude**, deliberately, to compare.

```text
I am a beginner. I want to design an AI agent for this problem:

  The agent observes a seller's Instagram page and wherever it leads — a
  website, a WhatsApp or Telegram chat — together with a pending payment.
  It must select approve, warn, examine, or stop because whether the buyer
  will receive the product as promised by the seller is not known.

The agent must make decisions when information is not complete.

Help me prepare my research.

1. Give me the technical terms for this problem.
2. Give me useful search queries.
3. Find 5 to 10 relevant Reddit communities.
4. Tell me why each community is relevant.
5. Find relevant researchers and engineers on X.
6. Give me questions about hidden states, evidence, actions, and errors.
7. Identify each claim that needs a source or a test.
8. Tell me which parts of my problem are not clear.

Do not present uncertain information as fact.
```

### Errors and disagreements `[MINE]` — my assessment

| # | Tool | What it produced | Problem | How I know / how to check |
|---|---|---|---|---|
| 1 | Gemini | Reddit list: MachineLearning, AI_Agents, datascience, learnmachinelearning, Cybersecurity, FraudDetection, NLP, ComputerVision | **Every community is technical.** Not one buyer, seller, or Indian community. My four open questions are about what a blocked sale costs a seller and how often purchases go wrong — nobody on r/ComputerVision can answer either | Compared the list against my own open questions and found no overlap |
| 2 | Gemini | r/NLP recommended for analysing Instagram captions | May be **Neuro-Linguistic Programming**, not natural language processing | ☐ open it and check |
| 3 | Gemini | r/FraudDetection | May not exist as an active subreddit | ☐ open it and check |
| 4 | Gemini | X accounts: LeCun, Anandkumar, Levine, Frosst, Choi | Famous ML researchers only. None work on social commerce or Indian payments, and none is likely to engage. The people who can actually answer my questions — **users and critics** — are entirely absent | Compared the list against who can answer my open questions |
| 5 | Gemini | Framed the problem as multimodal ML: knowledge graphs, GNNs, reinforcement learning, CV for receipt forgery | Frames this as an ML research system. I am building a decision on a spreadsheet. Also invented a component I never mentioned — payment-receipt screenshots are not in my problem at all | My problem statement says nothing about receipts |
| 6 | Gemini | Omitted base rate, class imbalance, cost-sensitivity | The core of any cost-sensitive decision problem, absent from a list of "technical terms for this problem" | Compared against standard decision-theory framing |
| 7 | Claude | Gave three X handles and declined to produce a full list | Honest about uncertainty, but leaves me well short of 15–25. Have to do the search myself | Target in §6 |
| 8 | Both | Neither flagged that `examine` may be infeasible if the decision is time-critical | Gemini raised time horizon as *unclear*; neither connected it to `examine` being unusable at 2 seconds | Noticed while reading both |

### Where both tools agreed — probably real gaps

- **Whose agent is it?** Buyer's advisor, platform gatekeeper, or payment-app filter. `stop` means something very different for an advisor than a gatekeeper. Biggest unresolved question.
- **Time horizon** — seconds at checkout, or can the payment stay pending?
- **Data access boundaries** — can the agent read the WhatsApp chat, or only know that a redirect happened? *(Settled 2026-09-30: public page signals only.)*

### Claims flagged as needing a source or test

| Claim I am currently relying on | How to check |
|---|---|
| Page signals predict delivery outcomes | The foundational claim. Test on labelled cases; ask victims whether the signs were visible in hindsight |
| Off-platform redirect raises risk | Ask legitimate sellers how often *they* close on WhatsApp. If most do, the signal is weak |
| A warning at payment time changes behaviour | Warning-fatigue literature suggests most warnings are ignored |
| ~3:1 tolerance | `ASSUMPTION`, my own judgement. Revise after asking sellers |
| Follower counts are trustworthy | Counter-evidence already found — see source 4 |
