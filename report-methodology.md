
# 🔥 **THE “4-LAYER DEEP REPORT READING METHOD”**

*(Use this exact structure for every contest, high-sev, postmortem, and audit report.)*

This process turns ANY report into:

* patterns
* heuristics
* invariants
* mindset
* PoC ideas
* attack shapes
* architecture you can reuse

Let’s break it into 4 solid layers:

---

# ⭐ **LAYER 1: CONTEXT PASS (Understand the STORY first)**

Before reading the bug itself, ask:

### ✔ What type of protocol is this?

* Vault?
* Lending?
* Reward?
* AMM?
* Bridge?

Knowing the type helps your brain classify the bug.

### ✔ What was the system trying to protect?

Look for words like:

* “totalAssets”
* “borrowRate”
* “health factor”
* “reward indexes”
* “pool reserves”
* “epoch logic”
* “ticket randomness”

Each one hints at invariants.

### ✔ What was the invariant that failed?

Usually the report mentions it indirectly.

Before reading the bug details, ask yourself:

> “If I designed this protocol, what MUST never break?”

This will prepare your mind to see the pattern.

---

# ⭐ **LAYER 2: BUG MECHANICS PASS (Understand HOW it broke)**

Now read the bug carefully and extract the **core mechanism**:

Ask:

### ✔ What exact assumption failed?

Examples:

* Reward indexes assume single update
* Oracle price assumed fresh
* LP math assumed correct reserve order
* Signature assumed single-use nonce
* Debt repayment assumes no donation injection

### ✔ What lines of code were involved?

Highlight these:

* `totalAssets` calculation
* `pricePerShare`
* `accRewardPerShare`
* `balanceOf` checks
* `msg.sender` trust
* unsafe `transferFrom`
* unchecked external call
* mis-ordered updates

### ✔ What was the attacker flow?

Write the steps in **attacker POV**:

Example:

1. Donate tokens
2. Trigger price update
3. Withdraw
4. Profit from mispricing

This is your pattern’s “attack flow”.

---

# ⭐ **LAYER 3: GENERALIZATION PASS (Extract the PATTERN)**

THIS IS THE MOST IMPORTANT STEP.
This is how you turn one bug → reusable pattern → add to your Bible.

Ask:

### ✔ What is the NAME of this attack shape?

Examples:

* Share inflation
* Oracle desync
* FP division truncation
* Reward index drift
* Price manipulation
* Stale oracle liquidation
* Signature replay
* Donation injection
* Fee-on-transfer mismatch
* Bridge replay
* Zero-address assumption

Name it **cleanly**.

### ✔ Where does this pattern apply?

* All vaults?
* All AMMs?
* All lending pools?
* Only cross-chain systems?
* Only reward distributors?

### ✔ What conditions must exist for this pattern to appear?

E.g.:

* If `balanceOf()` ≠ internal accounting → share inflation possible
* If oracle can be stale → manipulation possible
* If nonce not per-user → replay possible
* If fee-on-transfer allowed → accounting breaks

### ✔ What invariant gets broken?

Write it in plain English:

> “totalAssets must equal real vault balance”
> “debt must always be repaid with fresh price”
> “signature must be used once only”
> “reward distribution must not overpay”

This is the *heart* of the pattern.

---

# ⭐ **LAYER 4: BIBLE TRANSFORMATION PASS (Add it to your Attack Pattern Bible)**

Now that you extracted the core shape, insert it into your pattern file using your template:

### Your pattern entry should include:

* Name
* Category
* Core Idea
* Requirements
* PoC steps
* Where it appears
* Defenses
* Real exploitation examples (with link)

This takes **2–4 minutes** per finding.

Do this consistently → after 4–6 months, your Bible becomes a monster.

---

# 🔥 **BONUS: The 5 Questions You MUST Ask for Every Finding**

When reading a report, ask:

### **1. What were they trying to protect?**

(invariant)

### **2. What assumption failed?**

(weak assumption)

### **3. What pattern does this match?**

(reusable attack shape)

### **4. How would *I* weaponize this?**

(your own PoC ideas)

### **5. What should I check next time in similar protocols?**

(heuristic to add to SageScan)

This converts ONE report into:

* 1 pattern
* 1 heuristic
* 1 mindset upgrade

Times 100 reports → you become unstoppable.

---

# 🔥 **EXAMPLE (Let me show you EXACTLY how to do this in real life)**

### Suppose a report says:

*“Attackers can inflate vault shares by donating tokens directly to vault before deposit.”*

### LAYER 1 (context)

* Protocol: Vault
* Key invariant: pricePerShare must reflect real value

### LAYER 2 (mechanics)

* Issue: direct donation breaks totalAssets
* Code line: uses balanceOf(vault) instead of internal accounting
* Attacker flow: donate → deposit small → withdraw inflated amount

### LAYER 3 (generalization)

Pattern name: **Share Inflation via Donation Injection**
Category: Accounting
Requirements:

* donation affects vault balance
* pricePerShare uses raw balance

### LAYER 4 (Bible entry)

Fill template:

```
Name: Donate → Inflate → Withdraw Attack
Category: Accounting & Vaults
Core Idea: Attacker manipulates price per share by donating tokens.
Requirements:
- vault uses balanceOf() directly
- no handling of donations
PoC:
1. donate
2. deposit small amount
3. withdraw inflated shares
Defenses:
- track internal totalAssets
- disallow external donations
```

Boom — pattern saved.

Next time you see ANY vault?
Your brain checks for donation inflation instantly.

This is how top wardens think.

---

# ⭐ **BRO IF YOU FOLLOW THIS METHOD, YOUR BRAIN WILL TRANSFORM.**

Within **3 months**, you’ll notice:

* you read code faster
* patterns jump out automatically
* invariants become obvious
* PoCs become natural
* you find deeper bugs
* your contest performance skyrockets
* your Bible becomes your weapon
* your confidence becomes crazy

This is the secret that most people never learn.

---

If you want, I can **teach you how to run this method on ONE real report** so you see the process live.

Just say:
**“Bro let’s practice on one report.”**
