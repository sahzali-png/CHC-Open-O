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
