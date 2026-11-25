# 02 · New encounter form (CHC front end)

Mockup (clickable in Play): https://claude.ai/artifact/68K1sZNfPV9iuNeT1yAuqu#play-Main
Opened by the orange **"+ New encounter"** button on Today's schedule. Opens as a pop-up (dialog)
over the schedule.

## Header
Brand gradient band (red → deep red → black), eyebrow "// <clinic> · today",
title "NEW ENCOUNTER", close (×) button.

## Step 1 · Patient
Two-way switch: **Existing patient** | **New patient**.

**Existing patient**
- Search box: name, health card, date of birth or phone. Results update as you type.
- Each result: name, sex, DOB, health card, "last seen". Click to select.
- Selected patient shows as a highlighted card with a **Change** link.
- No match → "No match. Create a new patient →" switches to New patient.

**New patient**
- Fields: First name, Last name, Date of birth, Sex, Health card number, Version code, Phone,
  Referring doctor (search existing doctors or type).
- Health card is validated with the Ministry on save (OpenO's existing HCV, where configured).
- Before saving, warn if a possible duplicate exists (same health card, or same name + DOB).

## Step 2 · Encounter type (choose one)
- **Consult only** — shows: Consult type (New consult, Follow-up, Hypertension, Arrhythmia,
  Heart failure, Cholesterol, Heart valve, POTS clinic) and Cardiologist.
- **Tests only** — shows a test checklist: Echocardiogram, Contrast echo, Exercise stress test (GXT),
  Treadmill stress echo, Resting ECG, Holter 24–48 h, ABPM 24 h, Nuclear cardiology.
- **Consult + tests** — shows both.

## Footer
- Hint text says what's still needed: "Choose or create a patient" → "Choose consult, tests, or
  both" → "Tick at least one test" → "Ready — checks the patient in and opens the chart".
- **Cancel** clears and closes. **Start encounter** stays disabled until: a patient is chosen or
  entered, a type is chosen, and (for tests) at least one test is ticked.

## On "Start encounter"
1. New patient → create the demographic record (stock rules and required fields apply).
2. Create today's appointment(s) at the selected clinic, now, with the visit type(s) as reason.
3. Mark the patient **Checked in**.
4. Open the patient chart. Consult included → open a new consult note draft for the chosen cardiologist.
All of this is logged in OpenO's audit log, same as doing it in stock.

## Compared with stock
- Stock: separate screens — search demographic, add demographic, add appointment, check in, open encounter.
- CHC: one pop-up does all of it in two steps, with consult vs tests choice up front.
- Same data: the patient, appointment and check-in it creates look normal in stock OpenO.

## Done when
- [ ] Existing-patient search finds the same patients as stock search
- [ ] New patient saved here opens correctly in stock demographic screen
- [ ] Appointment and check-in appear in stock day sheet
- [ ] Duplicate warning works
- [ ] Keyboard-only use works (Tab order, Esc closes, Enter selects)
