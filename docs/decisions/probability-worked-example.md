# Probability worked example

One case where the true state is unknown, worked through from prior to action — then re-worked after one new piece of evidence arrives.

---

## Case

**ID:**
**Question:**

### Components

| Item | Required information | Value |
|---|---|---|
| Evidence | Information the agent observed | |
| Hidden states | Possible explanations | |
| Beliefs | Probability of each hidden state | |
| Event | Hidden states that are important to the user | |
| Actions | Available actions | |
| Costs | Results of correct and incorrect actions | |
| Policy | Decision rule and threshold | |
| Decision | Selected action and reason | |
| Audit data | Time, data version, model version, policy version | |

### Beliefs

The sum of all hidden-state probabilities must be 100 percent.

| Hidden state | Belief | Reasoning |
|---|---|---|
| | | |
| **Total** | **100%** | |

---

## The update — one new item of evidence

Six steps.

### 1. State the prior probability

State the reference class explicitly: which group, which dates, how many cases, who verified them, and why this group resembles the case in front of me.

| Hidden state | Prior | Source |
|---|---|---|
| | | |

### 2. State the new evidence

### 3. Estimate the likelihood for each important hidden state

*How often does this clue appear inside each story's pile?*

| Hidden state | P(evidence \| state) | Source or assumption |
|---|---|---|
| | | |

### 4. Calculate or simulate the posterior probability

```
support(state) = prior(state) × likelihood(state)
belief(state)  = support(state) ÷ sum of support over all states
```

Every route to the observed clue goes on the bottom.

| Hidden state | Support | Posterior |
|---|---|---|
| | | |

### 5. Compare the posterior with the decision threshold

### 6. Record the new action

**Action:**
**Changed from:**
**Reason:**

---

## Evidence quality notes

Rule: use recent and comparable historical cases. Do not use a large data group only because it is easy to find. Search for evidence for the safe state and the unsafe state.

| Check | Notes |
|---|---|
| Is the reference class recent, comparable, large enough? | |
| Is it selected in a way that biases it? | |
| Did I look for evidence supporting the safe state as hard as the unsafe one? | |
| What story is not on the board at all? | |
