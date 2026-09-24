# Execution work packages — v2.0



**Planning breakdown, not new issues created and not an effort estimate.** Map each package to an existing child issue or create a child under the current Check My Meal feature only after checking for duplicates.



## WP01 — Scope, content and release ownership

**Accountable roles:** Product + Clinical Nutrition + Business

**Output:** Agree two-mode boundary; review six preview topics, claims, warnings, launch subset, approved paid benefits and glossary.

**Dependencies:** None; prerequisite for production publication.

**Done when:** Approved content/hash, decision register and named owners; no topic is auto-approved from prototype.



## WP02 — Backend context, preview publication and action resolver

**Accountable roles:** Backend + Product

**Output:** Implement typed content schema, compatibility, state resolver, category preference isolation, publication/revocation and profile-aware commercial actions.

**Dependencies:** WP01 content contract; existing identity/free/paid/order APIs.

**Done when:** Contract tests prove profile isolation, no duplicate checkout origin and no preview-to-analysis rule mutation.



## WP03 — Food catalogue, numeric rules and safeguards

**Accountable roles:** Clinical Nutrition/Data + Backend

**Output:** Publish catalogue/preparation/portion/free-sugar data; implement retained general profile, facts-only and governed actions; precision vectors.

**Dependencies:** Existing source rule review; G02/G03 approval.

**Done when:** Exact reproducible outputs for signed-off vectors; incomplete data never becomes zero or healthy.



## WP04 — Attempts, recognition, history and privacy

**Accountable roles:** Backend + Security/Privacy

**Output:** Implement bounded attempts, model-output validation, mandatory mapping/confirmation, worker cancellation, idempotency, immutable records and cleanup.

**Dependencies:** WP02 identity/context; WP03 catalogue and calculation; provider/consent decisions.

**Done when:** Server tests for deadline races, duplicate finalisation, suspension, late workers, revision conflicts and deletion.



## WP05 — Reusable native mobile screens

**Accountable roles:** UX + React Native

**Output:** Build hub, optional category picker, preview blocks, consent/safety forms, all input routes, editors, progress, result/history/expiry states; integrate typed contracts.

**Dependencies:** WP02 schemas; approved mock state map; mock API fixtures allow parallel layout work.

**Done when:** No mobile clinical formulas; tested navigation, VoiceOver/TalkBack, font scaling and native permission recovery.



## WP06 — Existing paid-plan and service integration

**Accountable roles:** Backend + Mobile + Operations

**Output:** Map preview topics to current plans only when valid; apply pending/active/unavailable actions across preview and real-meal origins.

**Dependencies:** WP02 resolver + existing plan catalogue; G05 benefits approval.

**Done when:** Payment never implies activation or resets free access; Your Diet/labs/paid Home remain unchanged.



## WP07 — Integrated QA, rollout and release evidence

**Accountable roles:** QA + all owners

**Output:** Execute cross-mode/security/calculation/accessibility/regression matrix, pilot with feature controls and rollback; reconcile issue and mocks.

**Dependencies:** WP01–06 complete for intended launch scope.

**Done when:** Recorded test evidence and approval gates; preview-only staged release explicitly labelled if real analysis remains gated.



## Sequencing and safe parallel work

WP01/02 settle scope and contract first. UX and mobile can work against fixtures in parallel with catalogue/attempt implementation. Production category publication waits for the content approvals; real checks wait for signed-off data, rules and privacy controls. Do not treat a working screen mock as proof the upstream dependency exists.



## Release approach

Use separate flags for category previews, new real checks and optional glucose-impact output. Test with internal/Medical reviewers, then the approved pilot cohort. Roll back a content bundle independently of numeric policy; preserve historical results and do not silently re-score. Log changes to the feature issue with the matching v2.0 artifacts.
