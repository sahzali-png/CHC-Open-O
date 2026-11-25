# 03 · Patient chart (CHC front end)

Mockup: https://claude.ai/artifact/68K1sZNfPV9iuNeT1yAuqu#artboard-Chart

## Layout
- **Patient banner**: initials badge; name (UPPERCASE italic display); sex · age · DOB; health card
  with validity; phone; referring doctor. Right: allergy chip (red, with the word "Allergy"),
  **ConnectingOntario ↗** (contextual launch, patient already selected), **Bill visit**.
  "← Today's schedule" link.
- **Tabs**: Summary · Encounters · Tests & imaging · Labs · Hospital reports · Medications · Documents · Billing.
- **Summary tab, three columns** (stack on small screens):
  - Left: Problems; Medications (name + directions) with "+ Prescribe (interaction check on)".
  - Centre: **Consult note** with sections Reason for referral (pre-filled from the referral),
    History, Examination, Impression, Plan; autosave indicator; **Save draft** and
    **Sign & fax to <referring doctor>**.
  - Right: Tests & imaging (latest echo preview with **Open in viewer** = CHC DICOM viewer, and
    **Start report**; other tests with status); Labs · OLIS (value + flag word, critical in red);
    Hospital reports · HRM (list, newest first).

## Behaviour
- Notes, prescriptions, allergies, labs and documents use OpenO's existing data and rules
  (drug interaction checking, OLIS, HRM) — the CHC chart is a new view, not new storage.
- "Sign & fax" signs the note (locks it) and faxes the letter through OpenO's fax module.
- "Open in viewer" opens the CHC DICOM viewer (chc-platform Module 2) for that patient's study.

## Compared with stock
- Stock: eChart — left navbar modules, note area, many pop-up windows.
- CHC: one page with the cardiology essentials (tests, OLIS, HRM, note) visible together.
- Every item has "Open in Classic view" for anything the CHC chart doesn't cover yet.

## Done when
- [ ] A note signed in CHC shows in stock eChart, and vice versa
- [ ] Medications/allergies match stock exactly
- [ ] Critical OLIS results stand out and are marked reviewed in both views
