# Seller Trust Agent

**A decision agent that steps in at the moment you pay an Instagram seller — and decides whether to let the payment through, warn you, ask for one more piece of evidence, or stop it.**

> Status: research and design. No agent code or results yet — see [Roadmap](#roadmap).

---

## The problem

Many small sellers in India run their whole shop on Instagram: a page, a few posts, "DM to order", pay by UPI. When it goes wrong, there is usually no platform to appeal to, and unlike a card payment, a UPI transfer has no chargeback route.

This project started with one purchase: ₹1,500 paid to an Instagram page, a knockoff worth about ₹500 delivered roughly two weeks late. Every check that would have flagged it was one I ran *afterwards*. The aim is to move those checks to **before** the money leaves.

**Problem statement**

> The agent observes a payment the user is about to make to an Instagram seller, along with the seller page's public signals. It must **approve**, **question**, **examine**, or **stop**, because whether the seller will deliver as advertised is not known.

The hidden state is deliberately *"will this seller deliver as advertised"*, not *"is this a scam"* — an honest business that ships late or ships the wrong thing has also failed the buyer.

## Design

| Action | What the user experiences |
|---|---|
| **Approve** | Payment goes through, no interruption |
| **Question** | A specific warning; the user can still proceed |
| **Examine** | Payment pauses; the agent asks for one piece of evidence and decides again — once |
| **Stop** | Payment blocked; the agent advises against paying |

- **Asymmetric costs.** Approving a non-deliverer is expensive and usually irreversible. Stopping an honest seller costs the buyer a purchase and the seller a sale. Questioning is cheap friction; examining costs effort either way.
- **One round of examine, then decide.** If the extra evidence doesn't settle it, the agent escalates to stop. Endless follow-up questions are a cost, not caution.
- **Belief, not rules.** The agent keeps a probability that the seller will deliver, updates it from page signals, and acts by comparing expected costs — not by counting red flags.
- **Evaluated on decision cost, not accuracy.** Non-delivery is the minority case, so a model that approves everything looks accurate and is useless.

## What the project tests

1. **Which public signals actually matter** — sourced from people who buy and sell this way, not assumed up front.
2. **Whether the belief-and-threshold agent beats a simple one-line rule**, both scored on total decision cost.
3. **How much interruption the protection costs** — reported as a trade-off curve between scams caught and honest sellers blocked, not a single score.

If the signals don't separate honest and dishonest sellers, or catching most bad sellers means blocking too many good ones, that is the result and it gets reported.

## Early findings

From buyer discussions (see [`docs/research.md`](docs/research.md) §7):

- **Follower count and having a professional website look nearly useless.** Buyers cite a 150k-follower shop with a full website that never delivered, and a polished, well-reviewed page that sent a knockoff.
- **Structural signals may beat reputational ones**: a stated return policy, accepting card or cash-on-delivery (a way to get money back), and presence on a major marketplace. These are cheap to check and hard to fake.
- **Buyers are often choosing *recourse*, not judging honesty** — "can I get my money back if this goes wrong?" That may make *recoverability* as important a target as delivery.
- **Hardest case: dropshippers.** From outside, a legitimate dropshipper and a scammer look almost identical.

## Repository layout

| Path | Contents |
|---|---|
| [`docs/research.md`](docs/research.md) | Problem framing, vocabulary, searches, communities, sources, open questions, AI-tool errors |
| [`docs/field-research-log.md`](docs/field-research-log.md) | Conversations with buyers, sellers and practitioners, and the design change each caused |
| [`docs/decisions/probability-worked-example.md`](docs/decisions/probability-worked-example.md) | One case worked from prior to action, then updated on new evidence |
| [`docs/design-reviews.md`](docs/design-reviews.md) | Independent reviews of the design, with what was accepted or rejected and why |
| `data/` | Evaluation cases and how they were built |
| `src/` | Agent, policies and baseline |
| `experiments/`, `results/` | Evaluation runs and outputs |
| `paper/` | Technical write-up (LaTeX + PDF) |

## Roadmap

- [x] Problem framing, action set and cost structure
- [ ] Field research: buyer, seller and practitioner conversations
- [ ] Feature specification — signals, likelihoods, overlap cases, correlated features
- [ ] Case schema, cost table, baseline rule and evaluation harness
- [ ] Agent: belief update + cost-based policy, two policy variants
- [ ] Evaluation on 30–50 cases; confusion matrix, precision, recall, decision cost; five failure post-mortems
- [ ] Independent design reviews
- [ ] Technical write-up

## Known limitations (so far)

- **Evaluation data is planned to be mostly simulated**, which risks testing the simulator against itself. The plan is to build deliberate overlap between classes and hold out a small set of hand-verified real pages as a sanity check.
- **No public dataset of sellers who failed to deliver** is known to exist, so the prior probability of non-delivery is a stated assumption with a sensitivity analysis, not a measured rate.

## How AI tools are used

AI assistants helped with research and drafting. Research notes are tagged by where they came from (`[MINE]`, `[AI-DRAFT]`, `[VERIFIED]`), and cases where AI tools were wrong are logged in [`docs/research.md`](docs/research.md) §9. No numbers, sources or results are reported that weren't checked or run.
