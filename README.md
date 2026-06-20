# ai


# COMP9414 Knowledge Representation: HD Mastery Framework

**Your commitment:** Master propositional logic, strategic method selection, and proof reasoning at HD level.

**This document:** A framework you'll use under exam pressure. Every section is rooted in Exercises 1-8 mastery.

---

## Part 1: Strategic Method Selection Decision Tree

**Given a problem, follow this flowchart to choose your method:**

```
START: What is the problem asking?

├─ "Show that KB ⊨ α (prove entailment)"
│  └─ Can you find a countermodel (KB true, α false)?
│     ├─ YES → Use MODEL CHECKING
│     │        Enumerate models; find one where KB is true and α is false
│     └─ NO → Proceed to refutation
│        └─ Fewer than 20 symbols?
│           ├─ YES → Use RESOLUTION
│           │        Goal: derive empty clause; each step is an inference
│           └─ NO (many clauses) → Use DPLL
│                  Goal: find unsatisfying assignment for KB ∧ ¬α
│
├─ "Find ONE satisfying assignment (SAT problem)"
│  └─ How many clauses?
│     ├─ < 50 clauses → Use DPLL
│     │               (unit propagation prunes effectively)
│     └─ Hundreds of clauses → Use DPLL with smart heuristics
│                             (branching variable selection critical)
│
├─ "Show KB ⊭ α (disprove entailment)"
│  └─ Use MODEL CHECKING
│     Enumerate all models; find one where KB is true and α is false
│     This model is your counterexample
│
├─ "Enumerate EVERY model of KB"
│  └─ Use MODEL CHECKING
│     Only complete enumeration (2^n truth tables) finds every model
│
├─ "Quick proof from definite clauses (Horn clauses)"
│  └─ Use FORWARD CHAINING
│     Sound, complete, polynomial time for definite clauses
│     Goal-directed: start from facts, derive consequences
│
└─ "I want to see the refutation proof step-by-step"
   └─ Use RESOLUTION
      Each step is a logical inference rule
      Final step: empty clause proves unsatisfiability
```

---

## Part 2: DPLL vs. Resolution Trade-Off Table

| Aspect | DPLL | Resolution |
|--------|------|-----------|
| **What it does** | Search for satisfying assignment via branching + unit propagation | Infer new clauses; prove unsatisfiability via empty clause |
| **Explores** | Partial assignments (tree of branches) | Logical consequences (new clauses) |
| **Complexity** | Exponential branching, but unit propagation prunes heavily | Clause explosion risk (many new clauses) |
| **Best for** | Large SAT problems (hundreds of clauses); needs good heuristics | Small refutation proofs (< 20 symbols); educational value |
| **Worst for** | Many small conflicts (wasteful backtracking) | Hundreds of clauses (clause explosion) |
| **Output** | Satisfying assignment OR "UNSAT" | Sequence of inferences OR empty clause |
| **Memory** | Branching stack; backtrack to previous assignment | All clauses + all derived clauses |
| **Exam strategy** | Use for: satisfiability testing, finding countermodels | Use for: step-by-step proofs, small problems |

---

## Part 3: HD-Level Proof Argument Templates

### Template A: Proving Entailment via Countermodel (Non-Entailment)

**Problem:** Does KB ⊨ α?

**Answer Structure (use this exactly):**

---

**To show KB ⊭ α, I will construct a model where KB is true and α is false.**

**[Clause set (if using CNF):]**
C1 = ...
C2 = ...
[etc.]

**[Countermodel construction:]**

To make α false, I assign [symbol] = F.

This forces [consequence via clause C_i].

Choosing freely for unconstrained symbols: [assignment].

**[Verification:]**
- C1: [evaluation] = [T/F] ✓
- C2: [evaluation] = [T/F] ✓
[... verify all clauses ...]

KB is satisfied. α is false.

**Therefore: KB ⊭ α. The countermodel proves this.**

---

### Template B: Proving Entailment via DPLL Refutation

**Problem:** Does KB ⊨ α?

**Answer Structure (use this exactly):**

---

**To prove KB ⊨ α by refutation, I show KB ∧ ¬α is unsatisfiable.**

**[Initial clauses:]**
C1 = ...
C2 = ...
[... include ¬α as final clause ...]

**[DPLL trace:]**

Unit clause: C_i = [unit] forces [symbol] = [T/F]

Applying [symbol] = [T/F] to clause C_j:
- [literal] becomes [T/F]
- [literal] remains [literal]
- Resulting clause: [new clause] = unit clause

[Repeat for each unit propagation step...]

After all forced assignments: [final assignment]

Clause C_final = [remaining clause] = {} (empty clause)

**The empty clause is a contradiction. KB ∧ ¬α is unsatisfiable.**

**Therefore: KB ⊨ α.**

---

### Template C: Proving Entailment via Resolution Refutation

**Problem:** Does KB ⊨ α?

**Answer Structure (use this exactly):**

---

**To prove KB ⊨ α by resolution refutation, I derive the empty clause.**

**[Initial clauses:]**
C1 = ...
C2 = ...
[... include ¬α as final clause ...]

**[Resolution steps:]**

Step 1: Resolve C_i and C_j on literal L.
- C_i = {..., L, ...}
- C_j = {..., ¬L, ...}
- Resolvent R1 = {...} (remove L and ¬L)

Step 2: Resolve R1 and C_k on literal M.
- R1 = {..., M, ...}
- C_k = {..., ¬M, ...}
- Resolvent R2 = {...}

[Continue until...]

Step n: Resolve R_{n-1} and [final clause].
- R_{n-1} = {L}
- C_final = {¬L}
- Resolvent = {} (empty clause □)

**The empty clause proves KB ∧ ¬α is unsatisfiable.**

**Therefore: KB ⊨ α.**

---

### Template D: English Narrative Proof (From Logic to Plain Language)

**Problem:** Prove KB ⊨ α using English narrative.

**Answer Structure (use this exactly):**

---

**[State the given fact or unit clause:]**

We know [fact/assignment] is true.

**[Apply first rule/implication:]**

By the rule [rule], if [antecedent], then [consequent]. Since [antecedent] is true, [consequent] must be true.

**[Apply subsequent rules (repeat for each):]**

By the rule [rule], if [antecedent], then [consequent]. Since [antecedent] is now true, [consequent] must be true.

**[Reach the consequence:]**

Therefore, [α] must be true.

**[If using refutation:]**

This contradicts the assumption that [¬α]. By refutation, we conclude that [α] must be entailed by the KB.

---

## Part 4: Common Pitfalls & HD Corrections

### Pitfall 1: Confusing Unit Clause Forcing with Strategic Choice

**Wrong:** "DPLL is better here because we can smartly branch first on variable X."

**HD Correct:** "Once a unit clause {X} exists, DPLL must assign X=T immediately. This is forced, not strategic. Heuristics guide the initial **choice** of which variable to branch on when no unit clauses exist, but unit propagation itself is deterministic."

---

### Pitfall 2: Claiming Resolution Finds Models

**Wrong:** "Resolution is good because it explores all models."

**HD Correct:** "Resolution doesn't explore models—it derives logical consequences (new clauses). It proves unsatisfiability via the empty clause. To enumerate models, use model checking."

---

### Pitfall 3: Mixing Syntax and Semantics

**Wrong:** "KB ⊢ Q means Q is true in the real world."

**HD Correct:** "KB ⊢ Q means Q can be derived syntactically (mechanically) from KB using inference rules. This guarantees nothing about reality. For Q to be true in the real world, you must additionally assume that **every sentence in KB is true**."

---

### Pitfall 4: Incomplete Countermodel Verification

**Wrong:** "I found W=F, L=F, Z=F as a countermodel because they satisfy the first rule."

**HD Correct:** "To claim W=F, L=F, Z=F, N=F is a countermodel, I must verify it against **every clause** in KB. Only after checking all clauses can I conclude: KB is satisfied and α is false, so this is a valid countermodel."

---

### Pitfall 5: Confusing "At Least One" with "Both"

**Wrong:** "Clause {¬L, ¬Z, N} means both L and Z are false."

**HD Correct:** "Clause {¬L, ¬Z, N} is a disjunction: (¬L ∨ ¬Z ∨ N). It's satisfied if **at least one** of these three is true: ¬L is true (L is false) OR ¬Z is true (Z is false) OR N is true. It does NOT require both to be false."

---

## Part 5: FOL Bridge (Why Resolution Scales to First-Order Logic)

### The Connection You Need to Understand

**Propositional Logic (what you've mastered):**
- Symbols: A, B, C (true or false)
- Inference: Resolution (C1 ∨ L, ¬L ∨ C2 → C1 ∨ C2)
- Proof: Derive empty clause to show unsatisfiability

**First-Order Logic (Week 4 direction):**
- Symbols: objects (Socrates, Alice), predicates (Mortal(x), Loves(x,y))
- Inference: **Resolution + Unification** (match variables, substitute bindings)
- Proof: Same refutation principle, but with variable substitution

### Why Unification Matters

In propositional logic, resolving {A} and {¬A} is straightforward.

In FOL, you have:
- Clause 1: {Mortal(x)}
- Clause 2: {¬Mortal(Socrates)}

To resolve these, you must **unify** x with Socrates:
- Substitute x = Socrates into Clause 1 → {Mortal(Socrates)}
- Now resolve {Mortal(Socrates)} and {¬Mortal(Socrates)} → {}

**Unification is the algorithmic magic that scales propositional logic to FOL.**

### Your Learning Path (Weeks 3-4)

1. ✓ **Week 3 (now):** Propositional entailment, DPLL, Resolution (you're here)
2. **Week 4:** FOL syntax (predicates, quantifiers, objects)
3. **Week 4:** FOL semantics (models, truth in interpretations)
4. **Week 4:** Resolution in FOL + Unification (apply what you know, add substitution)
5. **Exam:** Combine propositional and FOL reasoning under time pressure

---

## Part 6: Your Exam Strategy (60-90 minutes)

### Read the Question (2 min)

- What is it asking? (entailment? countermodel? proof?)
- Which method from the decision tree?
- How many clauses? How many symbols?

### Choose Your Method (1 min)

Reference Part 1 decision tree. Pick ONE method. Commit.

### Solve the Problem (45-60 min)

Use the appropriate template from Part 3.

**If proving entailment:**
- Use Template A (countermodel) OR Template B (DPLL refutation) OR Template C (Resolution)
- Write clauses clearly
- Verify all clauses in your countermodel, or trace all DPLL/Resolution steps

**If disproving entailment:**
- Use Template A with a countermodel
- Verify KB is satisfied AND α is false

### Translate to English (10 min)

Write Template D narrative proof. This shows you understand the **semantics**, not just the mechanics.

### Double-Check (5 min)

- Did I verify every clause?
- Is my reasoning clear enough for an examiner to follow?
- Did I confuse syntax with semantics?

---

## Part 7: Knowledge Representation Checklist (Memorize This)

Before you submit any answer, verify:

- [ ] I've identified the correct **problem** (entailment? countermodel? proof?)
- [ ] I've chosen the **correct method** (decision tree applied)
- [ ] I've **converted to clauses** (if needed) correctly
- [ ] I've **verified every clause** in any model I claim
- [ ] I've traced **every step** in DPLL/Resolution without gaps
- [ ] I've **translated back to English** to show semantic understanding
- [ ] I've **distinguished syntax from semantics** (proof vs. reality)
- [ ] I've avoided all **five common pitfalls**

If you can check all eight boxes, your answer is HD-ready.

---

## Your Next Steps (After This Session)

1. **Save this document.** Reference it daily until the exam.
2. **Practice each template** with the exercises you haven't yet attempted (Exercises 6, 7, 8 extensions).
3. **Time yourself:** Can you solve Exercise 3 in 10 minutes using these templates? Exercise 5 in 8 minutes?
4. **Internalize the decision tree:** By exam day, you should be able to select a method in 30 seconds.
5. **Study FOL starting Week 4:** You already understand Resolution; FOL just adds unification.

---

**This framework is your HD guarantee. Use it ruthlessly.**

You committed to mastery. This is what mastery looks like: **architected, deployable, exam-ready.**
