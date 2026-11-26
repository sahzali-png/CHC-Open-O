# 01 · Day sheet / today's schedule (CHC front end)

**Updated 2026-11-25:** must match everything Cerebrum's Daysheet does — see
`09-cerebrum-parity.md`. The appointment table below is superseded by that checklist where they differ
(per-row tests with times, arrived/left, room/staff, action on abnormal, family + referring doctor
with fax and billing #, billing type, health card validation, confirmation method, filters, paging).

Mockup: https://claude.ai/artifact/68K1sZNfPV9iuNeT1yAuqu#artboard-Main
Users: front desk, technologists, cardiologists, clinic managers.

## Layout
- **Left sidebar (dark, `ink-950`)**: CHC logo mark in a white circle + "Canadian Heart Care · EMR";
  clinic selector (each CHC clinic + "All clinics"); navigation with count badges:
  Schedule, Patients, Referrals (7), Tests & imaging, Reports to sign (9), Inbox · labs & HRM (14),
  Billing (OHIP), Admin. Footer: tagline "Your Heart in Caring Hands" in orange, signed-in user.
- **Top bar**: patient search (name, health card, DOB, phone); "Referral inbox" with count;
  "Book appointment" (secondary); **"+ New encounter"** (orange primary — see `02-new-encounter.md`).
- **Heading**: eyebrow "// TUESDAY · NOVEMBER 25, 2026" (date of the day shown), title "TODAY'S SCHEDULE";
  view switch: By time / By room / By doctor.
- **Four counters**: Booked today · Checked in · Reports to sign · Referrals near 24 h.
- **Appointments table**: Time · Patient (name; sex, age, health card) · Visit (consult or test type) ·
  Room · Staff · Status · "Open chart →". Statuses, each a coloured chip WITH a word:
  Booked, Checked in, In room, Completed, Late (n min). Wide table scrolls sideways on small screens.
- **"Needs attention" panel**: referral near 24 h, critical OLIS lab not reviewed, echo ready to read,
  new HRM report. Each links to the right screen.

## Behaviour
- Defaults to today, the user's clinic, sorted by time; refreshes every minute.
- Clicking a row or "Open chart" opens the patient chart (`03-patient-chart.md`).
- Counts in the sidebar and counters come from live data, not hard-coded.
- Works on a tablet: sidebar stacks above content; toolbar wraps.

## Compared with stock
- Stock: `appointmentprovideradminday.jsp` — provider columns, dense grid, many small links.
- CHC: single list by time (with room/doctor views), patient details on each row, status chips,
  an attention panel, and one obvious "New encounter" action.
- Same data: an appointment made or changed in either view shows in both.

## Done when
- [ ] Shows the same appointments as stock for the same day/clinic/provider
- [ ] Status changes made here appear in stock, and vice versa
- [ ] Counts match the underlying data
- [ ] Tested by Ali with devcontainer test data in both views

## Calendar view (time slots, drag to move) — added 2026-11-25
Mockup: artboard "10 · Day sheet — calendar". A **List | Calendar** switch sits on the day sheet.
- **Columns:** by room (Echo 1, Echo 2, Stress 1, Holter, ECG, Consult 1, Consult 2…, per clinic)
  or by doctor (consultations only — tests stay in room view). Later: by tech.
- **Rows:** 15-minute slots for the clinic's hours (Mon–Sat), current time line, scroll to now.
- **Blocks:** start time, test codes, patient, status, room/doctor; height = duration; colour by
  test code (same colours as list view); red edge = action on abnormal; cancelled hidden unless
  "Show cancelled".
- **Drag to move** an encounter to another time and/or column. On drop:
  - blocked if the slot overlaps another booking in that room/for that doctor (message shown);
  - blocked past clinic hours; warns if the room can't do that test (e.g. GXT into an Echo room);
  - confirmation toast with **Undo**; option "Text the patient the new time" (on by default,
    uses the existing reminder service);
  - moved appointments update in stock OpenO's schedule too (same appointment record) and are audit-logged.
- **Keyboard/touch alternative:** click a block → side panel with Start time and Room/Doctor
  menus (same rules as drag), plus Open chart / Flowsheet.
- **Later:** drag the bottom edge to change duration; drag from a waiting list into a slot;
  1 Day / 3 Days / 1 Week calendar ranges.

## Patient menu on the patient's name — added 2026-11-25
Replaces Cerebrum's "e-Chart" hover list. Full item list and mapping: `09-cerebrum-parity.md`.
- **Opens** when you hover over the patient's name, and also on click or tap (touch screens) and
  with the keyboard (Enter/Space on the name). Esc or moving the mouse away closes it.
- **Header**: patient name, age · sex · health card, and an orange **Open chart** button.
- **Groups**: Chart · Visits & letters · Documents & results · Billing · Patient details, in two
  columns. All 21 Cerebrum links are kept; "VP History" is renamed **Consultation history**.
- **Calendar view**: hovering a block would fight with dragging, so the same menu opens from a
  **Patient menu** button in the "Selected" panel after you click an appointment.
- **Security**: each link checks the user's rights before opening (e.g. billing links need billing
  access); links the user can't use are hidden, not shown greyed out. Opening a chart page writes
  an audit-log entry, as everywhere else.
- **Build note**: draw the menu in a top-level popover layer so it isn't cut off by the table's
  horizontal scroll area on small screens.

### Done when (patient menu)
- [ ] Hover, click, tap and keyboard all open the menu; Esc and mouse-away close it.
- [ ] Every Cerebrum e-Chart item is reachable from it.
- [ ] A user without billing rights doesn't see the Billing group.

