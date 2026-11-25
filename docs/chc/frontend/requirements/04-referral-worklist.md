# 04 · Fax referral worklist (CHC front end)

Mockup: https://claude.ai/artifact/68K1sZNfPV9iuNeT1yAuqu#artboard-Referrals
Back end: CHC fax intake service in the sibling repo `sahzali-png/chc-platform`
(SRFax polling, local OCR, referral API — branch `feature/srfax-incoming`).

## Layout
- Heading: eyebrow "// INCOMING FAXES · SRFAX", title "REFERRAL WORKLIST", "Last checked hh:mm ·
  next in 1 min · every referral answered within 24 h".
- **Left list**: one card per referral — patient (or "(unreadable fax)"), referring doctor,
  services; time left on the 24 h clock (orange when under 4 h; "Booked <date>" when done);
  urgency chip (Urgent / Semi-urgent / Routine) and status chip (New / Needs details / Confirmed).
  Selected card has a red border.
- **Right panel**: the original fax (page preview, "Open full size") beside the details read from it:
  last/first name, DOB, health card, patient phone, referring doctor, billing number, doctor fax,
  reason, tests requested (checkboxes), urgency (with the suggested reason shown).
  Any field that couldn't be read is highlighted red with "Not found — please enter".
- Buttons: **Not a referral** · **Confirm & book** (orange).

## Behaviour
- Nothing is accepted automatically: staff must confirm (chc-platform hard rule 4).
- Confirm & book → creates/links the patient in OpenO, saves the fax PDF to the patient's
  documents, and opens booking; status becomes Confirmed.
- Every view of a fax and every change is audit-logged.

## Compared with stock
- Stock: faxes land in the inbox/document manager; staff read the PDF and retype everything.
- CHC: details pre-filled for checking, a 24 h clock per referral, urgency suggested.

## Done when
- [ ] A confirmed referral's patient and PDF appear correctly in stock OpenO
- [ ] Clock and urgency match the fax intake service
