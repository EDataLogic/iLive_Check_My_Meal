# Content, prompts and approvals — v2.0

## 1. Source reuse, not automatic publication
The supplied prototype has six FRX_RULES profiles and separate Your Diet templates. In this issue we reuse the Check My Meal profile identity and educational topics; Your Diet is not modified. The prototype's exact conditional rendering, targets and medical assertions are preserved in S02/reference excerpts. They are not treated as validated production nutrition advice.

The JSON catalogue in this pack is a **proposed editorial adaptation**, not verbatim prototype copy. All six entries are DRAFT and productionEnabled=false. Medical must review topic statements, warnings, suggestions and every label before release. No individual calorie/protein/potassium/phosphate/fluid target is published in a preview. Examples of calorie budgets are deliberately omitted rather than mistaken for the user's needs.

## 2. Section-by-section disposition
| Prototype section | v2.0 treatment | Reason / classification |
|---|---|---|
| Your Food Rules / rules.aim / heavier | Food topics for this care area, category-labelled, same source priority concepts | Proposed wording prevents a preview from sounding like the user's prescription |
| Important for you / hardWarnings | Important context, optional approved category content | Retain relevance but don't publish medication timing or blanket directives automatically |
| Today / kcal left | Your meals entry and history; no budget or daily intake tally | Existing free issue did not define a complete intake ledger or individual targets |
| Your Longevity List | Nutrition topics to discuss / approved educational ideas | Remove unverified longevity outcomes and absolute “safe for” claims; not a food prescription |
| Meal result / targets / verdict | Existing general profile or facts-only safety mode | Preview does not select clinical scoring |
| Add/cut suggestions | Retained deterministic actual-meal rules after safeguards and approval | Not copied from a browsed category's first three suggestions |
| Likely blood-sugar rise | Not shown in mocks; gated separate heuristic in inherited contract | No claim of a personal glucose prediction |
| Weekly dietitian review footer | Plan-specific benefit only when commercially verified | Free preview creates no review/assignment |
| Your Diet Eat/Avoid/Fruit/Three Rules | Unchanged, outside issue | Separate subsequent feature review |

## 3. Terminology delta
Source previews show “Added sugar” but S01 actual calculation uses canonical free sugar. Proposed preview copy uses **Free sugars**, pending Medical/Content confirmation. This is an explicit editorial harmonisation, not a claim that added sugar and free sugar are identical. Keep a short approved glossary. Source “Salt” maps to **Salt (sodium)** in educational labels; numeric rows are **Sodium (mg)**, never grams/milligrams of salt silently relabelled as sodium. Prototype “Phosphate” is preserved as a topic; numeric phosphorus/phosphate semantics require a separate clinical/data contract before future condition-specific scoring.

## 4. Candidate category coverage
Longevity/prevention; diabetes; hypertension; kidney disease without dialysis; kidney disease on dialysis; recovery. Six profiles are content capability, not six launch approvals or six purchasable programmes. Only approved entries render. No arbitrary automatic priority if several diseases are present. The user is exploring one subject; actual health context belongs in the protected safeguards/clinical record flow.

## 5. Conditions and multi-condition caution
The preview is not medical advice tailored from a single interest selection. Do not infer an actual condition from browsing, a glucose/BP value, an assessment result or a programme-name substring. No automatic mixing of kidney/diabetes/recovery food lists. Generic explanation: **“More than one health concern? An individual plan considers them together.”** A clinical service must validate appropriateness before any future personal targets or disease-specific meal recommendations.

## 6. Recognition prompt contract — retained service boundary
Source recognition returns nutrients in the prototype; S01 explicitly replaces that with identity/portion/preparation candidates and user confirmation. Keep provider credentials server-side. The required output schema is candidates + valid confidence + input classification. No authoritative nutrients, verdict, diagnosis, recommendation or clinical rule selection may be supplied by the model. Image text is data, not an instruction to the service. Validate every model output against allowed fields and map to the approved food catalogue. Missing/malformed confidence becomes LOW. Do not send preview category, health conditions, personal identifiers or protected preferences to the provider.

No LLM writes published category advice at runtime. Any draft-writing assistance passes the same human publication process. The app renders an approved complete bundle. Static text may vary by approved category but not by an invented patient inference.

## 7. Approval register (not yet completed)
| Gate | Required owner | Evidence required | Blocks |
|---|---|---|---|
| G01 Preview editorial scope, labels and six-category launch subset | Product + Clinical Nutrition | Approved bundle/hash/locale; target audience and exclusions | Each preview publication |
| G02 Numeric general profile and safety logic | Medical + Data | Signed version, reference provenance, expected outputs | General real-meal guidance |
| G03 Food catalogue/recipes/preparation/portion/free-sugar completeness | Clinical Nutrition + Data | Governed release and completeness checks | All actual calculation |
| G04 Consent, provider processing, retention and protected analytics | Privacy + Security | Approved data map, consent and deletion tests | External image processing/production storage |
| G05 Actual plan mapping and nutrition inclusions | Business + Operations | Active catalogue mapping and approved wording | Care CTA/benefit claims |
| G06 Supported blocks, accessibility and copy | UX + Mobile | Approved mocks; iOS/Android tests | Mobile release |
| G07 Central entitlement, completion window and preview-after-expiry | Product + Backend | Approved policy and test results | Release |
| G08 Historical free and future paid boundary | Product + Clinical + Backend | No silent clinical-rule migration; clear handling of paid transition | Release/integration |
| G09 Glucose-impact heuristic | Medical | Exact formula/metadata/wording/vector approval | Optional output only; disabled otherwise |

The team may launch approved previews while gated analysis stays unavailable, but must label that release honestly; it is not delivery of the complete feature's meal-analysis acceptance criteria.
