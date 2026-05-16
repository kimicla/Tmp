Create a professional, editable 16:9 PowerPoint presentation titled:

“EIPO Subscriptions and Payments: HKEX FINI Integration Blueprint”

Audience:
Operations, technology, risk, and product stakeholders at a financial institution preparing an automated EIPO/FINI integration.

Output requirements:
- Generate a fully editable PowerPoint deck, not image-only slides.
- Use editable text boxes, shapes, connectors, tables, and icons.
- Do not include any watermark.
- Use a clean financial-technology style: white background, deep navy headings, teal/cyan process arrows, green control/checker elements, light grey legacy/manual areas, rounded rectangles, subtle shadows, simple line icons.
- Use consistent terminology: EIPO, FINI, FSS Ops, RM, SCB, HKEX.
- Keep diagrams clear enough for executive review and implementation discussion.
- Add small footer text only where useful, no decorative clutter.

Create 18 slides with this structure:

Slide 1: Title
Title: “EIPO Subscriptions and Payments”
Visual: clean Hong Kong skyline line illustration with subtle teal highlight.
Subtitle optional: “HKEX FINI Integration Blueprint”

Slide 2: The Operational Ecosystem
Show a left-to-right ecosystem flow:
1. Individual & Broker Clients: aggregate investor orders and funding
2. SCB Relationship Manager: facilitates client instructions and loan details
3. FSS Ops / Operations: submit subscriptions, manage pre-funding, process refunds
4. HKEX Platform / FINI
Use large process cards connected by arrows.

Slide 3: Evolution of EIPO Operations: From Manual to Automated
Two-column comparison:
Left: “Legacy Vendor Program: Manual & Painful”
Flow: FSS Ops Checker → manual data copying and email communication → FSS Ops Maker → manual file uploads to FINI.
Reality: shortages, high manual effort, email dependency, high operational risk.
Right: “New Automated Target Operating Model: API-Driven & Secure”
Flow: automated data refresh → internal EIPO system → API-driven encryption and submission → real-time FINI direct connection and validation.
Improvements: streamlined workflow, reduced manual touchpoints, enhanced security.

Slide 4: The Operational Anchor: T-Day to T+2 Timeline
Left side: IPO lifecycle:
- T-Day, e.g. T-3 to T-1: deal initiated; broker clients aggregate orders
- T-Day, e.g. T-1: public offer closes; FINI submission deadline is 12:00 noon
- T+1: validation, funding, allotment; HKEX runs allotment
- T+2: trading starts on HKEX
Right side: EIPO system responsibilities:
1. Subscription & validation
2. Funding & submission
3. Allotment & processing

Slide 5: FSS Ops: The Maker/Checker Governance Engine
Create a 4-column governance table.
Columns:
- IPO Event Setup
- Client Instructions & Funding
- Submission Authorization
- Monitoring & Reporting
Top row: Maker / Operations actions.
Bottom row: Checker / Control actions.
Show controls such as official announcement verification, funding validation, final authorization before noon, immutable audit logs.

Slide 6: Phase 1: Before Closing - Front-End User Interface
Show architecture:
SCIP / Service Bench EIPO Plugin used by FSS Ops Maker/Checker.
Inside Service Bench: Plugin UI and Experience API.
Connect securely to HashiCorp Vault.
Add label: “You are here: T-Day”.

Slide 7: Phase 1: Before Closing - Backend Processing Engine
Show Ants SKE backend engine with:
- Security layer
- Process API
- IPO data maintenance
- Subscription maintenance
- PostgreSQL database
Show authorized data flow, maker/checker validation, and persistence of entries.

Slide 8: Phase 1: Strict Formatting & Reference Data
Show three validation blocks:
- HKID idType 1: uppercase letters; exactly 6 or 8 integers; check digit in parentheses; no spaces
- LEI idType 4: exactly 20 uppercase alphanumeric characters
- BCAN idType 8: 6 uppercase alphanumeric characters plus up to 10 positive integers; note PI names must be null when using BCAN
Right side: “Ingesting Reference Data”
API: GET /api/ipos/refdata/v1
Include key reference fields such as ipoId, openDate, bookOpenDate, pricingDate, allotmentDate.
Mark exact endpoint/field names as “verify against official FINI API docs”.

Slide 9: Phase 1: Before Closing - Outbound Dispatch
Show Ants Scheduler sending subscriptions through Zscaler Proxy to HKEX FINI API.
Include external boundary and optional boundary labels.
Add bulk submission mechanics:
- Endpoint: POST /api/ipo/subscriptions/add/v1
- Payload rule: package in maximum batches of 1,000 objects per request
- Output: API generates a unique 17-character recordID
Mark exact endpoint/rules as “verify against official FINI API docs”.

Slide 10: Operational Edge Cases: Amend vs. Invalidate
Two-column comparison:
Amend /change/v1:
- Requires valid recordID of active subscription
- Authenticated impact because full data is reprocessed through cryptographic pipeline
- Fresh AES keys must be generated
Invalidate /invalidate/v1:
- Logical cancellation, not hard database deletion
- Changes upload status from 3 Authorized to 4 Invalidated
- Pre-funding entry may not be reprocessed
Use lock/delete icons and concise bullet points.

Slide 11: The Engine of Trust: Cryptographic Pipeline
Create a four-step pipeline:
1. Generate local keys
2. Encrypt data fields
3. Encrypt the key
4. Sign the payload
Include labels such as AES/GCM/NoPadding, RSA public key encryption, digital signature, SHA/RSA where appropriate.
Mark cryptographic details as “verify against official FINI security specification”.

Slide 12: Phase 2: Funding Validation Logic
Show decision flow:
POST /api/eipo/funding/query/v1
If Y:
- Compare maximum public offer value vs. application value
- Pre-funding is the lower of maximum public offer value or application value
If N:
- Pre-funding is strictly the participant’s total application value
Then: after mathematical validation, participant must submit POST /confirm/v1.
Show transition from PreFundingStatus 10 Pending to 20 Confirmed.

Slide 13: Financial Reality: The Prefunding & Refund Ledgers
Create accounting-style flow diagram:
The Prefunding Phase / Debit:
Participant cash account → EIPO nominee account.
The Settlement / Refund Phase / Credit:
EIPO nominee account → participant cash account.
Show example amount labels as placeholders and note that actual amounts should be sourced from transaction records.
Mention APIs: toBankAcctNum, transactionRef.

Slide 14: Phase 3: Allotment - The Automated Backend Fetch
Show Ants SKE process polling allotment results from HKEX FINI API via Zscaler proxy and scheduler.
Persist results and update lifecycle status in PostgreSQL.
Use label: “You are here: T+1”.

Slide 15: Phase 3: Allotment - The Operational Front-End Review
Show FSS Ops accessing Service Bench EIPO Portal.
Flow:
Plugin UI → view allotment result / view refund result → SCIP authentication → security layer → Ants SKE Process API → PostgreSQL.
Outcome: FSS Ops can view and download reports without logging directly into the external HKEX portal.

Slide 16: Final Phase: Automated Fee Billing
Show UI-to-backend billing flow:
FSS Ops downloads results / submits fee request → SCIP authentication → BCOP authorization → Ants SKE process API → PostgreSQL.
Include HashiCorp Vault for secure delivery of generated reports.
Label: “You are here: T+2”.

Slide 17: Exception Handling & Error Architecture
Create three horizontal error categories:
1. Network & Gateway Constraints
Examples: too many requests, unauthorized, timeout.
2. Cryptographic Failures
Examples: missing signature, invalid payload, unchanged request, public key changes.
3. Payload Validation
Examples: semantic errors, rejected objects, array/file limits.
Use color coding: blue for network, green for cryptographic, red for validation.

Slide 18: Operational Resilience & Contingency Framework
Create three cards:
1. Audit Readiness
EIPO control data retained in compliance-accessible logs; additional monitoring; management reports.
2. Maintenance Windows
Scheduled downtime from Saturday 12:00 to Sunday 12:00; hard-coded API rejections during unavailable windows.
3. Contingency Protocols
Fallback workflow, FINI portal access, client orders, and operational readiness during HKEX or internal outages.

Quality requirements:
- Every slide must have a clear title, one main message, and a diagram/table/process visual.
- Avoid generic stock imagery except the title skyline.
- Use consistent icon style.
- Keep text concise and readable.
- Use speaker notes to flag any details that require verification against official HKEX FINI API documentation or internal SCB/FSS procedures.
- Final file should be editable PPTX.
