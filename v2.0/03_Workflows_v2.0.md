# User, backend and attempt workflows — v2.0

The diagrams describe the proposed feature, not existing production integrations. Read with the issue and backend contract.

## Two activities. No hidden clinical switch.
![Two activities. No hidden clinical switch.](Workflows/01_Two_Mode_User_Journey.png)
Editable: `Workflows/01_Two_Mode_User_Journey.mmd` (Mermaid) and `.dot` (Graphviz).

```mermaid
flowchart TD
 H[Existing Free Home] --> U[Check My Meal hub]
 U -->|Explore care| P[Optional category interest]
 P --> C[Approved educational preview]
 C --> A[Existing profile-aware care action]
 C -->|Your meals| U
 U -->|Actual meal| E{Authorised new check?}
 E -->|No| X[Lock analysis; retain history and previews]
 E -->|Yes| S[Consent and safeguards]
 S --> I[Photo / gallery / manual catalogue]
 I --> M[Map and explicitly confirm food / portion / preparation]
 M --> R[General result or facts-only]
 R --> L[Immutable completed history]
 R --> A
 N[Preview category never selects clinical rules] -.-> C
 N -.-> R
```

## Configuration changes content, not patient facts.
![Configuration changes content, not patient facts.](Workflows/02_Backend_First_Responsibilities.png)
Editable: `Workflows/02_Backend_First_Responsibilities.mmd` (Mermaid) and `.dot` (Graphviz).

```mermaid
flowchart LR
 UI[React Native: typed blocks and input] --> C[Context and profile authorisation]
 C --> P[Approved preview publication]
 P --> UI
 C --> A[Bounded attempts and revisions]
 A --> V[Recognition: candidates only]
 V --> A
 A --> F[Governed food catalogue]
 F --> E[General policy and safeguards]
 A --> E
 E --> H[Immutable results]
 H --> UI
 C --> B[Existing commercial resolver]
 B --> UI
```

## The attempt deadline is independent of the programme timer.
![The attempt deadline is independent of the programme timer.](Workflows/03_Attempt_Expiry_and_Commit.png)
Editable: `Workflows/03_Attempt_Expiry_and_Commit.mmd` (Mermaid) and `.dot` (Graphviz).

```mermaid
flowchart TD
 A[Create trusted attempt while free access is active] --> D[Draft / recognition / confirmation]
 D --> G{All authorisation, deadline, revision and safety checks pass?}
 G -->|Yes| C[Calculate and recheck atomically at save]
 G -->|No| X[Block with reason]
 C -->|Valid commit| R[One immutable completed result]
 C -->|Changed / late| X
 D -->|Cancel / terminal failure| T[Terminal; no history]
 X --> T
 R --> Z[Transient image cleanup]
 T --> Z
 N[60-minute attempt deadline never extends] -.-> G
```

## Exceptions shared by both activities
Changing preview category refreshes the complete educational bundle only. Changes to the protected safety answers can change actual output mode; they do not change published preview content. Profile switching invalidates prior private requests. Pending order status changes only the commercial action and does not extend meal entitlement. Error recovery never introduces a demo result into personal history.

## Future paid extension
The same UI blocks, catalogue mapping and result storage can be reused. Patient-specific clinical selection, personal nutrient targets, multi-condition reconciliation, nutritionist operations and paid entitlements require their own scope, contracts and approvals. Do not turn them on by reusing the prototype name-matching function.
