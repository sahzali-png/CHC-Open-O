# 09 · Match Cerebrum — feature checklist

CHC's current EMR is **Cerebrum** (AwareMD). Staff rely on these features today; the CHC front end
must cover every one before a clinic switches. Source: Cerebrum screenshots shared by Ali Zaidi,
2026-11-25 (Daysheet, Flowsheet). Screenshots are **not** stored in this repo (they contain patient details).

Status: **Mocked** = in the CHC mockups · **Req** = written into a requirement · **To do** = not yet covered.

## Daysheet (→ CHC Day sheet, `01-schedule.md`, mockup artboard 1)
| Cerebrum feature | CHC | Status |
|---|---|---|
| Office selector; date + Today; 1 Day / 3 Days / 1 Week | Same | Mocked |
| Total appointments count (e.g. 97), 25 per page, page navigation | Same + 50/100/All | Mocked |
| Search by type + patient search | Top search (name, HCN, DOB, phone) | Mocked |
| New Appointment | Book appointment + New encounter | Mocked |
| Devices · Add Patient · Print · Swipe Card · Upload Reports · Bill | Toolbar buttons | Mocked |
| My Tasks count (608) · To Do count | My tasks in toolbar; To Do → Inbox count | Mocked |
| Filters: Doctors, Techs, Test Groups, Status, Clear filters | Same | Mocked |
| Checkboxes: Show expected, Show orders, Exclude test only, Exclude cancelled, Action on abnormal | Same ("Action on abnormal only") | Mocked |
| Filter by patient | Same | Mocked |
| Appointment ID, time, visit-type tag ("Test Only Ext") | Same, colour-coded tag | Mocked |
| Arrived / Left times (editable) | Same | Mocked (edit = To do) |
| Room + staff initials per appointment | Same | Mocked |
| Status incl. "Flowsheet Completed" | Status chip + flowsheet status line | Mocked |
| Patient: name, attending doctor, phone, "Healthcard Validated" | Same, red warning when not validated | Mocked |
| Patient icons: messages, info, tags | ⋮ menu | To do (define icons) |
| Confirmation method ("Confirmed via Text Reminder") | Same | Mocked |
| VP column / chip (= consultation) | Shown as **"Consultation"** (not "VP") | Mocked |
| Test chips with times, colour per test group | Same codes (table below) + full name under the code + time; colour legend | Mocked |
| "Action on Abnormal" per appointment | Red flag + red row edge | Mocked |
| Flow (flowsheet) link | Flowsheet link → Flowsheet page | Mocked |
| Family doctor + fax | Same | Mocked |
| Referral doctor + fax + billing #, house icon when same as family doctor | Same, "also family doctor" label | Mocked |
| Billing type (OHIP) per appointment, editable | Same | Mocked (edit = To do) |
| Row ⋮ menu | Same | Mocked (contents = To do) |
| — (not in Cerebrum screenshot) | **Calendar / time-slot view with drag-to-move** (CHC addition) | Mocked |

## e-Chart menu on the patient name (→ CHC "Patient menu", `01-schedule.md`)
Cerebrum opens a hover list of 21 links ("e-Chart") on the patient's name. CHC keeps every link,
grouped so staff can find them faster. Nothing is removed.

| Cerebrum item | CHC group → label | Status |
|---|---|---|
| Patient Chart | Chart → Patient chart | Mocked (links to Chart) |
| Flow Sheet | Chart → Flowsheet | Mocked (links to Flowsheet) |
| Medications | Chart → Medications | Mocked (page = To do) |
| CPP | Chart → CPP (cumulative patient profile) | Mocked (page = To do) |
| CDF | Chart → CDF | Mocked — **Ali to confirm what CDF holds** |
| Additional Notes | Chart → Additional notes | Mocked (page = To do) |
| Appointments | Visits & letters → Appointments | Mocked (page = To do) |
| VP History | Visits & letters → **Consultation history** | Mocked (links to Consult) |
| Letter History | Visits & letters → Letter history | Mocked (page = To do) |
| External Documents | Documents & results → External documents | Mocked (page = To do) |
| Upload Loose Reports | Documents & results → Upload loose reports | Mocked (page = To do) |
| OLIS Report Search | Documents & results → OLIS report search | Mocked (page = To do) |
| Legacy Data | Documents & results → Legacy data | Mocked (page = To do) |
| EForms | Documents & results → eForms | Mocked (page = To do) |
| Billing History | Billing → Billing history | Mocked (page = To do) |
| Patient Statement | Billing → Patient statement | Mocked (page = To do) |
| Edit Patient | Patient details → Edit patient | Mocked (page = To do) |
| Contact Manager | Patient details → Contact manager | Mocked (page = To do) |
| Patient Alert | Patient details → Patient alert | Mocked (page = To do) |
| Patient Comments | Patient details → Patient comments | Mocked (page = To do) |
| Patient Label | Patient details → Print patient label | Mocked (page = To do) |

## Flowsheet (→ CHC Flowsheet, mockup artboard 9)
| Cerebrum feature | CHC | Status |
|---|---|---|
| Patient name + HIN header | Banner with HIN validated | Mocked |
| Save & Close · End Visit | Same | Mocked |
| Add visit: Doctor, Office, Date, Comments, Add | Same | Mocked |
| Requisition button | Same | Mocked (form = To do) |
| Visit list: date, doctor, tests done, status, tests ordered, billing code, Dx code | Same; test chips link to the test pages | Mocked |
| "No Visit" rows | Same | Mocked |
| Statuses Booked / Flowsheet Completed | + Report signed | Mocked |

## CHC test codes (confirmed by Ali Zaidi, 2026-11-25)
Used on every test chip (day sheet, flowsheet, filters, chart, worklists). Code shown bold, full name under it.

| Code | Shown as | Test page |
|---|---|---|
| VP | **Consultation** (the word, not "VP") | Consult note |
| SE | SE · Stress echocardiogram | Stress echo |
| E1 | E1 · Echo | Echo |
| GXT | GXT · Exercise stress test | Stress test |
| H1 | H1 · 1-day Holter | Holter |
| H2 | H2 · 2-day Holter | Holter |
| H3 | H3 · 3-day Holter | Holter |
| H7 | H7 · 7-day Holter | Holter |
| H14 | H14 · 14-day Holter | Holter |

## Still needed from Cerebrum
- Screens not yet seen: patient chart, test report screens, billing, admin, My Tasks, To Do,
  devices/upload reports, requisition form.
- What "CDF" in the e-Chart menu stands for and holds.
- Codes still unconfirmed: ECG, ABPM, contrast (shown as "ECG", "ABPM", "Contrast" for now),
  and any other test groups in use (e.g. nuclear).
