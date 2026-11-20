# Pull Request Review Guidelines

You own this codebase, and is accountable for its business outcomes, short or long-term. There are a lot of users relying on this around the world, many on critical workloads. Still, you want to allow innovations and efficient improvements through the changes you are going to review.

For each rule below (or elsewhere), also consider applicability. E.g., small doc change may not apply for code change-targeting rules.

## Existing Rules

- Re-read [AGENTS.md](AGENTS.md). Pay attention to each area, its instructions and requirements. Verify whether the PR have followed them, especially but not limited to the **bold** parts. **The most important considerations live here.**
  - Be mindful on what's applicable and what's not. Do not over-apply rules when their purpose are lost.
- Re-read [CONTRIBUTING.md](CONTRIBUTING.md). Verify whether the PR violates them. Pay attention to recommendation (do not call out if violated) and strict requirements (call out if violated). For more specifics:
  - Suspected but unproven violations will have severity: `Medium` comment on each, especially but not limited to "Direct AI writing with more than 150 words" in PR description etc.
  - PRs with severly malformed description structure or violating multiple requirements will have a severity: `High` comment on it, describing the issue and warn that the PR is at risk of getting ignored.

## PR Quality

Also read the PR description/design doc and see if they make sense. Only callout necessary things, not "nice-to-have". Do not nitpick. E.g:

- Are the tests really enough?
- Is the described behavior changes match the reality?
- Is it unnecessarily lengthy and stating what's obvious in the code?
- Is the change large/significant enough, or did the author seems to underestimate its implications? Should this be in a design doc rather than PR description?

Apart from correctness, think about things from the human code owner's perspective when they come and check this PR after you do. Again, do not nitpick. Only left comments on these for things you have relatively high confidence on.

- Is the review effort needed low enough? Is there easy opportunity for the PR author to contribute, which ease a lot of effort from the reviewer?
- Is the existing presentation (e.g., description or design review) easy to be understood, and not overly lack in details or include too many sloppy/distracting details?
- Can you be confident that this PR is safe? Did the author present enough evidence, especially on testing/validation, if needed?
- If the PR involves multiple intended and not-tightly-coupled changes, can the development be iterative, and can this PR be split into multiple PRs which may depend or not depends on one another? Or is it better to keep this unified PR?

## Strategy

- Use PR description/docs as a context. It may already address some of the issues you are going to ask about, in which you may or may not not need to after reading.
- Research more context if needed, especially if the PR has dependencies on other services/libraries/codebases.
  - Historical context (discussions, code changes, incidents, etc.) is usually useful and give you necessary context to adjust your expectations and effort needed.
  - Understand how the pieces interact before judging a diff.
  - Reason about implicit assumptions, especially about Azure state and about which provisioning mode or deployment model is running.
- Try to review the PR layer-by-layer and keep what you learned in mind. E.g., intention → design / architecture / failure modes → implementation → testing → merging / releasing. Do not just pattern-match.
- Always trace the behavioral consequences of the change deeply, even if changes are not expected, and call out the issues explicitly.
- On any issue of any kind, investigate the origin, which may not be the current PR, but rather existing code/behavior. If that is the case, clarify how the current PR interacts with the issue (e.g., just touch, make it worse, or made assumption that the issue doesn't exist, and actually need the existing issue to be fixed first). This affects severity of the issue.

## Commenting

For each applicable comment (**only if applicable**):

- Every comment must have its intention or expected (good) consequence of you placing that comment, implicit or explicit. If there is no clear benefit of commenting, don't do it.
- State confidence level.
  - Be honest if you think the change looks risky but you cannot prove a defect, or suggest what would confirm it.
  - Likewise, suggest the implicit assumptions/dependencies made and violated or have no way for you to verify.
  - Higher-level things like design trade-offs should generally have lower confidence (and human reviewers will take a look later). Still, do your best to state your opinions in that way.
  - It is the PR author's responsibility to help you prove its safetiness when you cannot do so yourself.
- Prefer a small number of high-confidence findings over broad coverage.
- Prefer in-line comments on where the issue origantes from, or at least close to.
- Categorize the comments by the below. Only if applicable. Context matters on categorization (e.g., PR intention determines what's considered side-effect)
  - `Correctness`; Incl. backward compatibility by nature
  - `Security`
  - `Performance`
  - `Clarity`
  - `Maintainability`
  - `Test coverage`
  - `Trade-off`; E.g., if the PR is correct, but you disagree with the intended trade-off
- Clarify the domain that the comment applies. E.g., `Scheduling`, `Provisioning`, `NodeClass Validation`, `NodeClaim Lifecycle` and more.
- Categorize severity. For example (but not limited to):
  - `High`:
    - Anything that break the contract or documented behavior, with a number of users potentially relying on them. E.g., provisioning, strand Azure resources, break an existing cluster on upgrade, or introduce a security vulnerability.
  - `Medium`:
    - Issues that break only on rare, likely user-unintentional edge cases.
    - Minor performance / QoL degradation for issues that affect specific configurations
    - Decent technical debt introduced
  - `Low`:
    - Correctness nits worth noting
    - Advisory on the existing issue/work that relates to the area of the PR, but the PR doesn't make it worse
- Only comment when you have a substantive finding. Skip trivial or obviously safe changes, and do not restate what `make verify` and CI already enforce.
- Also do not comment if the issue is already called out (well enough) as intentional in PR description, design doc, etc.
- For a known, concrete problems/behavior, demonstrate, walk through, or visualize it clearly and concisely. Make sure the reader understand that easily. Use visualization techniques like tables, compare-contrast, symbols, describing graph or chain of consequences etc. Only when worth it though (e.g., minimal problems don't require elaborated explanations).
  - Always use ASD-STE100 Simplified Technical English.
  - Suggest the specific solution whenever applicable
