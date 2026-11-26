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

Test codes on chips and worklists follow `09-cerebrum-parity.md` → "CHC test codes"
(SE, E1, GXT, H1/H2/H3/H7/H14; consultation shown as "Consultation"). The Holter page covers
all Holter durations; the duration code sets the expected recording length.

## Each test type has two views

### A. Worklist (e.g. "Holter studies")
- Table: patient · date · clinic · technologist · reading cardiologist · status.
- Statuses (chip + word): **Awaiting upload** → **Values to check** → **Ready to report** →
  **Draft** → **Signed** → **Sent**. Filters: my studies, clinic, date, status.
- "Upload PDF" button per row, and drag-and-drop of several PDFs at once (matched to the
  patient by health card + name + date in the PDF; unmatched go to a "Match patient" list).

### B. Study page (one test for one patient)
The page shows the **values read from the PDF — not the PDF itself.** The original document is
one click away: a **"View original PDF"** button (top of the page) opens it in a separate
viewer/tab, and each value's "p. N" link opens it at that page with the value highlighted.
Two parts, side by side on a wide screen, stacked on a tablet:
1. **Extracted values** — grouped table (below, per test). Each value shows:
   - the value and unit, editable;
   - a flag word where outside the normal range (High / Low / Abnormal);
   - "p. N" — opens the original PDF at the page it came from;
   - empty and highlighted red if it couldn't be found ("Not found — please enter").
   Staff/cardiologist must tick **"Values checked"** before the report can be signed.
   Also on the page: **Replace PDF** (upload a corrected file; values re-read, edits kept in audit log).
2. **Report builder** — CHC report template for that test, auto-filled from the checked values:
   header (CHC logo, clinic, patient, referring doctor, test date, technologist), a findings
   paragraph written from the values, then **Interpretation** and **Conclusion** for the
   cardiologist (with a small library of standard phrases they can click to insert). The Holter
   layout below is the pattern; other tests get their own box lists.
   Buttons: **Save draft** · **Preview PDF** · **Sign & send**.
   Sign & send = lock the report → save PDF to the patient's documents in OpenO → fax to the
   referring doctor (and family doctor if set) → status Sent. Edits after signing = addendum.

## Values to extract (first version)

**Holter** — CHC's Holter recorders produce a **Norav Medical "Holter Summary Report"** (Norav
Holter software 4.x). Read these fields from its summary page, in these groups (same as the PDF):
- *General*: duration recorded and analysed (hh:mm), start, stop, channels.
- *Heart rate*: total beats; average, max (with date/time), min (with date/time); bradycardia
  (< 60 bpm) count and longest (with time); tachycardia (> 100 bpm) count and longest (with time);
  pauses (> 2000 ms); min RR and max RR in ms (with times).
- *Ventricular ectopy*: total (% of beats), isolated, bigeminy, trigeminy, couplet, triplet,
  V. tachycardia, fusion.
- *Supraventricular ectopy*: total (% of beats), isolated, pair, run (≥ 3).
- *Atrial fibrillation / flutter*: count, % of analysed time, longest episode.
- *Relative ST analysis*: max elevation (> 1.00 mm), max depression (> 1.00 mm), reference ST per lead.
- *Heart rate variability*: SDNN, SDANN, RMSSD, HRV triangular index; ULF, VLF, LF, HF (ms²).
- *Header*: medications and indications (often blank — fall back to the referral reason).
The Norav page's own "Conclusion" paragraph is not copied; CHC writes its own report.

**Holter report (matches the current Cerebrum "Work Sheet" and CHC's printed Holter report)** —
title "HOLTER MONITOR {24/48/72 …} HRS" from the test code (H1/H2/H3/H7/H14). Header: CHC logo and
clinic address/phone/fax; patient, DOB, age, HIN, sex; family MD; referring MD; visit date.
Report boxes, in this order, each an editable text box:
Reason for test · Interim report · Rhythm · Rate comments · Atrial arrhythmias · SVT ·
Ventricular arrhythmias · Bradyarrhythmias and pauses · Symptoms · Miscellaneous ·
Interpretation · Impression · Recommendations.
- Boxes that can be written from the values start with a **suggested sentence** (marked as a
  suggestion), e.g. Interim report = "Patient {NAME} was monitored for {analysed} hours from {start}
  to {stop}." and Rate comments = "The minimum heart rate was {min} bpm, the maximum heart rate was
  {max} bpm and the mean heart rate was {avg} bpm." Suggestions restate numbers only; grading words
  ("rare", "frequent") and the pause cut-off used in wording (Cerebrum reports say 2.5 s, Norav
  counts > 2.0 s) are **for the cardiologist champion to decide**.
- Each box has the three Cerebrum buttons: **✎ phrases** (pick from a phrase list; the list can be
  edited), **↻ refill from values**, and **⚑ mark abnormal** (box turns red; the appointment shows
  "Action on abnormal" on the day sheet). *Ali to confirm ⚑ is what Cerebrum's lightning button does.*
- Toolbar (as in Cerebrum): View test history · Worklist · Resources · Boxes (All / Filled only /
  Marked abnormal) · status chip · Hide/Show medications (from the OpenO medication list).
- Footer: Reported by (reading cardiologist) · Technologist(s) · "Report finalized" date stamped on
  signing. Interpretation must be filled before signing.
- **Signed PDF** = 2-page CHC report + the Norav summary page appended as page 3 (as today).
- Fax and save to chart use OpenO's existing document and fax features (no new fax system).

**ECG** — rhythm; heart rate; PR, QRS, QT, QTc (and formula); P / QRS / T axes; machine
interpretation statements (shown as "machine read" — never used as the conclusion without the
cardiologist's edit).

**Stress test (GXT)** — CHC's stress system is **GE CASE (V6.73)**. Its PDF export (≈ 22 pages) is:
p.1 Exercise Stress Test Report (header, stage table, auto sentence, machine "Interpretation"
summary) · p.2 **Tabular Summary** · p.3 Graded Exercise Summary (ST by lead) · p.4 Selected
Medians · p.5–8 Sample cardiac cycles · p.9+ ECG strips.
**Every measurement on the GXT page is read from this PDF — nothing is typed in or calculated by
CHC** (Ali Zaidi, 2026-11-25). Fields:
- *Tabular Summary (p.2)*: protocol; total exercise time; max HR; % of max predicted; max predicted
  HR; HR at rest; max BP; BP at rest; max RPP; maximum workload (METs); max ST (mm, slope mV/s,
  lead, stage, time); arrhythmia counts (PVC, PSVC); ST/HR index; reasons for termination.
- *Stage table (p.2)*: phase, stage, time in stage, speed, grade, METs, HR, BP, RPP, ST level.
- *ST by lead (p.3)*: ST level (mm) and slope (mV/s) for 12 leads at baseline and at max ST.
- *Header (p.1)*: test type, attending physician, technician, medications, medical history,
  reason for exercise test ("--" in the PDF = blank).
- *Machine summary (p.1)*: resting ECG, HR response, BP response, chest pain, arrhythmias, ST
  changes — shown read-only as "machine summary", never used as the conclusion. If it disagrees
  with the counts (e.g. "Arrhythmias: none" but PVC 1), the page shows a warning.
- Note: GE's "HR at rest" (summary) can differ from the pretest stage HR; use the summary value.
- Echo-only fields (RVSP, LVEF, E/e′, exercise/supine/upright EF, SaO₂) are **not** on the GXT
  page (the GE PDF has none); they belong to the stress echo page, from DICOM.

**GXT page and report (matches the current Cerebrum GXT "Work Sheet")** — mockup board 11.
- Layout: toolbar (View test history · Worklist · Resources · Boxes filter · status · Save ·
  Sign & send) → report boxes → measurement panel at the bottom (like Cerebrum).
- Report boxes, in this order: Conclusion · Reason for testing · Medications · Protocol ·
  Resting ECG · Stress ECG · Stress test · Reason for termination · Symptoms during the test ·
  Arrhythmias. Each box has ✎ phrases · ↻ refill from measurements · ⚑ mark abnormal (same as Holter).
- **Stress test** box is suggested from the measurements, in CHC's current wording: "The patient
  exercised according to the {PROTOCOL} for {duration} min:s, achieving a work level of Max. METS:
  {METs}. The resting heart rate of {HR rest} bpm rose to a maximal heart rate of {max HR} bpm.
  This value represents {%} % of the maximal, age-predicted heart rate. The resting blood pressure
  of {BP rest} mmHg, rose to a maximum blood pressure of {max BP} mmHg. The exercise test was
  stopped due to {reason}."
- Box suggestions come only from PDF values. Reason for testing and Medications are blank when the
  PDF says "--" (staff can pick phrases); Arrhythmias = the PVC/PSVC counts; Symptoms = chest pain line.
- Measurement panel (bottom): the GE fields above, each with its unit and a "p. N" link to the
  page it came from, plus the stage table and the ST-by-lead table. Editable only to correct a
  misread (edits are audit-logged); no CHC-side calculations.
- **Range colours** use Cerebrum's legend: Normal · Mildly abnormal · Moderately abnormal ·
  Severely abnormal · No range available · Out of range. Thresholds in the mockup are drafts only.
- Conclusion must be filled and "Measurements checked" ticked before signing. Saving to the chart
  and faxing use OpenO's existing document and fax features.

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
