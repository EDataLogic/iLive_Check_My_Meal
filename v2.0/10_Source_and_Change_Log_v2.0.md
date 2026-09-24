# Source register and change log — v2.0
**24 September 2026. Source review and design proposal; not clinical validation.**

## Sources
- **S01** `ILive_Free_Check_My_Meal_MVP_Git_Issue.md`, recovered earlier issue, 1,369 lines. Preserved unchanged under `Reference_ONLY`. Provides the general free analysis, catalogue, safety, history and privacy baseline.
- **S02** `iLive-Updated-Prototype-Tech-Handover-v1.0 (1).zip`, source member `iLive-Updated-Prototype-Tech-Handover-v1.0/01-prototype-source/ILiveConnect.jsx`. Archive SHA-256 `f346bb11ea7771632c1ce3a6a315f74fc8e40a0264698096e0eb231588a95ee5`, checked against supplied checksum. Source lines 393–554 define six meal profiles and selection; 556–574 derive targets; 651–745 contain analysis/ranking/suggestions; 6687–6693 reads programme/condition context; 7101–7171 renders opening cards; 7279 onward is the separate Diet module.
- **S03** `iLive_Free_Home_Consistency_Review_v1.0_2026-09-23.zip`, Check My Meal addendum and shared-state contract. Proposed fixes for attempt deadline, precision, cross-origin CTA and mock coverage.
- **S04** `iLive_Free_Home_Configurable_Care_Programme_Discovery_MVP_Feature_v1.1.md`, FR-07–11. Provides price/benefit authority, profile/order priority, no overlapping checkout, payment/readiness distinction.
- **S05** User's current discussion: category-specific educational previews, real-meal checking separate, backend-first configurability, Check My Meal only, Your Diet later.

Sources with statements labelled “approved” were requirements, not evidence of a recorded sign-off. Live GitHub issue state/comments and current production APIs remain unverified. Connected repository listing returned no accessible repositories; no write was attempted against an unidentified issue.

## Explicit changes (proposal, not silent source correction)
| Change | Source behaviour | Proposed v2.0 disposition |
|---|---|---|
| Two activities | S01 general real checks only; S02 automatic conditional rules | Your meals + optional Explore care, explicitly separated |
| Categories | S02 programme/condition selection affects analysis | Six draft preview profiles; selection is browsing only |
| Condition logic | S02 string matching, weight defaults, combined precedence | Not ported to free actual analysis |
| Preview sections | S02 personal-sounding headers, warnings, longevity claims | Category-labelled educational content with Medical approval |
| Numeric budgets | S02 daily kcal and remaining balance | Removed from free preview and actual hub |
| Meal limits | Earlier generated art introduced one meal/day | Discard; S01 no invented daily quota retained |
| Safety mode | S01 facts-only suppression partly implicit | Explicit suppression of comparisons, verdicts, suggestions and glucose impact |
| Attempt expiry | S01 entitlement-OR ambiguity vs 60-minute limit | S03 correction: every draft must satisfy its original deadline |
| Precision | S01 scoring normalisation then “Round only for display” | S03 correction: one scoring-normalised copy, separate UI formatting |
| Food terminology | S02 added sugar/salt; S01 free sugar/sodium | Proposed label harmonisation explicitly marked for approval |
| Care CTA | S01 general plan link | S04 profile/order-aware resolver across every origin |
| Mocks | S01 limited interaction states; prior generated infographics inconsistent | Versioned precise HTML, individual PNGs and annotated PDF from one state dataset |
| Future reuse | Condition-specific paid analysis not defined | Reuse UI/data primitives only; clinical service and approval remain separate |

## Supersession
When accepted, v2.0 replaces the earlier Check My Meal issue and its mock reference, not the entire six-card review. Retain original issue identity/history. The two unversioned/generated infographic images are non-authoritative and are not included as current mocks. Never import their invented daily quota, personal calorie balance, automatic care-mode scoring or unsupported benefits into implementation.

## Validation limits
This pack verifies source identity, file readability, mock generation, selected UI navigation and artifact consistency. It does not verify production code, execute API integration tests, certify nutrient accuracy or obtain clinical/commercial approvals. QA scenarios are proposed until implemented and run by the team.
