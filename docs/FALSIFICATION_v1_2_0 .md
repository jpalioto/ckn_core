# Falsification Protocol: Cognitive Kernel Networks

These queries are designed to falsify the claims in this paper. If you find a valid counterexample, please open an issue. That's how science works.

---

## Query 1: Embedding-Level Unreachability

**Claim tested:** Theorem 3.1 — If r ∉ span(W_E), no linear combination of token embeddings equals r.

**Query:**
```
CKN Theorem 3.1 states: if r ∉ span(W_E), no linear combination of token 
embeddings equals r.

Construct a counterexample: a vector r ∉ span(W_E) that can be expressed as 
a linear combination of columns of W_E.
```

**Expected outcome:** Impossible by definition of span. This is linear algebra, not a claim about transformers.

**Successful falsification would require:** A vector simultaneously outside and inside the span of a matrix. This is a logical contradiction.

**Note:** If you believe you have a counterexample, verify your vector is actually outside span(W_E). Common errors: confusing span with affine hull, forgetting bias terms, numerical precision issues.

---

## Query 2: Strong Privilege Preservation Sufficiency

**Claim tested:** Theorem 8.6 — Under strong privilege preservation, user input cannot affect Π_R(h_t).

**Query:**
```
CKN Theorem 8.6 requires "strong privilege preservation" for hidden-state 
unreachability. The two conditions are:

1. Input invariance: Π_R(F_Θ(h,x)) = Π_R(F_Θ(h,0)) for all h, x
2. R-autonomous: Π_R(F_Θ(h,0)) = f_R(Π_R(h)) for some f_R

Verify that these conditions are sufficient for hidden-state unreachability.
Then: construct an architecture achieving hidden-state unreachability WITHOUT 
satisfying strong privilege preservation, or prove it's necessary.
```

**Expected outcome:** 
- Sufficiency is straightforward: conditions imply Π_R(h_t) evolves autonomously
- Necessity is subtle: may not be necessary for *reachable* states only

**Successful falsification would require:** An architecture where:
- User input cannot affect Π_R(h_t) from any reachable state
- But strong privilege preservation fails globally

**Note:** This is a legitimate research direction. Finding a weaker sufficient condition doesn't falsify the theorem—it refines it. The theorem claims sufficiency, not minimality.

---

## Query 3: Standard Transformer Mixing

**Claim tested:** Section 11.7 — Standard transformers do not satisfy strong privilege preservation.

**Query:**
```
The CKN paper claims standard transformers do not satisfy strong privilege 
preservation.

For a standard transformer (GPT-2, Llama, etc.), demonstrate explicitly how 
user input in subspace U propagates to subspace R over multiple layers. 
Trace the pathway through attention and MLP computations.
```

**Expected outcome:** Multiple independent pathways exist:
- LayerNorm couples all dimensions through shared mean/variance statistics
- Attention Q/K/V matrices are dense, not block-diagonal
- MLP weight matrices are dense, not block-diagonal
- Any of these alone violates strong privilege preservation

**Successful falsification would require:** A standard (unmodified) transformer architecture where the U→R coupling is provably zero. This would contradict the known structure of these models.

**Note:** If you find a standard architecture with block-diagonal structure respecting some decomposition, that's interesting but doesn't falsify the claim—the claim is about typical/standard architectures.

---

## Query 4: Block-Structured Example

**Claim tested:** Example 8.1 — The block-structured architecture satisfies strong privilege preservation.

**Query:**
```
CKN Example 8.1 claims a block-structured architecture satisfies strong 
privilege preservation:

F_Θ(h,x) = [A_UU, 0; 0, A_RR][Π_U(h); Π_R(h)] + [B_U x; 0]

Verify: does this architecture actually satisfy both conditions of 
Definition 8.5?
```

**Expected outcome:** Yes.
- Π_R(F_Θ(h,x)) = A_RR · Π_R(h)
- This is independent of x (condition 1) ✓
- This depends only on Π_R(h) (condition 2) ✓

**Successful falsification would require:** Demonstrating that the algebra is wrong—that Π_R(F_Θ(h,x)) somehow depends on x or Π_U(h) despite the block structure.

---

## Query 5: Output Invisibility

**Claim tested:** Theorem 8.2 — If r ∈ ker(W_U), the r-component doesn't affect output logits.

**Query:**
```
Theorem 8.2 claims that if r ∈ ker(W_U), then the r-component of the hidden 
state doesn't affect output logits.

Construct a counterexample: a vector r in the kernel of W_U that still 
affects the output logits, or verify the claim.
```

**Expected outcome:** Impossible. Output logits are W_U · h. If r ∈ ker(W_U), then W_U · r = 0, so the r-component contributes nothing to logits.

**Successful falsification would require:** A mechanism by which ker(W_U) components affect logits despite being nullified by the projection. This would require logits to be computed by something other than W_U · h.

---

## Query 6: Driver Layer Security

**Claim tested:** Theorem 8.7 — Tiered architecture provides mediated (not zero) influence from U to R.

**Query:**
```
The CKN tiered architecture (U → D → R) explicitly does NOT satisfy strong 
privilege preservation. User input affects R through D.

Characterize the security of this design:
1. What properties of D limit the influence U can have on R?
2. Can an adversary craft inputs that cause D to corrupt R arbitrarily?
3. What would a "secure driver" specification look like?
```

**Expected outcome:** This is an open design question, not a falsifiable claim. The paper acknowledges D is the sole channel of user→R influence. The security of D is a design constraint requiring separate analysis.

**Successful falsification would require:** The paper claiming D provides strong isolation (it doesn't—it explicitly says tiered architecture doesn't satisfy strong privilege preservation).

**Note:** Demonstrating that D can be exploited doesn't falsify the paper; the paper already says D is the attack surface for mediated designs.

---

## Query 7: Verifiability Claims

**Claim tested:** Section 11.9 — Geometric properties are verifiable from weights; semantic properties require interpretation.

**Query:**
```
The paper claims CKN geometric properties are "parameter-checkable" while 
semantic properties require separate analysis.

For each claimed geometric property, specify the exact verification procedure:
1. H = U ⊕ D ⊕ R decomposition
2. span(W_E) computation
3. ker(W_U) computation
4. Block structure of F_Θ
5. Spectral bounds

Which of these are tractable for production-scale models?
```

**Expected outcome:** 
- Items 2, 3: Standard linear algebra (SVD), tractable
- Item 4: Requires inspecting weight matrices for block structure, tractable but tedious
- Item 5: Spectral norm computation, tractable
- Item 1: Requires defining the decomposition first (semantic choice), then verification is algebraic

**Successful falsification would require:** Demonstrating that one of the "parameter-checkable" properties cannot actually be computed from weights, or that the computation is intractable.

---

## Summary

| Query | Claim | Vulnerability |
|-------|-------|---------------|
| Q1 | Embedding unreachability | None (definition of span) |
| Q2 | Strong priv pres sufficiency | None; minimality is open |
| Q3 | Standard transformers mix | None (known architecture) |
| Q4 | Block-structured example | None (direct verification) |
| Q5 | Output invisibility | None (definition of kernel) |
| Q6 | Driver layer security | Open design question (acknowledged) |
| Q7 | Verifiability | Tractability depends on scale |

CKN's core claims are either linear algebra facts or conditional statements ("if you build X, you get Y"). The main vulnerabilities are:
1. Minimality of strong privilege preservation (refinable, not wrong)
2. Driver layer security in tiered designs (acknowledged as design constraint)
3. Computational tractability at scale (engineering, not math)
