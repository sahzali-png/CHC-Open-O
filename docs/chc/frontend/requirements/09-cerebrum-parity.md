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
Cerebrum opens a hover list ("e-Chart") on the patient's name. CHC shows the same list, grouped.
**Rule (Ali Zaidi, 2026-11-25): the menu adds no new features. Every item only opens a screen
that already exists in OpenO**, for that patient. If OpenO has nothing equivalent, the item is
left out until a decision is made — we do not build a new screen for it.

| Cerebrum item | CHC label | Opens this existing OpenO screen (`webapp/…`) |
|---|---|---|
| Patient Chart | Patient chart | eChart — `oscarEncounter/IncomingEncounter.do?demographicNo=` |
| Flow Sheet | Flowsheet | Measurement flowsheet — `oscarEncounter/oscarMeasurements/TemplateFlowSheet.jsp?demographic_no=&template=` (CHC cardiology template is set up in Admin → flowsheets, which is configuration, not code) |
| Medications | Medications | Rx — `oscarRx/choosePatient.do?demographicNo=` |
| CPP | CPP | eChart (the CPP boxes on the chart) |
| Additional Notes | Additional notes | eChart notes — **Ali to confirm** this is where Cerebrum's notes belong |
| Appointments | Appointments | Appointment history — `demographic/demographiccontrol.jsp?displaymode=appt_history` |
| VP History | Consultation history | eChart visit notes — **Ali to confirm** |
| Letter History | Letter history | Consultation requests — `oscarEncounter/oscarConsultationRequest/DisplayDemographicConsultationRequests.jsp?de=` |
| External Documents | External documents | Document manager — `documentManager/documentReport.jsp?function=demographic&functionid=` |
| Upload Loose Reports | Upload loose reports | Document manager, add mode — `documentManager/documentReport.jsp?…&mode=add` |
| OLIS Report Search | OLIS report search | `olis/Search.jsp?demographicNo=` (works once OLIS is certified/enabled) |
| Legacy Data | Legacy data | Document manager, filtered to a "Cerebrum import" document type (a document type is a setting, not new code) — depends on how Cerebrum data is migrated |
| EForms | eForms | `eform/efmpatientformlist.jsp?demographic_no=` |
| Billing History | Billing history | `billing/CA/ON/billinghistory.jsp?demographic_no=` |
| Patient Statement | Patient statement | `billing/CA/ON/endYearStatement.jsp` (year-end statement) — **Ali to confirm** it matches |
| Edit Patient | Edit patient | Master record — `demographic/demographiccontrol.jsp?displaymode=edit&demographic_no=` |
| Contact Manager | Contact manager | `demographic/ManageContacts.jsp?demographic_no=` |
| Patient Alert | Patient alert | Master record, **Alert** field (already shown when the eChart opens) |
| Patient Comments | Patient comments | Master record, **Notes** field |
| Patient Label | Print patient label | `demographic/printDemoLabel.jsp?demographic_no=` |
| CDF | — (left out) | No OpenO match identified yet — **Ali to say what CDF is** before it's mapped |

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
