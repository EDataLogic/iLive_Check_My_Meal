# [Feature][Free Home] Check My Meal — Real Meal Insights + Care Nutrition Preview (v2.0)

**Version:** 2.0 · **Date:** 24 September 2026 · **Status:** Proposed replacement; ready for team breakdown, subject to listed release approvals.  
**Parent:** Preserve the existing Free User / Seven-Day Heart Health Check epic and this feature's issue identity. Live issue number not verified.  
**Owners:** Product, UX, Backend/API, React Native, Data/Clinical Nutrition, QA; Privacy/Security and Commercial/Operations for their gates.  
**Suggested labels:** feature, free-user, check-my-meal, backend-driven, nutrition-preview, ux, clinical-review-required.

## 1. Background and problem
The business prototype offers food guidance that changes with a programme or recorded condition. Our earlier free-MVP issue deliberately narrowed real meal analysis to a single general heart-health profile. That protects the calculation boundary, but by itself does not explain to a person interested in diabetes, blood pressure or kidney care how iLive's nutrition support could be relevant to them.

The user's updated product direction is to make that relevance visible before purchase. We should reuse suitable category-specific material from the prototype, without implying that browsing a category constitutes diagnosis, enrolment or an individually prescribed diet. We must also avoid a misleading experience where a diabetes-themed header sits above a meal result still calculated using unrelated general rules.

## 2. Objective and user value
Give free users two useful choices within **Check My Meal**:
1. **Understand an actual meal:** photograph or manually enter food, correct and confirm it, and receive reproducible estimated nutrition information with general guidance where permitted.
2. **Explore relevant care:** browse an educational nutrition preview for a care interest, understand what topics individual support can consider, and reach the existing eligible plan journey.

For users, the difference must be visible without reading a disclaimer: **“Your meals” is about food they submitted; “Explore care” is about services and nutrition topics they are browsing.** The business benefit is a clearer, relevant introduction to paid care; increased retention/conversion is a hypothesis to measure, not a guaranteed outcome.

## 3. Scope boundary and non-goals
This issue updates Check My Meal only. Do not redesign Your Diet, heart assessment, login, lab services, paid Home, payment processing or device pairing. Do not add a new master epic.

Included: feature hub; six draft category profiles; reusable previews; shared state-aware care CTA; consent and safety/preferences; camera/gallery/manual input; mapping and confirmation; general/facts-only results; immutable history/revisions; active/expired/error states; backend configuration and release governance.

Excluded: condition-specific real-meal scoring; patient-specific calorie/protein/mineral/fluid targets; “kcal left” budgets; daily intake totals; automatic diagnosis or programme assignment; dietitian review/callback/queues; new paid meal entitlement; measured/predicted personal glucose; live meal coaching; medication advice; a new content-admin application. Existing content administration or a controlled publishing pipeline may be used.

## 4. Two modes — one unambiguous contract
| Dimension | YOUR_MEALS | CARE_PREVIEW |
|---|---|---|
| Purpose | Actual confirmed meal insight | Educational exploration of care nutrition |
| Input | Confirmed foods, portions, preparation, meal slot | User-selected browsing category |
| Rule selection | Backend-owned general rule profile, with separate safety mode | Backend-approved content bundle only |
| Result | Estimated facts and permitted general observations | Nutrition topics, general context and eligible care benefits |
| Persists | Authenticated meal record and its exact versions | Optional protected browsing preference; not a diagnosis |
| May change actual meal targets? | Only an approved analysis-policy release | **Never** |
| After Day 7 | History only; bounded in-flight exception | Approved previews remain readable |

A preview category must not be a parameter accepted by the meal-calculation request. The service must reject an attempted category/rule override, rather than trust a hidden mobile field. Selecting a preview never creates or edits a medical condition, assessment answer, free episode, subscription, clinician task or timer.

## 5. Screen and navigation behaviour
### CM-01 — Home entry and hub
Retain the Home card title **Check My Meal**. Suggested subtitle: **“Understand your meal. Explore nutrition support.”** Open the feature hub on **Your meals**, with **Explore care** equally discoverable. Do not force a care-category selection before a real check. Preserve tab, scroll and selected preview on return. Do not redesign adjacent Home features.

The Your meals tab explains that the live free check uses general heart-health rules, shows an eligible **Check a meal** action and **Past insights**, and names the active free window. The Explore care tab opens an optional category picker and the matching preview. A user may choose **General heart-health information / Not sure**; do not infer a diagnosis or programme from this action.

### CM-02 — Category previews
The source has six food-rule profiles: longevity/prevention, diabetes, hypertension, kidney disease without dialysis, kidney disease on dialysis, and recovery. Support these as six catalogue entries; publish only entries with complete Medical/Product approval. Their presence in the prototype is not launch approval, and absence of a paid plan must not be hidden behind a fake purchase button.

Every preview displays:
- A permanent **CARE PREVIEW · NOT YOUR PERSONAL DIET** label and selected category.
- **Food topics for this care area**, replacing the personal-sounding “Your food rules”.
- Relevant predefined nutrient topics, short explanatory content and approved important context.
- **How individual targets are set**, not the prototype's numeric daily allowance.
- An approved care-benefits panel and the shared state-aware action.

Update the complete preview atomically when the category changes. Do not leave another category's warnings, suggestions, CTA or headings on screen. When a profile has no warning, omit its warning component rather than show an empty danger card. No automatic combination of condition templates. A multiple-concern message explains that an individual plan considers conditions together; it does not collect a new medical diagnosis.

### CM-03 — Actual meal journey
From Your meals: authorise access → first-use explanation/consent → complete the existing safety/preferences → choose meal slot and camera/gallery/manual entry → recognition where applicable → user correction/confirmation → service calculation → automatically saved result.

Retain all three input routes. Native camera/gallery permission denial offers manual entry. Every food, portion and preparation must map to the approved catalogue before calculation; a photo's confidence never bypasses confirmation. Low confidence/unmapped items require correction, replacement or explicit removal. No typical meal or missing-nutrient-zero fallback.

### CM-04 — Results and history
Label results **Estimated meal insight · General heart-health reference** or **Estimated nutrient facts only**. Do not put the currently browsed condition above an actual result or relabel old results when the preview changes. The result template may show nutrient totals, rule-derived observations and at most three permitted actions. Historical results use the same template in immutable mode with the original date and versions.

Keep the existing single general profile and its clinical release gates. Facts-only mode has no colour verdict, suitability classification, target comparison, add/reduce/swap advice or glucose-impact category. This revision makes that suppression explicit. Changing safety answers invalidates any unfinished confirmation; an old completed record is not recalculated silently. Editing a completed record creates a new authorised draft revision and retains its predecessor.

### CM-05 — Today, calories and educational examples
Keep meal-slot selection and timestamped history, but do not copy the prototype's daily allowance, remaining calories, daily completion ring or four-check limit. Source rules permit multiple checks in the same slot; a check is not proof the user consumed that meal. For this MVP there is no complete daily intake ledger. Calories may appear as an estimate for the confirmed meal. Any future demo must be clearly labelled, kept out of personal history and never used after real analysis fails. Numeric preview examples are deferred in this revision to avoid a personal-budget implication.

## 6. Backend-first ownership
The backend supplies entitlement, preview category visibility and ordering, complete published content, typed sections, action destinations, safety decisions, catalogue mapping, calculations, result wording and versioned history. React Native supplies an allowlisted component renderer, input capture, native permissions, accessibility and navigation.

Content edits, categories using existing components, copy, approved ordering and commercial action rules should not require app releases. A new interactive component/schema capability still requires a mobile release and compatibility checks. Do not download executable rules or arbitrary HTML. Extend current services rather than mandate new microservices.

Separate `previewCategoryId` from `analysisPolicyId`. Preserve server rules independent of the user's preview choice. The detailed API, security, caching and state contracts are in `02_Backend_and_Mobile_Contract_v2.0.md`.

## 7. Eligibility, attempt deadline and expiry
- New checks: authenticated/authorised adult profile with the central free programme ACTIVE and server time before its end. No independent timer; no new one-meal-per-day limit.
- Every attempt must be created server-side while eligible and expire 60 minutes after creation. Completing a draft requires valid ownership/account permission, an open attempt, a current confirmed revision and server time **at or before** that deadline.
- An attempt created before Day 7 ends may finish once within its original deadline. Continued free eligibility does not extend an old attempt; it allows a new authorised attempt.
- Repeated finalisation of a completed attempt returns the same stored result. Retrying does not create duplicate meals. Storage TTL is not authorisation.
- After expiry, lock new camera/gallery/manual analysis; retain history and approved previews. Purchasing/pending hardware does not restore free analysis. Active paid access is determined by the existing paid system, not this issue.
- Account suspension/ownership failure overrides completion grace. After a safety rule or consent version changes, recheck before finalisation; reject revoked rule versions and do not invent a replacement result.

## 8. Care conversion without duplicate purchase
Every CTA from preview, result, history or expiry resolves against the selected profile: active paid → existing care; unresolved order → track status; eligible/no pending plan → currently available plans; unresolved/loading → retry/read-only. Revalidate at the service boundary before checkout. Preserve the source context for back navigation.

Do not promise weekly dietitian review, instant response, continued meal analysis or a nutritionist assignment unless included in a confirmed available plan. Payment, fulfilment and clinical activation are separate. Preview category selection does not map one-to-one to a purchasable programme unless the business catalogue explicitly says so.

## 9. Privacy, safety and content governance
Retain the source security contract: backend-only provider credentials; authenticated private uploads; format/size/content validation; EXIF removal; recognition provider receives only necessary image/note; no health or food data in general analytics. Retain structured completed records under the approved account/clinical policy.

Preserve the source's proposed terminal image deletion within 15 minutes and abandoned-storage hard 24-hour TTL, subject to Privacy/Security approval. Neither permits attempt completion after 60 minutes. Complete deletion must cover retry workers/cache/object storage as well as the app.

The six content profiles in this pack are editorial drafts with production disabled. Preserve source copy in reference records, but do not publish its disease directives, medication timing or absolute longevity/safety claims unchanged. Medical approval is a real release gate; a disclaimer is not a substitute. Detailed care-content policy is in `05_Content_Governance_v2.0.md`.

## 10. Acceptance criteria
1. Both hub activities are accessible; category exploration is never a compulsory gate for real checking.
2. Each published preview shows its category and preview label consistently across all visible sections.
3. A browsing choice never changes diagnoses, episode/timer, analysis policy, safeguards or completed records.
4. Identical real-meal inputs and versions produce identical output across all six preview selections.
5. New checks obey central entitlement; a server-authorised pre-expiry attempt can finish once before its original deadline.
6. A 61-minute-old draft cannot be revived simply because Day 2 access is active.
7. All input paths require mapped, user-confirmed foods/quantities; no fabricated nutrients or substitute meal.
8. Facts-only output suppresses comparisons, verdicts, suggestions and glucose-impact information.
9. History is immutable; revision is a new authorised draft; examples never enter it.
10. No daily calorie budget, unsupported daily quota, condition-specific actual verdict or personal target is introduced.
11. All commercial origins respect active/pending/eligible/unresolved selected-profile state.
12. A complete valid content bundle renders; unavailable/revoked/unsupported content fails safely without unrelated-category substitution.
13. Late responses cannot show another profile's health record or overwrite a newer category selection.
14. Content and rule changes are independently versioned and auditable; revoked versions cannot authorise new work.
15. Existing Your Diet, assessment, labs, paid Home and device journeys pass regression testing.
16. Screen-reader, large-text, contrast, non-colour status and native-permission recovery are verified on supported iOS/Android devices.
17. Required Medical/Product/Privacy/Security/Commercial approvals and implementation tests are recorded before release.

## 11. Delivery and evidence
Use the accompanying backend contract, draft content, inherited calculation contract, workflows, screen map, mocks and QA matrix as one v2.0 set. The work-package plan separates content governance, server configuration, meal processing, mobile, commerce and integrated QA without implying a time estimate.

**Definition of done:** agreed scope and mappings; working production integrations; reviewed content and clinical policy; reproducible results; enforced safety/expiry; validated privacy/security; approved UX; completed automated/manual QA and regression evidence. Supplied mocks and artifact checks are NOT this evidence.

## 12. Sources and change control
S01 is the previous Check My Meal issue; S02 is the supplied prototype `ILiveConnect.jsx`; S03 is the cross-feature review; S04 is Care Programme discovery v1.1. Full identifiers and proposed changes appear in `10_Source_and_Change_Log_v2.0.md`.

This is a proposed replacement, not an append-only addendum. On acceptance preserve the issue identity, link this versioned pack, retire old mocks and record the approvers. Your Diet will be reviewed separately.
