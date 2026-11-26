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
| VP column | — | **To do — confirm what VP means at CHC** |
| Test chips with times, colour per test group (SE, H3, E1, GXT, VP, contrast) | Chips with full names + times, colour legend | Mocked |
| "Action on Abnormal" per appointment | Red flag + red row edge | Mocked |
| Flow (flowsheet) link | Flowsheet link → Flowsheet page | Mocked |
| Family doctor + fax | Same | Mocked |
| Referral doctor + fax + billing #, house icon when same as family doctor | Same, "also family doctor" label | Mocked |
| Billing type (OHIP) per appointment, editable | Same | Mocked (edit = To do) |
| Row ⋮ menu | Same | Mocked (contents = To do) |

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

## Still needed from Cerebrum
- Screens not yet seen: patient chart, test report screens, billing, admin, My Tasks, To Do,
  devices/upload reports, requisition form.
- Test-group codes used at CHC and their meanings (SE, H3, E1, GXT, VP, contrast…), so the CHC
  chips use the same codes staff know.
