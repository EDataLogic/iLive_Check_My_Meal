# Retained actual-meal calculation contract — v2.0

**Source:** S01, previous `ILive_Free_Check_My_Meal_MVP_Git_Issue.md`, sections 10–19. Original section numbering is retained for traceability. This is inherited proposed clinical behaviour, NOT independent medical validation.

**Scope:** actual free-user meal checking only. Category preview content never selects these rules. Publication of preview content does not authorise any real calculation. All source medical/data release gates still apply. No paid condition-specific formulas are enabled.

**Explicit v2.0 changes:** resolve the contradictory “Round only for display” wording in favour of the source's explicit one-time scoring normalisation; explicitly suppress all comparisons and glucose-impact output in facts-only mode. The source thresholds, general reference amounts, ranking and test examples are otherwise retained unchanged. Source references to “approved” mean approval is required, not that an approver has signed this pack.

**Important:** the source glucose-impact heuristic remains feature-disabled until the exact formula, wording and vectors are approved. This package does not validate it or present it in the mocks. User-specific nutrition advice, medication dosing and disease-specific targets are outside this revision.

---

## 10. Mandatory safety and preference gate

This is a short first-use guardrail for meal guidance. It is **not** the detailed heart assessment and does not change the numeric rule targets.

### 10.1 Required fields

| Field | Allowed values | Purpose |
|---|---|---|
| Diet preference | Vegetarian / Eggetarian / Non-vegetarian / Vegan | Filter any specific add/swap candidate |
| Allergies/intolerances | Nuts / Milk / Egg / Soy / Gluten / Seafood / Other / None | Suppress conflicting suggestions; user must explicitly select `None` if applicable |
| Kidney disease or dialysis | Yes / No / Not sure | Determines whether general recommendations may be shown |
| Insulin or sulfonylurea medicine | Yes / No / Not sure | Applies the carbohydrate-reduction guard below |
| Clinician-prescribed special diet or medical nutrition requirement | Yes / No / Not sure | Determines whether general recommendations may be shown |
| Pregnancy or breastfeeding status | Yes / No / Not applicable / Prefer not to say | Prevents generic advice from appearing as pregnancy-specific nutrition guidance |

### 10.2 Safety modes

```text
FACTS_ONLY if:
    kidneyOrDialysis IN {YES, NOT_SURE}
    OR prescribedSpecialDiet IN {YES, NOT_SURE}
    OR pregnancyOrBreastfeeding IN {YES, PREFER_NOT_TO_SAY}

GENERAL_GUIDANCE otherwise
```

#### `FACTS_ONLY`

- Show confirmed foods, estimated nutrient totals and the disclaimer.
- Do not show green/amber/red overall suitability language.
- Do not show reduce/add/swap recommendations.
- Do not show “tomorrow's experiment.”
- [v2.0 explicit suppression] Do not show target comparisons or estimated glucose-impact outputs in FACTS_ONLY mode.
- Explain that general suggestions may not fit a prescribed or kidney-related diet and recommend following the user's clinician/dietitian.

#### `GENERAL_GUIDANCE`

- Apply the nutrient-row, verdict and guidance rules in this issue.
- Filter specific suggestions using diet preference and allergies.

#### Insulin/sulfonylurea guard

If `insulinOrSulfonylurea` is `YES` or `NOT_SURE`:

- Do not recommend reducing a breakfast, lunch or dinner below 30 g of total carbohydrate.
- Do not recommend skipping a meal.
- Show: “If you use glucose-lowering medicine, discuss major carbohydrate changes with your care team.”

### 10.3 Guardrail behaviour

- These answers do not select diabetes, CKD, cardiac-recovery or hypertension formulas.
- They do not modify the numeric meal targets.
- If preferences are later changed, historical results are not silently rewritten.
- Preferences are editable from **Check My Meal → Information → Food preferences and guidance safeguards**.
- Changing an answer during an incomplete attempt invalidates any provisional result and re-evaluates the safety mode before calculation. Completed history remains unchanged.
- Consent is accepted again only when the consent version changes; a version change is shown before the next new attempt and never rewrites history.
- If a proposed specific addition conflicts with a preference or allergy, it must be removed from the candidate list.
- If no safe candidate remains, show nutrient-level guidance without naming a food.
- `Other` allergy/intolerance may contain up to 100 characters. Store it with the protected preference profile; never send it to the recognition provider or general analytics.
- Preference/allergy filtering reduces obvious conflicts but does not certify a food, brand or recipe as allergen-free. Suggestions must say: “Check ingredients and follow any allergy advice given by your clinician.”

---

## 11. Input and image requirements

### 11.1 Supported routes

- Take one new meal photograph.
- Select one meal image from the gallery.
- Enter foods manually from the approved searchable catalogue.

### 11.2 File rules

| Requirement | MVP rule |
|---|---|
| Accepted original types | JPG/JPEG, PNG, HEIC/HEIF |
| Quantity | One still image per analysis |
| Maximum original size | 10 MB; configuration-controlled |
| Not accepted | PDF, GIF, video, document or renamed non-image file |
| Validation | Inspect actual file bytes; do not trust extension/client MIME only |
| Normalisation | Correct orientation; convert unsupported analysis format; strip EXIF/location metadata |
| Meal scope | One primary plate/meal per check; ambiguous multiple meals require user selection |
| Optional note | Maximum 250 characters; may help candidate recognition/search only; never authoritative and deleted with the attempt image |

### 11.3 Image-recognition contract

Recognition may return:

- `foodCandidate`
- `preparationCandidate`
- estimated household measure/grams
- item-level confidence
- `food`, `not_food`, `unreadable` or `ambiguous` status

Item-confidence bands for `FREE_HEART_HEALTH_MEAL_V1` are versioned as:

```text
HIGH   = confidence >= 0.85
MEDIUM = confidence >= 0.60 AND confidence < 0.85
LOW    = confidence < 0.60
```

- All bands require user confirmation.
- `MEDIUM` items are visually highlighted and require an explicit confirm/edit action.
- `LOW` items cannot be accepted as-is; the user must search and select a catalogue item, replace it or remove it.
- If the provider does not return a valid finite value from `0` to `1`, treat the item as `LOW`.

Recognition must **not** return or determine:

- authoritative nutrient values,
- the nutrient-row statuses,
- overall green/amber/red verdict,
- estimated glucose-impact category,
- a clinical condition profile,
- medical recommendations.

### 11.4 Production rules

- Provider credentials must never be present in the mobile application.
- Image interpretation is completed through the authenticated iLive service boundary.
- Food identity and portion candidates are mapped to the approved food catalogue before calculation.
- Nutrients supplied by a vision/LLM response must be ignored.
- A photo result always proceeds to mandatory confirmation, including high-confidence recognition.
- The system must never create nutrients or a verdict from a default/typical meal after failure.

---

## 12. Mandatory confirmation and catalogue mapping

### 12.1 User capabilities

Before calculation, the user must be able to:

- Edit an identified food.
- Change its preparation/cooking method where nutritionally relevant.
- Change household measure and/or portion.
- Remove an incorrectly identified item.
- Add a missing item.
- Retake or replace the image.
- Change the meal slot.
- Cancel without creating history.

### 12.2 Calculation prerequisites

Every confirmed food item, including beverages, sauces, oils, salt and accompaniments, must have:

- approved catalogue food/preparation identifier,
- reference quantity,
- confirmed quantity greater than zero,
- energy,
- carbohydrate,
- protein,
- total fat,
- saturated fat,
- fibre,
- free sugar,
- sodium,
- refined/high-GI classification when glucose impact will be displayed.

If any verdict-driving nutrient is unavailable for a confirmed item, the system must not show a green/amber/red result. The user must replace the mapping, remove the item explicitly, or stop the check. Missing values must never default to zero.

### 12.3 Validation

The user cannot continue when:

- no meal slot is selected,
- no food remains,
- an item is unresolved,
- a required portion is missing or zero,
- a relevant preparation is unresolved,
- an item lacks the required approved nutrient mapping,
- the user has not reconfirmed after an edit.

---

## 13. Approved food-data and nutrient calculation contract

The production contract is **`ILIVE_FOOD_CATALOGUE_V1`**, owned and released by iLive Clinical Nutrition/Data. It is seeded from the licensed/approved **Indian Food Composition Tables 2017** data and supplemented only by governed iLive recipe and packaged-food records. Engineering must not substitute LLM-generated values or an unversioned public lookup.

Before release, `ILIVE_FOOD_CATALOGUE_V1` must contain:

- stable food/preparation codes and catalogue version;
- nutrient values per 100 g edible portion as consumed;
- edible-portion factors where entered weight includes non-edible material;
- cooked-yield/retention mappings for preparation methods;
- governed composite-recipe ingredient weights and final cooked yield;
- household-measure-to-gram mappings with versioned supported portion steps;
- curated synonyms for Indian dishes and regional names;
- governed `refinedHighGi` metadata used only by Section 17;
- canonical `freeSugarG` data or governed recipe derivation;
- packaged/brand records only when label data and serving basis have been verified.

If a brand/preparation is not verified, the user must explicitly choose a governed generic equivalent; it must not be silently substituted. Catalogue ownership, approval workflow and publishing are release dependencies, not mobile-team decisions.

### 13.1 Item calculation

For each nutrient `n` in a confirmed food item:

```text
itemAmount[n] = catalogueAmount[n] × confirmedQuantity / catalogueReferenceQuantity
```

### 13.2 Meal calculation

```text
mealTotal[n] = SUM(itemAmount[n] for every confirmed item)
```

Required totals:

- energy, kcal,
- carbohydrate, g,
- protein, g,
- total fat, g,
- saturated fat, g,
- fibre, g,
- free sugar, g,
- sodium, mg.

### 13.3 Calculation rules

- Map household measures such as roti, cup or katori to governed quantities.
- Use the user's final confirmed quantity, not the original vision estimate.
- Canonical `freeSugarG` means sugars added during manufacture/cooking plus sugars naturally present in honey, syrups, fruit juices and fruit-juice concentrates. It excludes sugars naturally present inside intact fruit/vegetables and lactose naturally present in milk.
- `freeSugarG` is already included within total carbohydrate. Do not add it to carbohydrate totals again.
- Derive recipe free sugar from governed ingredient/recipe metadata. Do not substitute total sugar or assume missing free sugar is zero.
- If required `freeSugarG` data is unavailable, the result is not calculable for this MVP.
- Use decimal precision for item/meal calculations. Before threshold comparison, normalise energy to the nearest 1 kcal, sodium to the nearest 1 mg and gram-based nutrients to the nearest 0.1 g, using half-up rounding. Apply the strict comparisons in Section 15 to these normalised values; no additional tolerance is implied.
- After the explicit scoring normalisation above, format a separate display copy. Do not round intermediate item contributions or use rounded UI targets for scoring. [v2.0 precision clarification; numeric thresholds unchanged.]
- Suggested display rounding:
  - energy and sodium: nearest whole unit, half-up;
  - grams: one decimal, half-up;
  - targets on compact cards: nearest whole unit, half-up.
- Equality with a threshold is not a breach because the rules use strict `<` and `>` comparisons.
- Same confirmed inputs + same catalogue version + same rule version must produce the same output.

---

## 14. Rule profile: `FREE_HEART_HEALTH_MEAL_V1`

This is a **general reference profile**, not a personal prescription.

### 14.1 Daily reference values

| Nutrient | Daily reference |
|---|---:|
| Energy | 2,000 kcal |
| Carbohydrate | 250 g |
| Protein | 100 g |
| Fibre | 30 g |
| Saturated fat | 20 g maximum |
| Free sugar | 25 g maximum |
| Sodium | 2,000 mg maximum |

These values must be stored as versioned clinical/product configuration. A configuration change creates a new rule version; it must not rewrite historical results.

### 14.2 Default meal slot using `Asia/Kolkata`

| Local time | Suggested slot | Slot share |
|---|---|---:|
| 04:00–10:59 | Breakfast | 25% |
| 11:00–14:59 | Lunch | 35% |
| 15:00–18:59 | Snack | 10% |
| 19:00–03:59 | Dinner | 30% |

The slot is a suggestion only. The user must be able to change it before confirmation.

### 14.3 Carbohydrate reference ranges

| Slot | Lower | Upper |
|---|---:|---:|
| Breakfast | 30 g | 45 g |
| Lunch | 45 g | 65 g |
| Snack | 10 g | 20 g |
| Dinner | 40 g | 60 g |

### 14.4 Target derivation

```text
energyTarget       = 2000 × slotShare
proteinTarget      = 100 × proteinShare
fibreTarget        = MAX(3, 30 × slotShare)
saturatedFatLimit  = 20 × slotShare
freeSugarLimit     = 25 × slotShare
sodiumLimit        = 2000 × slotShare
```

Protein share:

```text
Breakfast = 30%
Lunch     = 30%
Snack     = 10%
Dinner    = 30%
```

### 14.5 Derived targets

Target calculations use the unrounded values shown in parentheses. Actual meal totals are normalised for scoring as specified in Section 13; UI may show the rounded display values.

| Slot | Energy | Protein | Fibre | Saturated fat | Free sugar | Sodium |
|---|---:|---:|---:|---:|---:|---:|
| Breakfast | 500 kcal | 30 g | 8 g (`7.5`) | 5 g | 6 g (`6.25`) | 500 mg |
| Lunch | 700 kcal | 30 g | 11 g (`10.5`) | 7 g | 9 g (`8.75`) | 700 mg |
| Snack | 200 kcal | 10 g | 3 g | 2 g | 3 g (`2.5`) | 200 mg |
| Dinner | 600 kcal | 30 g | 9 g | 6 g | 8 g (`7.5`) | 600 mg |

### 14.6 Prototype discrepancy resolved

The supplied prototype allocates 35% of daily sodium to each main meal and 15% to snack, which totals 120% across breakfast, lunch, snack and dinner. It also floors snack saturated fat/free sugar at 3 g, causing the four slots to exceed the stated daily maxima. This issue supersedes both shortcuts: all three upper limits follow the 25% / 35% / 10% / 30% shares and total 100%.

---

## 15. Nutrient-row status logic

Each scored row receives:

```text
0 = within this profile's reference
1 = moderately outside the reference
2 = materially outside the reference
```

### 15.1 Upper-limit rows

Applied to energy, saturated fat, free sugar and sodium:

```text
severity = 2 if actual > target × 1.40
severity = 1 if actual > target
severity = 0 otherwise
```

### 15.2 Lower-target rows

Applied to protein and fibre:

```text
severity = 2 if actual < target × 0.60
severity = 1 if actual < target
severity = 0 otherwise
```

### 15.3 Carbohydrate row

```text
severity = 2 if actual > upper × 1.35
severity = 1 if actual > upper
severity = 1 if slot IN {BREAKFAST, LUNCH, DINNER}
                AND actual < lower × 0.50
severity = 0 otherwise
```

Notes:

- Snack is not penalised for carbohydrate below the lower reference.
- There is no generic red state for low carbohydrate in this MVP.
- The insulin/sulfonylurea guard in Section 10 applies to the recommendation engine separately.
- Total fat is calculated and used in glucose-impact estimation but has no traffic-light target in this profile.

---

## 16. Overall meal verdict

Priority rows, in order:

1. Saturated fat.
2. Fibre.
3. Free sugar.
4. Sodium.

```text
severeCount        = COUNT(rows where severity == 2)
moderateCount      = COUNT(rows where severity == 1)
prioritySevere     = ANY(priority row where severity == 2)
priorityModerate   = ANY(priority row where severity == 1)

RED if:
    prioritySevere
    OR severeCount >= 2

AMBER if not RED and:
    severeCount == 1
    OR priorityModerate
    OR moderateCount >= 3

GREEN otherwise
```

### 16.1 User-facing labels

| Engine value | UX label |
|---|---|
| Green | **Generally supports the heart-health meal targets** |
| Amber | **A few changes could improve this meal** |
| Red | **Several parts of this meal need attention** |

Colour must never be the only signal. Display the text label and an accessible icon.

A green outcome means only that the confirmed meal falls within this profile's rules. It does not mean “safe,” “doctor approved,” or appropriate for every condition.

Under this supplied aggregation logic, an overall green result can coexist with one or two non-priority rows at severity `1`. UX must show those row observations and must not replace them with an “everything is ideal” message.

---

## 17. Estimated glucose-impact calculation

This is a **dimensionless composition score** from the supplied business/clinical logic. It is not a glucose measurement, predicted glucose value or medical alert. The entire output remains hidden unless Clinical signs off the exact formula, input definitions, thresholds, labels and test vectors for the released rule version.

### 17.1 Prerequisites

- Complete confirmed meal composition.
- Total carbohydrate, free sugar, fibre, protein and total fat.
- Curated `refined/high-GI` classification for each carbohydrate item.
- No unresolved confirmed carbohydrate item.

The classification must come from governed food-catalogue metadata, not image-model inference.

`freeSugarG` is already included in total carbohydrate; the additional coefficient below is an intentional weighting factor in the supplied heuristic, not a second carbohydrate quantity.

Input validation before calculation:

```text
all inputs must be finite and >= 0
0 <= refinedCarbohydrate <= totalCarbohydrate
```

If validation fails, suppress the score and return a calculation-unavailable state.

### 17.2 Formula

```text
refinedCarbohydrate =
    SUM(carbohydrate from confirmed items classified refined/high-GI)

refinedShare =
    0                                      if totalCarbohydrate == 0
    refinedCarbohydrate / totalCarbohydrate otherwise

baseLoad =
    totalCarbohydrate × (0.7 + 0.6 × refinedShare)
    + freeSugar × 1.5

protectiveFactor =
    fibre × 4
    + protein × 1.6
    + totalFat × 0.8

deduction = MIN(baseLoad × 0.35, protectiveFactor)

estimatedGlucoseImpactScore = baseLoad - deduction
```

### 17.3 Classification

| `estimatedGlucoseImpactScore` | Display category |
|---:|---|
| `≤ 38` | Small estimated glucose impact |
| `> 38` and `≤ 70` | Moderate estimated glucose impact |
| `> 70` | Large estimated glucose impact |

### 17.4 Required copy

> This is estimated from the meal composition. It is not a blood-glucose reading.

Do not display the category if a prerequisite is missing. Do not infer that the meal changed the user's actual glucose.

---

## 18. Observation and suggestion logic

All user guidance must come from approved deterministic templates. Do not generate medical/nutrition recommendations freely with an LLM.

### 18.1 Leading observations

1. Select all rows with `severity > 0`.
2. Sort by:
   1. severity descending,
   2. priority-row order,
   3. relative deviation from the relevant threshold,
   4. fixed display order as final tie-breaker.
3. Show no more than three.

Relative deviation used for ordering:

```text
upper-limit row: MAX(0, actual / target - 1)
lower-target row: MAX(0, 1 - actual / target)
carbohydrate high: MAX(0, actual / upper - 1)
carbohydrate very low: MAX(0, 1 - actual / (lower × 0.50))
```

Final fixed tie order is: saturated fat, fibre, free sugar, sodium, carbohydrate, protein, energy.

### 18.2 Positive observations

- Select only rows with `severity == 0`.
- Show no more than three.
- Never describe missing/unavailable information as positive.

### 18.3 Action selection

Rank candidate actions using the same issue ordering. Show no more than three actions.

| Trigger | Deterministic action rule |
|---|---|
| Energy high | Identify the confirmed item contributing the most energy; suggest reducing its portion |
| Carbohydrate high | Identify the largest refined/high-GI carbohydrate contributor; suggest a smaller portion |
| Protein low | Offer one preference/allergy-compatible protein candidate from the approved content catalogue |
| Fibre low | Offer one preference/allergy-compatible fibre/vegetable candidate from the approved content catalogue |
| Saturated fat high | Identify the largest saturated-fat contributor; suggest a smaller portion or approved lower-fat preparation |
| Free sugar high | Identify the largest free-sugar contributor; suggest removing it or choosing an approved unsweetened/reduced-sugar form |
| Sodium high | Identify the largest sodium contributor; suggest reducing it, added salt, or the relevant high-salt accompaniment |

Additional rules:

- Never claim that an unconfirmed food was present.
- Never recommend skipping a meal.
- Never recommend medication changes.
- Never use a static/random rotating “longevity boost.”
- If the triggering contributor cannot be identified, show nutrient-level guidance only.
- Apply diet-preference, allergy and medicine guards before choosing an add/swap.
- If no compliant candidate remains, omit the specific food suggestion.
- In `FACTS_ONLY` mode, suppress all action suggestions.
- If “This week” or “Tomorrow's experiment” remains in the design, it must use the highest-ranked available action rather than a random tip.

#### Exact action simulation

- For a “reduce existing item” action, calculate the reduction needed for the triggered nutrient to reach its target using that item's nutrient contribution.
- Clamp the proposed reduction to `10%–50%` of that item's confirmed quantity, then round to the nearest smaller supported catalogue portion step.
- Recalculate the complete meal using the proposed change before showing it.
- For an “add” action, use one clinically approved catalogue candidate and its configured serving size; never invent a serving in the client.
- Suppress any action that increases the severity of energy, sodium, saturated fat or free sugar, introduces a declared allergen/preference conflict, or causes a main-meal carbohydrate total below 30 g when the medicine guard applies.
- Remove duplicate actions that target the same food and nutrient; retain the higher-ranked action.
- If simulation does not improve the triggering row or no compliant portion exists, show neutral nutrient-level education instead of an action.

---

## 19. Calculation examples and boundary tests

These examples are part of the functional contract and should be automated tests.

### 19.1 Breakfast energy boundary

Breakfast energy target = `500 kcal`.

| Actual | Expected severity | Reason |
|---:|---:|---|
| 500 | 0 | Equality is within target |
| 501 | 1 | Greater than target after scoring normalisation |
| 700 | 1 | Severity 2 uses strict `>` |
| 701 | 2 | Greater than `500 × 1.40` after scoring normalisation |

### 19.2 Breakfast fibre boundary

Unrounded target = `7.5 g`; severe boundary = `4.5 g`.

| Actual | Expected severity |
|---:|---:|
| 7.5 | 0 |
| 7.4 | 1 |
| 4.5 | 1 |
| 4.4 | 2 |

### 19.3 Breakfast carbohydrate boundary

Range = `30–45 g`; high severe boundary = `60.75 g`; very-low threshold = `15 g`.

| Actual | Expected severity |
|---:|---:|
| 45 | 0 |
| 45.1 | 1 |
| 60.7 | 1 |
| 60.8 | 2 |
| 15 | 0 |
| 14.9 | 1 |

### 19.4 Overall verdict example

- Saturated fat severity `0`
- Fibre severity `1`
- Free sugar severity `0`
- Sodium severity `0`
- All other rows severity `0`

Expected overall verdict: **AMBER**, because fibre is a priority row with severity `1`.

### 19.5 Estimated glucose-impact example

Confirmed totals:

- carbohydrate = `60 g`
- refined/high-GI carbohydrate = `30 g`
- free sugar = `5 g`
- fibre = `8 g`
- protein = `25 g`
- total fat = `20 g`

```text
refinedShare = 30 / 60 = 0.5
baseLoad = 60 × (0.7 + 0.6 × 0.5) + 5 × 1.5
         = 60 × 1.0 + 7.5
         = 67.5

protectiveFactor = 8 × 4 + 25 × 1.6 + 20 × 0.8
                 = 32 + 40 + 16
                 = 88

deduction = MIN(67.5 × 0.35, 88)
          = 23.625

estimatedGlucoseImpactScore = 67.5 - 23.625
                            = 43.875
```

Expected display: **Moderate estimated glucose impact**.

---
