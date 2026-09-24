# UX screen and state map — Check My Meal v2.0

**27 rendered review snapshots across 14 logical view templates.** These are not 27 new app pages. Home is reused; native permissions, history modes and error states share templates. Each preview category reuses T03. No new Your Diet screen is designed here.

## Source and visual direction
Navy iLive palette and rounded cards follow the supplied prototype/reference mocks. All text, categories, controls and states in this pack are deterministic HTML, not generated image text. Browser review uses fixtures only. Prior infographic images are superseded, not extra screen scope.

| State ID | Screen/state | Template | Required interpretation |
|---|---|---|---|
| S01 | Home entry | T00 | Keep the existing Home hierarchy. Only the Check My Meal entry is revised; adjacent modules are unchanged. |
| S02 | Your meals · active | T01 | Default hub. General real-meal scope is explicit; Explore care is optional. No daily calorie budget or one-meal quota. |
| S03 | Choose a care interest | T02 | Browsing only. Six draft categories demonstrate capability, not launch approval. Do not write selected interest to the clinical record. |
| S04 | Preview · heart health | T03 | One preview template. Every visible section belongs to this category and is labelled as education, not a personal diet. |
| S05 | Preview · diabetes | T03 | Category content changes, but the real-meal engine does not. A visible Your meals tab leads back to general checking. |
| S06 | Preview · blood pressure | T03 | Three nutrient topics are supplied for this source profile. Do not force every category to show four chips. |
| S07 | Preview · kidney care | T03 | No specific mineral/protein/fluid limits or blanket food prescription. Do not mix this preview with a real-meal verdict. |
| S08 | Preview · dialysis | T03 | Separate content identity from non-dialysis. No medication/binder timing or personal treatment advice is published here. |
| S09 | Preview · recovery | T03 | Recovery is a draft content category, not automatic recovery-programme enrolment or rehabilitation advice. |
| S10 | First use · explanation | T04 | Shown before creating a real check. Consent text needs Privacy approval. Informational previews do not require image processing. |
| S11 | Safety & food preferences | T05 | All six source fields remain explicit. This long page scrolls. No missing answer defaults to No/None; factual safety mode is derived by the server. |
| S12 | Add a meal | T06 | Camera, gallery and manual entry plus an editable meal slot. This review mock never uploads a real image. |
| S13 | Camera permission denied | T06 | Permission state variant, not a new nutrition flow. Offer gallery/manual routes and native settings guidance. |
| S14 | Manual food catalogue | T07 | Approved searchable preparation records. Search/add/edit interactions need existing catalogue identifiers, not model-created nutrient values. |
| S15 | Food & portion editor | T08 | Update preparation/quantity from governed choices. Confirmation is invalidated by every edit; no zero or missing portion. |
| S16 | Recognition in progress | T09 | Identity/portion candidates only. Cancellation terminates work. No clinician-review wording; no nutrient verdict before confirmation. |
| S17 | Confirmation · unresolved | T10 | An unresolved item disables calculation. User must map, replace or explicitly remove it. No guess or silent omission. |
| S18 | Confirmation · ready | T10 | Explicit confirmation required even with high-confidence image recognition. One server revision produces one immutable result. |
| S19 | General meal result | T11 | Illustrative numeric fixture, not a real meal. Server supplies all rows and observations; preview category is absent from the analysis context. |
| S20 | Nutrient facts only | T11 | Same result template, safety variant. No targets, traffic lights, suitability judgement, suggestions or glucose-impact category. |
| S21 | Meal history | T12 | Only completed checks. Multiple entries in one meal slot are allowed; these are not proof of consumption or a daily intake total. |
| S22 | Historical result detail | T11 | Original result and original context. Changing browsing category or current rules never rewrites history. Edit creates a new authorised draft. |
| S23 | Free analysis ended | T13 | New camera/gallery/manual analysis is locked. History and approved care previews remain readable. No timer restart from browsing/purchase. |
| S24 | Preview · paid order pending | T03 | Commercial state variant. Replace Explore plans with Track activation; preview selection cannot create a second primary order. |
| S25 | Preview content unavailable | T03 | No stale revoked, partial or unrelated-category content. Other permitted activities remain available. Unknown required blocks fail safely. |
| S26 | Meal recognition failure | T09 | No sample-meal substitution. Retry/manual recovery uses the original attempt deadline. A new draft requires new authorisation. |
| S27 | Empty meal history | T12 | History empty state; no fabricated examples in personal records. Illustrative review fixtures are not production records. |

## Interaction and accessibility
The category picker is an optional sheet/page from Explore care. Preview pages scroll; all categories share the same component set. Preserve the originating tab/category and scroll on return. First-use consent and all six safeguards are real input states, not passive banners. Editors and confirmation must preserve supported portions and reason codes. Long-form copy should reflow under native large text. Minimum touch targets follow the existing app standards (44 pt iOS / 48 dp Android); verify at supported small and large devices, and with screen readers. Mock browser pixels are not a native accessibility certification.

The overview and sample navigation do not replace missing API validation. Demo buttons intentionally advance fixture states without uploading images, calculating nutrition or purchasing a plan. Plain-language unavailable/expired states must retain the correct recovery action and never show a success result after a failure.
