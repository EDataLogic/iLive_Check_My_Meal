# Check My Meal — complete replacement handoff v2.0
**24 September 2026 · iLive · Product / UX / Backend / Mobile / QA / Clinical / Operations**

## Start here
Use `01_Git_Issue_REPLACE_BODY_v2.0.md` as the proposed replacement body for the existing Check My Meal issue. Keep its existing issue number, history, assignees and parent epic. No live GitHub issue was changed: the connector returned no accessible repositories. The source issue number is not verified.

Open `UX/Check_My_Meal_v2.0.html` in a browser for the interactive review mock. The HTML is self-contained and uses illustrative fixtures only; it is not an app build or a nutrition service. `UX/Check_My_Meal_Mock_Screens_v2.0.pdf` contains annotated screen states; `UX/PNG` contains the individual images. Open `Workflows` for the flow diagrams and editable Mermaid sources.

## Product decision carried into this revision
One feature, two clearly separated activities:
- **Your meals:** genuine, user-confirmed meal input and the retained general heart-health calculation, or facts-only output under the safety policy.
- **Explore care:** category-specific educational previews showing the nutrition topics that relevant care can consider. A browsing choice is not a diagnosis, enrolment or a scoring-rule selection.

The review does NOT activate condition-specific judging of the user's actual meal. Those clinical rules remain a separately approved future paid-care scope. Your Diet is NOT changed by this pack.

## Status and approval
This is an implementation-planning proposal based on the user's latest direction. It is not a record of medical, privacy, commercial or final UX sign-off. Draft category content is `productionEnabled=false`; only approved, published bundles may render in production. Backend endpoint names and schemas are proposed contracts to align with existing services, not claims that APIs already exist.

## What to replace
Replace the prior Check My Meal issue body and UX reference with this matching v2.0 set after the relevant owners accept the revision. Do not append the old contradictory issue underneath. Do not use the two earlier generated infographic images as implementation specifications: their one-meal-per-day quota, personal-looking calorie balance, automatic category-to-scoring sequence and expanded paid-service promises are not carried forward.

## Files
| File | Reader / purpose |
|---|---|
| `01_Git_Issue_REPLACE_BODY_v2.0.md` | All stakeholders; complete scope and acceptance baseline |
| `02_Backend_and_Mobile_Contract_v2.0.md` | Engineering; server ownership, APIs, states, permissions, caching |
| `03_Workflows_v2.0.md` and `Workflows/` | Product, UX and engineering; flows and state transitions |
| `04_Content_Catalogue_v2.0.json` | Content/engineering; six disabled draft preview profiles |
| `05_Content_Governance_v2.0.md` | Medical, Product and Content; reuse decisions and release gates |
| `06_Calculation_Contract_v2.0.md` | Backend/Data/Clinical; retained v1 general rules with explicit precision correction |
| `07_QA_Acceptance_v2.0.md` | QA; executable test descriptions, not test results |
| `08_Execution_Work_Packages_v2.0.md` | Delivery planning; owners, outputs, dependencies and completion conditions |
| `09_UX_Screen_Map_v2.0.md` | UX/Mobile; templates versus state snapshots and interaction notes |
| `10_Source_and_Change_Log_v2.0.md` | Traceability; source versus proposed changes |
| `Backend_Examples/` | Illustrative payloads and preview JSON Schema |
| `Reference_ONLY/` | Original issue and verified prototype excerpts; not the new implementation baseline |
| `Validation_Report.json` | Artifact checks actually run; not production clinical/API/QA validation |

**No duration or engineering-effort commitment is made in this package.**
