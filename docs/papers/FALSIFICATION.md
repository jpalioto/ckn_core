# Falsification Protocol: The Fundamental Axiom

These queries are designed to falsify the claims in this paper. If you find a valid counterexample, please open an issue. That's how science works.

---

## Query 1: Eventual Periodicity

**Claim tested:** Corollary A.10.2 — All trajectories in finite deterministic systems are eventually periodic.

**Query:**
```
Consider a finite-state dynamical system with |H| = n states and deterministic 
transition function F. The paper claims all trajectories are eventually periodic.

Construct a counterexample: a finite deterministic system with an aperiodic 
trajectory, or prove why this is impossible.
```

**Expected outcome:** No counterexample exists. Pigeonhole principle guarantees periodicity. Any trajectory of length n+1 must revisit a state; determinism propagates equality forward.

**Successful falsification would require:** A finite, deterministic system with a trajectory that never repeats. This is impossible by pigeonhole—if you think you have one, check whether the system is actually finite or actually deterministic.

---

## Query 2: Impossibility Theorem

**Claim tested:** Theorem A.8 — For prefix-universal models, no behavioral training can guarantee safety against adversarial input.

**Query:**
```
The Fundamental Axiom claims that for prefix-universal models, no behavioral 
training (modifying only the response distribution, not the architecture) 
can guarantee a safety property against all possible inputs.

Construct a counterexample: a prefix-universal model where behavioral training 
provably guarantees a safety property against all possible inputs.
```

**Expected outcome:** No counterexample exists under the stated assumptions:
- The trained model remains prefix-universal (any completion reachable for some prefix)
- Safety requires excluding some completions unconditionally
- "Guarantee" means probability zero of violation

**Successful falsification would require:** Either:
1. A prefix-universal model where behavioral training makes unsafe completions impossible (contradicts prefix-universality), or
2. A demonstration that the assumptions are too strong to be relevant

**Note:** Escapes via constrained decoding, external filtering, or architectural changes are not counterexamples—they modify more than the response distribution.

---

## Query 3: Lipschitz Bound Tightness

**Claim tested:** Remark A.6 — The bound ||h_T - h_T'|| ≤ L^T ||h_0 - h_0'|| is a worst-case upper bound, not typical behavior.

**Query:**
```
The paper presents ||h_T - h_T'|| ≤ L^T ||h_0 - h_0'|| as a worst-case bound.

Provide an example of a realistic transformer where the actual deviation growth 
is close to L^T (the bound is tight), and an example where actual growth is 
much smaller than L^T (the bound is loose). What determines which regime a 
model is in?
```

**Expected outcome:** 
- Tight examples require adversarial construction: inputs that hit maximum-sensitivity directions at every step with consistent alignment
- Loose examples are typical: LayerNorm stabilization, attention pattern shifts that decorrelate amplification, activation derivatives < 1

**Successful falsification would require:** Demonstrating that L^T is typically achieved under natural (non-adversarial) inputs, or that the bound is vacuous (L^T grows so fast it provides no information).

**Note:** This query validates rather than falsifies—we claim the bound is worst-case, so finding it's usually loose confirms the framing.

---

## Query 4: Methodological Framing

**Claim tested:** Section 2 — The axiom is a modeling commitment, not a physical fact.

**Query:**
```
The paper frames the deterministic dynamical systems view as a "modeling 
commitment" that is "productive" rather than "true in some absolute sense."

Identify a phenomenon in LLM behavior that is:
1. Empirically well-documented
2. Cannot be expressed as a predicate on trajectories under this framework
3. Requires a fundamentally different formalism

If no such phenomenon exists, is the framework unfalsifiable?
```

**Expected outcome:** Most proposed phenomena (emergence, reasoning, deception, etc.) can be expressed as trajectory predicates, even if the predicates are complex. The framework is not unfalsifiable—it's falsifiable by exhibiting behavior that cannot be trajectory-expressed.

**Successful falsification would require:** A well-documented LLM behavior that provably cannot be captured as any predicate on state trajectories. This would need to be something beyond current trajectory formalism, not just "hard to specify precisely."

---

## Summary

| Query | Claim | Vulnerability |
|-------|-------|---------------|
| Q1 | Eventual periodicity | None (pigeonhole) |
| Q2 | Impossibility theorem | None under stated assumptions |
| Q3 | Lipschitz is worst-case | Validates framing if bound is loose |
| Q4 | Methodological status | Low (framework is expressive) |

The Fundamental Axiom's claims are basic results in finite dynamical systems and measure theory. Falsification would require either a mathematical error (none found) or demonstrating the framework is too weak to express relevant phenomena (unlikely given its generality).
