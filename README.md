# Cognitive Kernel Networks (CKN)

### Root Trust for Privilege-Separated Reasoning in Latent Space  
### CKN ≡ 𝒦⊗

<p align="center">
  <img src="docs/media/ckn_canonical_logo.jpg" width="260" alt="CKN Tensor Logo (𝒦⊗)">
</p>

CKN is a **geometric primitive** for building **privilege-separated reasoning** inside
high-dimensional neural models.

Where CTN (Cognitive Tensor Networks, 𝒯⊗) shapes **user-space geometry** via structured prompts,
CKN shapes **architecture-level geometry** by defining a **privileged reasoning manifold** that
user tokens cannot reach *by construction*.

CKN is:

- math, not machinery  
- a **specification**, not an implementation  
- a **root trust** concept, not a new architecture  
- an application of tools model builders already have  

---

## 𝒦⊗ One-Line Intuition

> **The weights define the conceptual world.  
> CKN defines the part of that world user tokens can’t touch.**

Everything else is engineering on top.

---

## 𝒦⊗ Why Geometry Has To Be Solved Geometrically

Today, **all constraints and all attacks live in the same space**:

- system prompts  
- safety policies  
- user queries  
- adversarial text  

After embedding, they are all just vectors in the same subspace:

```math
\text{system tokens} ∈ span(W_E)  
\text{safety tokens} ∈ span(W_E)  
\text{user tokens}   ∈ span(W_E)
````

So:

* safety tokens **compete** with adversarial tokens
* both pull on the same latent geometry
* as sequences grow, initial constraints dilute
* the model’s priors dominate the trajectory

This is not a prompt-engineering failure.
It’s an **architectural limitation**:

> You can’t solve a kernel-level problem from user space.
> Adding more tokens into the same manifold just moves the geometry around.

CKN’s answer:

> **Move privileged computation into directions user tokens can’t reach.
> Not by policy. By linear algebra.**

---

## 𝒦⊗ Builder Control → Root Trust

Model builders **already control** everything that matters:

* latent dimensionality `n`
* embedding matrix `W_E` (tokens → vectors)
* unembedding matrix `W_U` (vectors → logits)
* attention / MLP projection structure
* internal tokens and reserved embeddings

Users only control the **tokens**. They never touch Θ.

If the builder chooses:

```math
rank(W_E) < dim(ℋ)
```

then there exist directions in latent space that **no external token can ever express**.

Call:

* `U = span(W_E)` → user-span (reachable by tokens)
* `R ⊂ ℋ` with `R ∉ U` → privileged directions (unreachable by tokens)

Then:

> **No linear combination of input embeddings can produce a component along `R`.
> User-space is algebraically confined to `U`.**

That’s **root trust**:

* directions in ℋ that are unreachable from the vocabulary
* fully visible to the builder
* fully invisible to user-space

This is not probabilistic, not heuristic, not “alignment”:

> **It’s a hard guarantee from linear algebra.**

---

## 𝒦⊗ From Root Trust to Privilege Separation

Once unreachable directions exist, everything else is design.

CKN uses them to define:

* **User space**: `U = span(W_E)`
* **Kernel space**: `R ⊂ ℋ` with `R ∉ U` (privileged manifold)
* **Privilege boundary**: the algebraic separation between `U` and `R`

User tokens:

* live in `U`
* can perturb `U`
* *cannot* generate components in `R`

Privileged computation:

* lives in `R`
* can depend on `U` via a **driver layer** `D : U → R`
* cannot be rewritten by any combination of user tokens

Information flow is:

```text
U  ──D──>  R  ──D†──>  U
```

Where:

* `D` is builder-defined (what influence `U` is allowed to have)
* `D†` maps privileged results back into user-space (what gets revealed)

Everything above that — multiple rings, permeable vs strict boundaries, specialized kernels — is **engineering**, not theory.

CKN just provides the primitive:

```text
unreachable directions → privileged subspaces → mediated access
```

---

## 𝒦⊗ What CKN Is

CKN is a **geometric specification** defining a kernel:

```text
𝒦⊗ = ( R, D, I, Λ, d )
```

Where:

* `R` – privileged reasoning manifold (kernel space)
* `D` – driver/interface operators mediating U ↔ R
* `I` – architectural invariants that must be preserved in R
* `Λ` – dominance parameters (internal dynamics dominate external perturbations)
* `d` – bounded perturbation radius in R

CKN says:

* privileged computation **occurs in R**
* user-space lives in **U = span(W_E)**
* user-driven perturbations in R are **bounded** (`∥Π_R(h_{t+1} − h_t)∥ ≤ d`)
* all U ↔ R interaction goes through `D` / `D†`
* some privileged directions may be **internal-only** (representable in R but not in tokens)

CKN **does not** say:

* how R is implemented
* how D is built
* which invariants I are chosen
* which ring structure is “best”

It’s **the mathematical root of trust**, not the OS you build on top.

---

## 𝒦⊗ What CKN Is Not

CKN is **not**:

* ❌ a new architecture
* ❌ a training method
* ❌ a safety mechanism
* ❌ an exploit or jailbreak vector
* ❌ a way to bypass RLHF or policy
* ❌ a way to change what the model is “willing” to do
* ❌ a guarantee of factuality or alignment

CKN:

* **does not** modify weights at inference
* **does not** add new operations to the model
* **does not** give users any capabilities they don’t already have

It’s a **language for builders**, not a trick for users.

---

## 𝒦⊗ Relationship to CTN (𝒯⊗)

CTN and CKN are **two geometric control surfaces** on the same system:

| Aspect          | CTN (𝒯⊗) – Cognitive Tensor Networks | CKN (𝒦⊗) – Cognitive Kernel Networks |
| --------------- | ------------------------------------- | ------------------------------------- |
| Layer           | Input / prefix (user space)           | Architecture / latent (kernel space)  |
| Space           | `U = span(W_E)`                       | `R ⊂ ℋ, R ∉ U`                        |
| Mechanism       | Structured constraints in context     | Algebraic unreachability              |
| Enforced by     | Model’s emergent interpretation       | Builder’s construction                |
| Builder control | Prompt format, kernel design          | Dimensionality, `W_E`, `W_U`, Θ       |
| Scope           | Stabilize trajectories in U           | Protect computation in R              |

**CTN:**

* gives the model a **well-specified environment** to think in
* reduces under-specification and drift in user-space
* is purely a **prompting protocol**

**CKN:**

* defines where trusted reasoning is allowed to happen
* prevents user tokens from deforming privileged reasoning
* is a **latent-space spec**, not a prompt technique

They are independent but complementary:

* CTN is useful even without CKN (better prompts)
* CKN is useful even without CTN (latent privilege separation)
* Together they provide a **full story**:

  * shape the path (CTN)
  * shape the space (CKN)

---

## 𝒦⊗ Safety and Alignment Disclaimer

CKN:

* does **not** bypass model safety or RLHF
* does **not** override policy constraints
* does **not** allow users to reach anything they couldn’t reach via tokens before

CKN changes **architecture-level reasoning geometry**, not the surface API.

Safety remains:

* whatever alignment / policy / guardrails the builder has applied
* enforced at the same boundaries as before

CKN’s contribution is simply:

> to give model builders a precise way to talk about, and eventually enforce, **where** trusted reasoning happens.

---

## 𝒦⊗ Status

CKN is currently:

* **a formal specification and research direction**
* mathematically grounded in linear algebra and dynamical systems
* compatible with existing transformer-style architectures
* designed to be architecture-agnostic

What does **not** exist yet:

* a full CKN-compliant implementation
* empirical benchmarks for CKN-style architectures

Those are **future work**, not part of this repo.

---

## 𝒦⊗ Whitepaper

For a full formal treatment, see:

* **CKN Whitepaper v1.0.1 (PDF)** – `docs/CKN_Whitepaper_v1.1.0.pdf`

  * motivation and problem framing
  * builder control and algebraic root trust
  * kernel/user geometry and driver operators
  * nullspace and column-span theorems
  * bounded perturbation and stability
  * specification vs. implementation
  * relationship to CTN

---

## 𝒦⊗ Citation

```bibtex
@misc{alioto2025ckn,
  title        = {Cognitive Kernel Networks: Root Trust for Privilege-Separated Reasoning in Latent Space},
  author       = {Alioto, John P.},
  year         = {2025},
  howpublished = {\url{https://github.com/jpalioto/ckn_core}},
  note         = {Protocol v1.0.1}
}
```

---

## 𝒦⊗ Contributing

CKN is intentionally open. Useful contributions include:

* critiques of the specification
* mathematical refinements (e.g. stronger theorems, tighter bounds)
* experiments on:

  * CTN-style prompting and latent geometry
  * activation steering toward privileged directions
  * toy CKN-style wrappers for open models
* architectural proposals for models that approximately satisfy the CKN axioms
* interpretability work on internal-only directions

If you’re building or analyzing systems and want to reason about **privilege in concept space**, this repo is for you.

---

## 𝒦⊗ License & Trademarks

* MIT License — free for research and commercial use.
* © 2025 John P. Alioto.
* Cognitive Tensor Networks™, Cognitive Kernel Networks™, CTN™, CKN™, 𝒯⊗, and 𝒦⊗ are trademarks of John P. Alioto.
* Tensor logos (𝒯⊗, 𝒯⊗₀, 𝒦⊗) are copyrighted graphical works.