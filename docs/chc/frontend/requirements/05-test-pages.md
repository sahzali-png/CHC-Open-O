# 05 · Test pages and report builder (CHC front end)

Requested by Ali Zaidi, 2026-11-25. Mockup (Holter, as the pattern for all): see the Design canvas,
artboard "4 · Holter study".

One page per test type, reached from the sidebar's **Tests & imaging** section:

| Page | Values come from | First version |
|---|---|---|
| **Echo** | Echo machine (DICOM measurements) — full detail in `06-echo-report-and-viewer.md` | Study list + report builder + viewer |
| **Holter** | **Uploaded PDF** from the Holter analysis software | Upload → extract → check → report |
| **ECG** (resting 12-lead) | **Uploaded PDF** from the ECG cart | Upload → extract → check → report |
| **Stress test** (exercise / GXT) | **Uploaded PDF** from the stress system | Upload → extract → check → report |
| **Stress echo** | **Both**: uploaded stress ECG PDF + DICOM echo exam (rest vs peak) — see `07-stress-echo.md` | Combined report |

ABPM gets the same pattern later.

## Each test type has two views

### A. Worklist (e.g. "Holter studies")
- Table: patient · date · clinic · technologist · reading cardiologist · status.
- Statuses (chip + word): **Awaiting upload** → **Values to check** → **Ready to report** →
  **Draft** → **Signed** → **Sent**. Filters: my studies, clinic, date, status.
- "Upload PDF" button per row, and drag-and-drop of several PDFs at once (matched to the
  patient by health card + name + date in the PDF; unmatched go to a "Match patient" list).

### B. Study page (one test for one patient)
Three parts, side by side on a wide screen, stacked on a tablet:
1. **Source PDF** — the uploaded file, page by page, zoomable.
2. **Extracted values** — grouped table (below, per test). Each value shows:
   - the value and unit, editable;
   - a flag word where outside the normal range (High / Low / Abnormal);
   - "found on page N" — clicking it highlights the spot in the PDF;
   - empty and highlighted red if it couldn't be found ("Not found — please enter").
   Staff/cardiologist must tick **"Values checked"** before the report can be signed.
3. **Report builder** — CHC report template for that test, auto-filled from the checked values:
   header (CHC logo, clinic, patient, referring doctor, test date, technologist), a findings
   paragraph written from the values, then **Interpretation** and **Conclusion** for the
   cardiologist (with a small library of standard phrases they can click to insert).
   Buttons: **Save draft** · **Preview PDF** · **Sign & send**.
   Sign & send = lock the report → save PDF to the patient's documents in OpenO → fax to the
   referring doctor (and family doctor if set) → status Sent. Edits after signing = addendum.

## Values to extract (first version)

**Holter** — hook-up date/time, duration and analysed time; total beats; heart rate min / mean /
max (with times); pauses > 2.0 s (count, longest with time); ventricular ectopy (total, % of beats,
couplets, triplets, runs, longest run and rate); supraventricular ectopy (total, %, runs,
longest run); atrial fibrillation/flutter burden (%); patient diary events and the rhythm at
each; ST changes if reported.

**ECG** — rhythm; heart rate; PR, QRS, QT, QTc (and formula); P / QRS / T axes; machine
interpretation statements (shown as "machine read" — never used as the conclusion without the
cardiologist's edit).

**Stress test (GXT)** — protocol (e.g. Bruce); exercise time; stage reached; METs; resting and
peak heart rate; % of age-predicted maximum (computed); resting and peak blood pressure;
reason for stopping; symptoms (chest pain yes/no, limiting or not); ST changes (lead, mm, when);
arrhythmias; recovery. **Duke treadmill score** computed from time, ST deviation and angina.

**Stress echo** — all GXT values + rest vs peak wall motion by segment, wall motion score
index, LVEF rest/peak.

**Echo** — LVEF (and method), LV dimensions and wall thickness, LA volume index, diastolic
values (E/A, E/e′), RV function (TAPSE), RVSP, aortic root, valve findings.

## Rules
- Extraction is a **suggestion**: nothing goes into a signed report until a person ticks
  "Values checked" (chc-platform hard rule 4).
- **Normal ranges, the findings sentences, phrase library and computed scores need cardiologist
  sign-off** before use with patients.
- Each machine/vendor PDF layout needs its own extraction template. Before building: collect
  2–3 **de-identified** sample PDFs per machine model at each clinic (list models in Discovery).
- PDF reading happens on CHC's servers in Canada (local text extraction/OCR) — no outside service.
- Every upload, view, edit, sign and send is audit-logged.
- Stock OpenO stays untouched; signed reports appear in stock OpenO as normal patient documents.

## Where it is built
- Pages: CHC front end in this repo (`/oscar/chc/tests/...`).
- PDF extraction + report generation: chc-platform **Module 3** (`services/reporting/`),
  requirement `docs/requirements/0003-test-report-extraction.md` there.

## Done when (per test type)
- [ ] Sample PDFs from each machine model extract correctly (every value checked against the PDF)
- [ ] Missing values are flagged, never guessed
- [ ] A signed report appears in the stock OpenO patient documents and reaches the referrer by fax
- [ ] Cardiologist champion signed off ranges, wording and scores
