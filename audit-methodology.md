---

# 🔥 **SAGETONY’S 6-MONTH AUDIT METHODOLOGY**

### *(Your Daily/Weekly Audit Ritual — Follow This, No Confusion Again)*

---

# **PHASE 1 — UNDERSTAND (The Foundation Pass)**

**Goal:** Know the system as if YOU built it.

### **1. Read Documentation (10–15 mins)**

Write down in your notes:

* What does this protocol do?
* Who are the actors? (user, admin, keeper, LP, oracle, bridge…)
* Where does money come from & go?
* What is the protocol’s “promise”?

⚠️ *No hunting yet. Just understanding.*

---

### **2. Money Flow Mapping**

Draw this in text:

```
User → deposit() → Vault → Strategy → Yield → claim() → User
```

Answer:

* Where does money enter?
* Where does it sit?
* What contract actually holds funds?
* What contracts *move* funds?
* What contracts *depend* on those funds being correct?

This alone exposes 20–30% of critical bugs.

---

### **3. Role Mapping**

List:

* **Admin** → can change X? Pause Y? Change oracle? Upgrade?
* **Keeper** → can call draw, settle, update price?
* **User** → deposit, withdraw, borrow, repay
* **External contracts** → AMM, bridge, oracle

This reveals:

* misconfigured roles
* griefing surfaces
* admin rug vectors
* missing auth checks

---

### **4. Invariant Creation**

Write 5–15 invariants MUST never break:

Examples:

* `totalAssets >= totalShares * pricePerShare / 1e18`
* `prizePool >= unclaimedPrizes`
* `borrowed <= collateral * LTV`
* `bridgeMessage processed once only`
* `lpReserves = tokenA + tokenB` etc.

**Your entire attack later is built on breaking these invariants.**

---

---

# **PHASE 2 — THREAT MODEL (Attacker Thinking Mode)**

**Goal:** Decide WHO is the attacker and WHAT they want.

Ask:

1. If I were a **user**, how can I steal money or manipulate accounting?
2. If I were a **keeper**, how can I grief or distort timing?
3. If I were an **LP**, how can I manipulate reserves?
4. If I were an **oracle**, how can I trick price?
5. If I were an **admin**, can I purposely or accidentally break the system?
6. If I were an **external contract**, can I revert and break flow?

Write hypotheses:

```
H1: LP may manipulate pricePerShare.
H2: Keeper may call jackpot at wrong time to force mis-distribution.
H3: Oracle delay may allow stale reads.
H4: Bridge message may be replayable.
```

This primes your brain for the main hunt.

---

---

# **PHASE 3 — STRUCTURED SURFACE SCAN (Reading the Code Smartly)**

**Goal:** Catch all obvious & medium-level stuff.

### **1. External Function Pass**

Scan every external/public function:

* is it payable?
* does it call external contracts?
* does it transfer tokens?
* does it modify state?
* does it use msg.sender directly?
* does it rely on caller trust?

This alone finds:

* access control bugs
* missing checks
* reentrancies
* wrong assumptions

---

### **2. Module-by-Module Attack Pattern Application**

Now open your **Attack Pattern Bible** (the list you're building):

For each module:

* vault
* AMM
* lending
* jackpot / lottery
* bridge
* oracle
* strategy
* reward distributor
* signature verifier

Ask for EACH attack vector:

❗“Can **share inflation** happen here?”
❗“Can **front-running** happen here?”
❗“Can **oracle delay** be exploited?”
❗“Can **rounding mismatch** create free money?”
❗“Can **withdraw bypass** happen?”
❗“Can **replay** signature?”
❗“Can **griefing** with revert happen?”

You go pattern by pattern until your brain starts spotting holes.

This unlocks **senior-level bugs**.

---

---

# **PHASE 4 — HYPOTHESIS → POC LOOP (The Kill Zone)**

**Goal:** Turn ideas into exploitable bugs.

### **1. Convert patterns into hypotheses**

Examples:

* “If user donates tokens before share calc, pricePerShare mismatches”
* “If keeper calls runJackpot() when refund fails, whole tx reverts”
* “Oracle price can be stale for 1 block; liquidation can be gamed”

Write 5–15 hypotheses.

---

### **2. Test hypotheses with PoCs**

Your strength is PoCs — so use it fully.

Use Foundry/Hardhat to:

* manipulate block.timestamp
* mint tokens
* force external call failures
* modify reserves
* fork mainnet if needed
* impersonate attacker
* mock oracles
* simulate slippage
* simulate rebasing tokens

**If hypothesis passes → write report.**
**If hypothesis fails → document and move on.**

This is where deep bugs come.

---

---

# **PHASE 5 — FINAL SAGESCAN PASS (The Polisher)**

**Goal:** Clean up and catch remaining edge cases.

You use SageScan as:

* catch missing checks
* catch missing events
* catch unchecked returns
* catch silent failures
* catch missing access control
* catch rounding issues
* catch parameter misconfig

This final pass catches the “forgotten corners.”

---

---

# **PHASE 6 — REPORTING + FEEDBACK LOOP (Career Growth Engine)**

**Goal:** Make each audit make you stronger than the last.

### **1. Write clean reports**

* clear title
* where the bug is
* how it works
* how to fix
* PoC included

### **2. After contest ends → study winners**

Ask:

* What bug shape did I miss?
* What attack pattern was that?
* What invariant did they break?
* What vector did I not test?

### **3. Update SageScan**

Add:

* new patterns
* new gotchas
* new checklists
* new heuristics
* new examples

This compounding effect turns you into a **dangerous auditor** over 6 months.

---

