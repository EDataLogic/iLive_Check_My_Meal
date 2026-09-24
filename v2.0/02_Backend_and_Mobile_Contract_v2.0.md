# Backend and mobile implementation contract — Check My Meal v2.0
**Proposed service contract, not an existing API inventory.** Align route names and storage with the current iLive backend. No new service deployment or CMS user interface is mandated.

## 1. Ownership boundary
| Responsibility | Backend / governed configuration | React Native |
|---|---|---|
| Profile and free/paid/order access | Resolve from authorised selected profile and trusted timestamps | Render returned capabilities; never infer from cached `isFree` |
| Preview | Published category, ordering, all copy, supported blocks, benefit mapping, revocation | Reusable templates, tab/scroll/accessibility, category input |
| Clinical/food policy | Safety mode, rule version, nutrients, observations/actions | Capture explicit answers and food confirmations; render result |
| Recognition | Normalise/scan image, private provider request, catalogue candidates | Camera/gallery permissions and progress/cancellation |
| History | Immutable completed snapshots and linked revisions | List/detail, read-only after expiry |
| Commercial route | Typed destination and resolved state; atomic checkout guard in existing service | Navigate to permitted existing route; preserve origin |

New text/order/category data within supported blocks should require configuration publication, not app rebuilds. New block types or interactions require mobile capability support. The backend must NOT transmit arbitrary executable code or unrestricted HTML.

## 2. Independent identities and versions
`profileId`: authorised patient context; not a phone-number proxy.  
`previewCategoryId`: educational browsing preference only; nullable; not stored as a diagnosis.  
`contentBundleVersion`, `previewContentVersion`: approved public/educational release.  
`analysisPolicyId`: fixed general policy for free real checks; never taken from a preview request.  
`ruleVersion`, `foodCatalogueVersion`, `templateVersion`: frozen for a reproducible attempt.  
`safetyAnswerRevision`, `safetyPolicyVersion`, `consentVersion`: independently tracked and checked before result commit.  
`attemptId`, `attemptRevision`, `resultId`, `sourceResultId`: actual work and immutable history.  
`requestContextId`: binds profile and UI request generation to discard late responses.

A server response can advertise both activities, but content and calculation objects must remain separate. Do not reuse the prototype `frxRulesFor()` to assign a preview choice to real analysis. Do not match disease/programme names by string fragments in production. Six preview IDs are not six enrolled programmes.

## 3. Proposed endpoints and responsibilities
All profile routes require authenticated authorisation on every request. Body profile IDs do not override the authorised path/session context. Use existing naming conventions where equivalent endpoints exist.

| Operation / suggested route | Input | Output / decisive behaviour |
|---|---|---|
| GET `/profiles/{p}/meal-check/context` | Supported schema/block versions | Server time; free/paid context; booleans + reason codes; content manifest; action descriptors |
| GET `/meal-check/previews/{categoryId}` | Locale, supported schema, optional ETag | One complete approved preview; no health record or meal result |
| PUT `/profiles/{p}/meal-check/preview-preference` | One valid category ID + revision | Browsing preference only; optional, omit if not needed |
| GET/PUT `/profiles/{p}/meal-check/safeguards` | Explicit questionnaire answers, revision | Protected answers/revision; server derives safety mode |
| POST `/profiles/{p}/meal-check/attempts` | Source, mealSlot, optional sourceResultId; Idempotency-Key | Trusted attempt, revision, expiry, frozen rule/catalogue/consent references |
| POST `/.../attempts/{a}/image` | Private upload descriptor/reference | Validated transient upload; exact source-type/content checks |
| POST `/.../attempts/{a}/recognise` | Attempt revision; Idempotency-Key | Recognition job/status; candidate foods/portions/confidence; no authoritative nutrients |
| GET `/.../attempts/{a}` | Authorised profile | Status, revision, deadline, recoverable input; never another profile's data |
| GET `/meal-check/foods` | Search, preparation, catalogue version, pagination | Approved catalogue selections and supported portions |
| PATCH `/.../attempts/{a}/items` | Mapped IDs, preparation, quantities, revision | Updated draft; any old confirmation invalidated |
| POST `/.../attempts/{a}/confirm` | Explicit confirmation, expected revision, safeguards/consent revision | Server validates mappings and safety; returns confirmed revision |
| POST `/.../attempts/{a}/complete` | Confirmed revision, Idempotency-Key | Atomic finalisation or typed rejection; already completed returns same result |
| POST `/.../attempts/{a}/cancel` | Expected revision | Cancel work, no history, start cleanup; late workers may not persist results |
| GET `/profiles/{p}/meal-check/results` | Cursor, date filter | Completed personal records only, original dates and display snapshots |
| GET `/profiles/{p}/meal-check/results/{r}` | Authorised profile | Immutable recorded input/output/version snapshot; no fresh scoring |
| POST `/profiles/{p}/care-actions/resolve` | Origin, optional previewCategoryId, allowed destination hint | Active/pending/available/unavailable action; not a checkout override |

Request-schema denylist is not enough: use an allowlist and reject unknown override fields such as `ruleId`, `diseaseMode`, `safetyMode`, client nutrient totals, approved entitlement or client expiry. A calculation request does not accept `previewCategoryId` at all. Persist analytics origin separately from the calculation inputs only when privacy-approved.

## 4. Configuration contract and safe rendering
Supported MVP blocks: `header`, `notice`, `nutrient_topics`, `topic_list`, `targets_explainer`, `care_benefits`, `action_group`. A profile can omit optional blocks but must retain the mode label and critical context. Resolve six categories through explicit IDs; the sample catalogue has production disabled.

Draft → Medical/content review → Product/commercial review → Published → Retired/Revoked. Approval records include actor, timestamp, scope, language, validity, content hash and replacement/revocation reason. Publishing checks that every required block/copy/destination exists. Rollback must select a still-approved coherent version, not an arbitrary old cache.

Clients advertise supported schema and block types. If a required block/type/schema is unsupported, show a neutral unavailable/update state; do not drop a required warning and render the rest. Unknown optional decorative blocks may be omitted only if the manifest explicitly marks them nonessential. No raw HTML/script/remote event handlers. Destinations must be typed allowlisted app routes.

Preview content may use an approved still-valid cache. A revoked version is ineligible. If revocation status cannot be verified, serve no new preview as approved; keep history and other permitted actions available. The exact offline validity window is a publishing policy requiring owner sign-off, not an invented client constant. Cache keys include locale, category, schema and version; private responses additionally include profile. Do not mix versions on a page.

## 5. Attempt state machine
`DRAFT → RECOGNISING (image only) → NEEDS_CONFIRMATION → CONFIRMED → CALCULATING → COMPLETED`. Manual input skips RECOGNISING. User edits return to NEEDS_CONFIRMATION. Terminal exits: CANCELLED, FAILED, EXPIRED. Recoverable error metadata may accompany a nonterminal state while the original deadline remains valid.

New attempt eligibility = authenticated/authorised adult + ACTIVE central free programme + serverNow < programmeEndsAt. Creation is one trusted transaction. Store creation timestamp and `expiresAt = createdAt + 60 minutes`.

Completion checks ALL of: valid ownership/current account permission; attempt created during valid free eligibility; open nonterminal attempt; serverNow <= expiresAt; confirmed current revision; acceptable current safeguards/consent; rule/catalogue not revoked; required mapping completeness. Do not OR in current free eligibility as an alternative to the deadline. Inclusive equality follows S01; changing it needs an explicit policy revision.

The final commit must still satisfy the deadline. A job started before expiry but finishing afterward cannot silently save. A safety/consent change during calculation blocks commit and returns an explicit re-confirmation requirement within the remaining deadline, otherwise a new attempt. Hold the expected revision through finalisation so concurrent edits/cancel cannot race a save.

An already COMPLETED request replays the stored result without new calculation after verifying access; a changed request under the same idempotency key is a conflict. A cancelled/expired result worker cannot re-open an attempt. Retries and request keys are scoped to profile + operation + canonical request hash. Do not create a new daily quota.

A free attempt validly created before purchase may complete under the same free authorisation/deadline if account access remains valid; this does not grant paid meal access or a new attempt. Any paid policy that explicitly invalidates such drafts takes precedence and must return a reason rather than silently switching scoring profiles. New paid attempts remain outside this release.

## 6. Recognition, data and reproducibility
Carry forward S01 image rules, catalogue provenance, confidence bands, mandatory confirmation and nutrition logic in `06_Calculation_Contract_v2.0.md`. Recognition proposes foods/portions/preparation only; server-approved catalogue values produce nutrient totals. Treat missing/malformed confidence as LOW. Nutrient data from a model is not a permitted calculation source.

All real calculations use the fixed free policy. No preview-selected CKD targets, weight fallback, programme-string selection or prototype model-generated nutrients. The backend returns formatted rows, reason codes, verdict and approved actions from the same rule version. Mobile does not derive any of these.

Freeze catalogue and numeric versions on the attempt so mapping cannot drift; recheck revocation before commit. Store exact user-approved items, quantities, versions, normalized scoring totals, display snapshot, safety mode and immutable original result. Store enough provenance to reproduce the record without re-running image recognition. Historical edits create linked drafts; no silent overwrite or recalculation.

Facts-only means totals and limitations only: no numeric targets, traffic-light judgement, suitability vocabulary, suggestions or glucose impact. Completeness still matters: do not fill absent nutrient values with zero. The clinical/data owner must approve the minimum computable facts dataset; absent approval or required fields yields no result, not an inferred safe result.

## 7. Responses and errors
| Code | Required mobile behaviour |
|---|---|
| `FREE_NOT_STARTED` | Existing activation route; no local timer start |
| `FREE_EXPIRED` | New analysis locked; history/previews accessible under their policies |
| `ATTEMPT_EXPIRED` | Explain this check timed out; create a fresh one only if newly eligible |
| `ACCOUNT_INELIGIBLE` / `PROFILE_FORBIDDEN` | No analysis or private data; account/support route |
| `SAFEGUARDS_REQUIRED` / `CONSENT_REQUIRED` | Capture/confirm explicit information; no result yet |
| `REVISION_CONFLICT` | Fetch current draft and explain; never overwrite concurrent changes |
| `FOOD_MAPPING_REQUIRED` / `NUTRIENT_DATA_MISSING` | Edit/replace/remove or stop; no invented facts |
| `IMAGE_INVALID` / `NOT_FOOD` / `AMBIGUOUS_MEAL` | Retake, gallery or manual entry; retain permitted draft |
| `SERVICE_UNAVAILABLE` | Retry/cancel within original deadline; no sample meal |
| `PREVIEW_UNAVAILABLE` / `SCHEMA_UNSUPPORTED` | No partial/unrelated preview; show other valid choices/return |
| `ORDER_PENDING` | Track existing activation/order, no additional base checkout |
| `POLICY_REVOKED` | Stop new/finalised use of the revoked version; explicit explanation |

Use project-standard HTTP conventions; proposed mapping is 403 ownership/eligibility, 409 revision/state conflicts, 410 expired draft, 422 field/content validation and 503 service unavailability. Result JSON retains typed codes, not only human-readable text.

## 8. Commercial actions
Resolve against authoritative selected-profile paid/order state at display and action time. An active paid user remains in existing paid care even with an unresolved renewal. Pending new order replaces all purchase origins with Track activation. Cancellation request alone does not release the lock. Terminal cancellation/refund re-resolves; payment does not renew free access. Only catalogue-confirmed benefits are presented, and there is no inference that a preview category necessarily has a live plan.

## 9. Security, privacy and observability
Authenticated APIs and object-level checks for every attempt/result; private, short-lived upload grants; MIME/byte/size/orientation checks; EXIF/location removal; approved malware/security pipeline. Client contains no provider credentials. Model prompts carry necessary image/note only, not diagnosis, name, age, phone, other health records or browsing interest. Content read logs must not turn a browsing category into a diagnosis.

Retain proposed S01 deletion: <=15 min after terminal success/cancel/failure; abandoned storage hard TTL <=24 h; client cache cleared on terminal action/next launch. Delete consistently in queues, temp files and storage; get provider retention terms approved. These are proposed product periods, not legal compliance certification.

Product telemetry uses mode, generic success/failure and timing only. Treat care-interest IDs as sensitive health-interest information: do not send them or food/medical/safeguard content to general analytics without explicit privacy approval. No raw images, food names, nutrient values, notes or diagnoses in logs. Use opaque correlation IDs for debugging. Audit protected content approvals, access failures, rule versions and finalisations.

## 10. Release controls and future paid extension
Separate flags for preview publication, new real attempts, general guidance, estimated glucose impact and future paid clinical analysis. Keep future condition-specific actual analysis unavailable in v2.0; a flag is not clinical approval. Existing reference profile values require recorded Medical approval. Paid extension can reuse typed blocks, food mapping, inputs, history and service interfaces; its clinical assessment, multi-condition resolution, personal targets and staffed service are separate work.
