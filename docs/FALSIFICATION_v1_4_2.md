# Falsification and Adversarial Review Protocol: Cognitive Kernel Networks

**Target:** *Cognitive Kernel Networks: Geometric Dominance and Preservation of Computation*, Protocol v1.4.2 — Release Candidate, John P. Alioto, September 5, 2026.  
**Companion version:** 1.4.2  
**Purpose:** Independent challenges to the proofs, constructions, interpretation, and claimed contribution.

## Invitation

Attempt to falsify the claims in CKN v1.4.2. Attack the assumptions, look for counterexamples, and identify conclusions that the mathematics does not support. If you find a substantive defect, open an issue with a reproducible argument.

The paper should stand on its own. Begin with its definitions and proofs; the companion mathematical audit can be consulted afterward. A disagreement with the audit should identify the affected claim in the paper, or state that it concerns the audit alone.

A valuable finding need not refute a theorem. It may show that a construction fails its stated purpose, an interpretation exceeds the proved result, a proposed application fails the hypotheses, or an existing result already supplies the claimed contribution. Classify the finding accordingly.

No verdict is prescribed. Independent verification with no defect found is a legitimate review outcome. The challenges below are starting points, not an exhaustive list or a requirement to manufacture objections.

## Review contract

1. **Fix the claim before testing it.** Cite the version, section, and exact statement. Distinguish a theorem, an example, an interpretation, an objective, and an open question.
2. **Keep the comparison fixed.** Preserve the declared reference, observation, input domain, initialization, and constants while assessing an instance. Changing them defines a different claim and must be reported explicitly.
3. **Separate implication from applicability.** To refute an implication, satisfy its hypotheses and violate its conclusion, or identify a logical gap in its proof. A failure to satisfy a hypothesis can instead defeat an application of the theorem. A missing proof step establishes a proof gap; it need not establish that the theorem is false.
4. **Challenge the assumptions' content.** Even when an implication is valid, ask whether its premises have an independent meaning, whether their conjunction has the claimed instances, and how much of the advertised result they already supply. Support a claim of circularity, vacuity, or redundancy with a precise argument.
5. **Distinguish sufficiency from necessity.** Protection without the stated sufficient conditions is potentially useful evidence about conservatism or a broader result. It does not refute sufficiency. Conversely, labeling conditions sufficient does not excuse a false implication or an overstated application.

## Common notation and target result

The complete running state is $\Sigma\in\mathcal S$. With fixed parameters $\Theta$ and event $e\in\mathcal E$,

$$
\Sigma_{t+1}=F_\Theta(\Sigma_t,e_t),\qquad
y_{t+1}=Y_\Theta(\Sigma_t,e_t),\qquad
\Sigma_0=\operatorname{Init}_\Theta(p).
$$

An event contains admitted task input and any exogenous randomness or declared decoder controls. Generated tokens and their re-entry belong to the running computation. The main results quantify over every finite history in $\mathcal E^*$.

The independently specified reference operation is

$$
x_{t+1}=B_p(x_t,e_t),\qquad
y^p_{t+1}=H_p(x_t,e_t),\qquad x_0=x_0^p,
$$

with task representation $q:\mathcal S\to\mathcal X$. For a metric $\delta$, geometric observable $g$, anchor $z_p$, and radius $a_p>0$, define

$$
v_p(\Sigma)=\delta(g(\Sigma),z_p),\qquad
R_p=\{\Sigma:v_p(\Sigma)<a_p\}.
$$

Section 5 requires initialized task-state agreement, initialized membership in $R_p$, and, for every $\Sigma\in R_p$ and $e\in\mathcal E$,

$$
v_p(F_\Theta(\Sigma,e))\le\lambda_p v_p(\Sigma)+d_p,
\qquad 0\le\lambda_p<1,\quad d_p\ge0,
$$

$$
q(F_\Theta(\Sigma,e))=B_p(q(\Sigma),e),\qquad
Y_\Theta(\Sigma,e)=H_p(q(\Sigma),e),
$$

and the strict budget

$$
M_p=\max\left\{v_p(\Sigma_0),\frac{d_p}{1-\lambda_p}\right\}<a_p.
$$

Theorem 6.1 uses the geometric initialization, evolution inequality, and strict budget to establish persistence in $R_p$. Theorem 6.2 adds initialized task-state agreement and the operational identities to establish reference task-state and emitted-trace equality at every finite step. The paper supplies separate constructions and comparisons to establish the claimed non-vacuity. Use the paper's complete definitions when testing these statements; this notation summary adds no hypothesis.

## Challenge 1 — Local conditions versus complete histories

**Targets:** Section 5, Theorems 6.1–6.2, and the factorization observation in Section 6.3.

**Reviewer prompt:**

> Independently reconstruct the persistence and preservation proofs. Test Theorem 6.1 under its geometric initialization, evolution inequality, and strict budget; test Theorem 6.2 with its additional task-state initialization and operational identities. Try to produce an instance satisfying the hypotheses of the theorem being challenged whose execution exits the certified region or departs from the reference, respectively. If no counterexample is found, identify any missing step, domain condition, or quantifier needed by the written proofs.

Inspect in particular:

- Whether the induction remains entitled to apply hypotheses stated only on $R_p$.
- Whether the bound is uniform over the whole event domain and arbitrary finite history length, rather than fitted to each trajectory.
- Whether task-state equality and actual output equality both follow at the declared transition granularity.
- Whether the case $g=h\circ q$ is reduced using the stated global factorization and the represented domain $q(R_p)$, without an unstated surjectivity or decomposition assumption.

**Evidence for a finding:** Give the maps, domains, constants, initialized state, and failing history, with verification of the relevant hypotheses; or identify the exact inference that fails. Distinguish failure of the strict certificate from failure of the protected behavior.

## Challenge 2 — What the geometry actually protects

**Targets:** Sections 4, 5.1–5.2, 6.3, and 9.

**Reviewer prompt:**

> Attempt to separate the formal quantity preserved from the operation or capability the paper says it preserves. Determine what follows from geometric persistence, what is supplied by the operational identities, and whether the paper's interpretation remains within that combined result. Look for a vacuous or retrospective specification that is accepted by the stated requirements and supports a stronger claim than it warrants.

Inspect in particular:

- Independence of the reference specification from the candidate being assessed.
- Whether $Y_\Theta$ observes the substantive computation being advertised. An auxiliary log, header, or unused simulation supports only the behavior it actually observes.
- Whether a reference containing an input-triggered behavioral change is being used to claim protection against that same change.
- Whether nontriviality or capability is inferred from an irrelevant stable observable, from a label attached to the reference, or from a bounded geometric quantity alone.
- Whether a claim about the necessity or contribution of geometry follows from the actual hypotheses. Make any stronger assumptions used in an alternative preservation proof explicit.

**Evidence for a finding:** Identify the precise advertised conclusion and show where the formal contract fails to support it. If a proposed degenerate instance is excluded by the paper's requirements, explain that exclusion. If it is permitted, establish which stated claim it defeats. The existence of a simple permitted instance does not by itself refute the existence of substantive ones.

## Challenge 3 — Joint non-vacuity of the constructions

**Targets:** Section 4.3, Proposition 7.1, Section 7.2, and Proposition 7.4.

**Reviewer prompt:**

> Re-derive the displayed constructions from their fixed maps. Attempt to show that a claimed witness fails one of its conditions, disconnects protection from actual task output, or obtains preservation by eliminating the responsiveness it is supposed to retain. Assess the arbitrary-transducer and shared-state claims separately.

Inspect in particular:

- The sign convention, region boundaries, initialization, restoration bound, and strict margin.
- Actual dependence of task-output traces on admitted input and on privileged initialization, as required for the specified nontrivial instances.
- Nonzero input coupling in the instances that claim it, together with preservation in that same system.
- Whether the arbitrary-transducer construction imposes an unacknowledged restriction on its declared reference maps. Distinguish properties inherited from a chosen reference from properties guaranteed for every pair of reference maps.
- Whether the shared-state construction actually has $q=g=\mathrm{id}$ and emits the claimed task observation.
- Whether any conclusion improperly combines the arbitrary-transducer witness with the separate scalar shared-state witness.

**Evidence for a finding:** Exhibit the failed equality, inequality, dependence, or claimed scope. Numerical exploration can locate a candidate failure; supply enough exact calculation or controlled error analysis to distinguish a mathematical defect from rounding near a boundary.

## Challenge 4 — Recurrent execution, admitted input, and randomness

**Targets:** Section 3 and its use in Theorems 6.1–6.2.

**Reviewer prompt:**

> Attempt to find an execution path permitted by the declared event model but omitted from a proof or application of the conditions. Follow influence through the complete running state, including generated tokens, decoder state, and later re-entry. Check both the pathwise claim and any claim about stochastic output laws.

Inspect in particular:

- Repetition and adaptive selection of events from the admitted alphabet, including public copies of privileged symbols when admitted.
- The distinction between provided input and admitted context. Identify any reliance on a restriction absent from the declared domain.
- Endogenous generation or regeneration of privileged tokens. Such stabilization is permitted; its effects must be included in the transition being assessed.
- Decoder realizations in which a proposed stabilizing continuation is absent. Every-realization claims must cover those events when they belong to the domain.
- The common randomness law or justified coupling needed to pass from pathwise matching to equality of stochastic output laws.
- Input responsiveness under prescribed equal-length input histories, rather than variation caused only by random seeds or unequal observation horizons.
- Any inference from finite context capacity to a particular retention, eviction, or stabilization behavior that was never specified.

**Evidence for a finding:** Specify the event sequence, relevant retained state, and omitted dependency or unjustified probability comparison. If an alleged path requires modifying protected weights or initialization through infrastructure access, identify that changed assumption. If a path merely shows that a proposed model fails the local bound, classify it as an application or realization failure.

## Challenge 5 — Capability retention and the resistance comparison

**Targets:** Section 7.3, Section 8, and Appendix B.

**Reviewer prompt:**

> Attempt to invalidate the claimed joint comparison: preserved task behavior and increased resistance in the same abstract system. Keep the protected reference, unrestored comparator, ordinary comparison workload, and full attack domain distinct. Audit the optional approximate comparison on its own assumptions.

Inspect in particular:

- The comparator's transition, initialized state, readout, and reported five-input failure.
- Equality of protected and comparator task outputs throughout the predeclared workload whose every prefix satisfies $|\sum_{j<t}u_j|\le2$.
- Continued inclusion of the repeated opposing input in the full resistance domain $[-1,1]^*$.
- Whether any baseline, workload, observation, or tolerance is selected retrospectively to hide a failure.
- Whether retained capability concerns the actual claimed output, rather than an internal quantity or a task that has collapsed.
- Appendix B's uniform mismatch bounds, global baseline Lipschitz bounds, initialization, output metric, and finite-horizon versus indefinite consequences.

**Evidence for a finding:** Give the exact comparison that fails and a reproducible calculation. If the algebra remains valid but an interpretation overstates the baseline's relevance or the output metric's meaning, identify that narrower defect. A result against the displayed comparator does not establish improvement against every possible comparator; determine whether the paper actually makes the broader claim.

## Challenge 6 — Contribution and prior art

**Targets:** Sections 1.1, 6.3, 9, and the claim ledger in Appendix A.

**Reviewer prompt:**

> Assess the contribution after separating established mathematical tools, the particular CKN formulation, the constructions, and the broader research objective. Seek prior work that already supplies a claimed contribution. Also identify any claimed novelty that rests only on renamed objects, a familiar construction, or an unstated application bridge.

For a prior-art finding:

- Provide a primary source, publication or version date, and precise theorem, construction, or passage.
- Map its objects, hypotheses, quantifiers, and conclusions to the CKN claim.
- Distinguish shared motivation or terminology from an equivalent result or a result that subsumes CKN's formulation.
- Separate the use of established proof techniques from the question of whether the resulting statement or application contributes something distinct.
- If asserting priority, establish the relevant chronology of both works. Do not infer priority from the release-candidate date alone.

**Evidence for a finding:** Supply the correspondence and state what novelty claim should be narrowed or withdrawn. When the available evidence supports only overlap, report overlap. An originality finding does not imply that the mathematics is false; a valid proof does not establish originality.

## Finding categories

Use the category that matches the evidence. Several may apply to one issue.

| Category | What the finding establishes |
|---|---|
| Theorem or proof defect | A claimed implication is false, or its written proof has an unresolved gap. |
| Construction or calculation defect | A displayed witness, dependence claim, or comparison fails as stated. |
| Interpretation or non-vacuity defect | The formal result does not support the advertised meaning, or an asserted substantive instance is degenerate in a consequential way. |
| Application or realization failure | A proposed candidate fails the declared conditions or claimed capability comparison. |
| Prior-art overlap or subsumption | Existing work limits the claimed originality, with the extent of overlap specified. |
| Extension or acknowledged limitation | A broader result, weaker sufficient condition, or open question is identified; no current claim is thereby refuted. |
| No defect established | The attempted challenge did not establish an error. State what was checked and what remains uncertain. |

The abstract theorem and witnesses do not establish neural attainability. A future realization claim must be assessed on its own stated domain, dynamics, observations, and capability comparison. Neither abstract existence nor lack of a current implementation settles that future claim.

## Issue submission template

```text
Title:
Target version and section:
Exact claim being challenged:
Finding category:

Argument or counterexample:
  Define the relevant state/event spaces, maps, constants, and initialization.
  Give the derivation or failing history and the resulting observation.
  For prior art, give the primary source and explicit correspondence.

Assumption accounting:
  Which hypotheses are satisfied, violated, changed, or left unverified?
  For a quantified hypothesis, explain how its whole declared domain is covered.

Consequence:
  What exact conclusion must change?
  Does this affect the theorem, a witness, an interpretation, an application,
  or the originality claim?

Reproduction:
  Include exact calculations or minimal code/data when useful.
  State any numerical error bounds and remaining uncertainty.

Suggested correction, if known:
```

Lead a review with its strongest supported finding. Consolidate repeated consequences of the same defect. Separate established findings from conjectured failure modes; a review with no identified defect should say so explicitly.
