# 08 · Consult note page (CHC front end)

Requested by Ali Zaidi, 2026-11-25. Mockup: Design canvas artboard "8 · Consult note".
Opened from the schedule (New encounter → consult), the patient chart, or a confirmed referral.

## 1. Billing and diagnosis — top of the page
- **Fee codes**: search by code or words (e.g. "A605 Consultation", "A606 Repeat consultation").
  Codes and fees come from OpenO's Ontario billing tables (kept current with OHIP Schedule of
  Benefits updates) — never hard-coded in the CHC front end.
- **Diagnosis codes**: OHIP 3-digit diagnostic codes (e.g. 413 Angina pectoris, 427 Cardiac
  dysrhythmias, 428 Congestive heart failure, 401 Hypertension), search by code or words.
  One is marked **primary**.
- **Suggestions** (never auto-added): from the referral reason and the encounter type
  (new consult vs repeat), shown as "+ Add" chips. Staff/cardiologist confirm.
- Fields: referring physician + billing number (from referral), service date, location (clinic),
  manual review flag. **Send to billing** creates the claim in OpenO's Ontario billing (stock
  claim flow; nothing new is sent to OHIP from the CHC front end).
- Validation shown inline: missing referring physician for a consult code, repeat consult within
  the rules window, etc. (rules from OpenO billing; flagged, not blocked, unless OpenO blocks).

## 2. Standard sections (text boxes)
Default sections (the clinic template), each an **editable text box right on the page**:
Reason for referral · History of presenting illness · Past medical history · Medications ·
Allergies · Family history · Social history · Cardiac risk factors · Review of systems ·
Physical examination · **Recent investigations** · Impression · Plan · Recommendations to referring
physician · Follow-up.
- Boxes auto-grow; autosave every few seconds ("Saved 9:41").
- Some boxes can be pre-filled from the chart on open (Medications, Allergies, Investigations
  from Holter/Echo/ECG/OLIS), clearly marked "from chart — edit freely".

### Recent investigations box
- Lists every test this patient has had **in the CHC system** (Echo, Stress echo, Stress test,
  Holter, ECG, ABPM), newest first: test, date, clinic, reading cardiologist, status.
- Signed reports show their **Conclusion**; **"+ Add conclusion"** inserts
  "<Test> (<date>, CHC): <conclusion>" into the box. **"Add all signed"** adds every one.
  Once added, the row shows "Added ✓" (removing the text from the box re-enables it).
- Unsigned studies show "Report pending" and can't be added (no unsigned conclusions in a letter).
- Each row links to the full report. Filter: last 12 months / all.
- The box text stays freely editable; it also keeps its own quick phrases (e.g. "Labs reviewed on OLIS").
- Pulled live from the CHC reporting service (chc-platform Module 3) for this patient; every view audit-logged.

### Reorder by drag and drop
- Each box has a drag handle on its heading; drag to change the order.
- Keyboard/touch alternative: "Move up" / "Move down" buttons (accessibility).
- The order is saved to the user's template ("Save as my default order") and can be reset to the
  clinic default.

### Quick phrases (pre-populated values) for every box
- Under each box: chips of standard phrases (e.g. Physical exam: "JVP not elevated",
  "S1 S2 normal, no murmurs", "Chest clear", "No peripheral edema").
- **Hover** a chip → full text preview. **Click** → inserts it into the box at the cursor
  (or at the end). Chips can contain blanks to fill, e.g. "BP ___/___ mmHg".
- **Customize** per box: add a phrase, edit it, remove it, reorder phrases.
  - "My phrases" (personal) vs "Clinic phrases" (shared; editing needs the template-admin role).

### Customize the sections themselves
- Rename a section heading, hide/remove a section, add a new section (e.g. "Device check").
- Save as: my template · clinic template (admin) · a named template (e.g. "Arrhythmia clinic",
  "Heart failure follow-up"); pick the template when the note opens.

## 3. Finish
Save draft · Preview letter (CHC-branded consult letter) · **Sign & fax to referring physician**
(and family doctor/cc list) → saved to the OpenO chart, letter faxed via OpenO fax, claim to billing.

## Rules
- Stock OpenO eChart/encounter note untouched; the signed note appears there as a normal note.
- Every edit audit-logged; signed notes locked; later changes = addendum.
- Phrase libraries and default sections: cardiologist champion approves clinic-wide ones.
- Billing codes and rules: confirm with CHC billing staff; OHIP schedule changes flow from OpenO tables.

## Done when
- [ ] Boxes reorder by drag and by keyboard, order saved per user
- [ ] Phrase chips insert on click; add/edit/remove phrases per box works and persists
- [ ] Fee and diagnosis codes search OpenO tables; claim appears in stock OpenO billing
- [ ] Signed note in stock eChart; letter faxed to referrer
