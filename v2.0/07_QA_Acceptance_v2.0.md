# QA acceptance matrix — v2.0

**46 proposed scenarios. Not production tests already executed.** Combine with the exact numeric examples in `06_Calculation_Contract_v2.0.md`, approved security tests and supported-device regression.

| ID | Area | Given / action | Expected |
|---|---|---|---|
| Q01 | Mode separation | Open from Free Home | Your meals is default; Explore care visible; no mandatory category choice. |
| Q02 | Mode separation | Browse every one of six previews then check identical meal | Same real policy, numeric output and safety decisions for the same confirmed inputs and versions. |
| Q03 | Mode separation | Inject previewCategoryId/ruleId/safetyMode in attempt/complete | Schema rejects unsupported overrides; no clinical mode switch. |
| Q04 | Content | Switch diabetes → kidney while old response is delayed | All preview blocks update together; late diabetes payload cannot overwrite kidney. |
| Q05 | Content | Inspect six profile bundles | Correct priority topic list, source identity, preview label and no automatic enrolment. |
| Q06 | Content | Unapproved/incomplete/revoked bundle | Not published; no partial warnings or unrelated-category fallback. |
| Q07 | Content | Unsupported required block/schema | Neutral unavailable/update state, not a page missing a required warning. |
| Q08 | Content | Expired free programme | Approved preview remains readable without reopening real checks. |
| Q09 | Content | Multiple-concern user browses a category | No template combination/diagnosis write; clear individual-review context. |
| Q10 | Inputs | Camera success, high confidence | Explicit mapped item/portion confirmation still required. |
| Q11 | Inputs | Camera permission denied permanently | Settings guidance plus gallery/manual alternatives; no forced camera loop. |
| Q12 | Inputs | HEIC/photo orientation and unsupported/corrupt/oversized files | Normalise valid formats; reject invalid content; no result from a sample meal. |
| Q13 | Inputs | No food/blur/multiple plates | Explain and offer correction/retake/manual; no fabricated data. |
| Q14 | Inputs | Medium/low/malformed confidence | Highlight uncertain items; low/malformed cannot be accepted unmapped. |
| Q15 | Inputs | Manual search → preparation → portion → add/remove | Approved records only; zero/missing quantity blocked; confirmation invalidated on edit. |
| Q16 | Inputs | Required nutrient/free-sugar mapping absent | No zero fallback or traffic-light result; resolve/replace/remove or stop. |
| Q17 | Safety | Each unanswered mandatory safeguard | No submit; no default No/None; field-level recovery. |
| Q18 | Safety | Kidney/dialysis YES or NOT_SURE | Facts-only; no targets, verdict, suggestions or glucose impact. |
| Q19 | Safety | Prescribed diet YES/NOT_SURE or pregnancy YES/PREFER_NOT_TO_SAY | Same facts-only suppression; historical output unchanged. |
| Q20 | Safety | Insulin/sulfonylurea YES/NOT_SURE and reduction suggestion | Retained approved guard applied; no meal skipping or prohibited reduction. |
| Q21 | Safety | Allergy/preference excludes all suggested candidates | No conflicting food suggestion; only permitted neutral observation. |
| Q22 | Safety | Change safety or consent while worker calculates | Finalisation rechecks; stale confirmation cannot commit; no extension of deadline. |
| Q23 | Rules | Same food/quantity/catalogue/rule versions across input routes | Same totals, normalized scoring, verdict and wording. |
| Q24 | Rules | Source energy/fibre/carbohydrate exact thresholds and adjacent rounded values | Matches retained calculation contract with half-up scoring and strict comparison. |
| Q25 | Rules | Optional glucose-impact flag off or prerequisites missing | No output or personal glucose prediction. |
| Q26 | Entitlement | Programme available not activated; under18; suspended account | New checks blocked with distinct reason; browsing eligibility uses its own policy. |
| Q27 | Entitlement | Request at programmeEndsAt or expired deep link | No new attempt; history/preview remain separately governed. |
| Q28 | Entitlement | Attempt created before free expiry; complete at <=60-minute deadline | Exactly one completed result with original rule/version. |
| Q29 | Entitlement | Attempt older than60 minutes while free programme active | Old attempt rejected; eligible user may create a new one. |
| Q30 | Entitlement | Worker starts before deadline but commit is after deadline | No late completed result; clear timeout and cleanup. |
| Q31 | Concurrency | Double-tap complete; retry after success; changed request same key | Same result for identical replay; conflicting key payload rejected. |
| Q32 | Concurrency | Concurrent edit/cancel/complete or late recognition callback | Revision/terminal-state guard prevents stale save or resurrected attempt. |
| Q33 | History | Multiple successful checks same meal slot | Separate timestamped checks, no implied consumption or daily total. |
| Q34 | History | Read old result after changing category/rules/preferences | Original protected snapshot; no silent recalculation. |
| Q35 | History | Edit old result after expiry | No new draft unless newly authorised; source history stays immutable. |
| Q36 | Profile | Switch linked profile while content/meal response in flight | No cross-profile result, preferences, history or checkout state. |
| Q37 | Commerce | Pending order from any entry origin | Track activation only; no duplicate base purchase or renewed free access. |
| Q38 | Commerce | Active paid + renewal pending | Existing paid care; no forced Free Home or invented paid meal entitlement. |
| Q39 | Commerce | Cancellation requested vs confirmed terminal; plan unavailable | Lock remains until terminal; unavailable benefits never promised. |
| Q40 | Privacy | Inspect app bundle, provider requests, logs and analytics | No provider secret/identifiers/diagnoses/preview interests/food data leaked outside approved boundaries. |
| Q41 | Privacy | Complete/cancel/fail/abandon then verify cleanup | Deletion periods and caches/queues enforced; storage TTL cannot authorise attempts. |
| Q42 | Failure | Offline or catalogue/entitlement outage | No fail-open analysis; valid private history/cache policy only; recoverable navigation. |
| Q43 | UX | Native screen reader, large text and narrow screen | Logical focus, readable labels, adequate controls and no colour-only state. |
| Q44 | Regression | Your Diet, assessment, lab report exception, existing paid flow | No changes from this issue; no new clinical queue or automatic review. |
| Q45 | Release | Attempt deployment with sample/draft fixture data | Build/release pipeline rejects productionDisabled, fixtureOnly and missing approvals. |
| Q46 | Workflow | Back from plan/preview/real result to hub | Correct tab, selected preview and history preserved; no meal lost or quota consumed. |
