# 07 · Stress echo page (CHC front end)

Requested by Ali Zaidi, 2026-11-25. Mockup: Design canvas artboard "7 · Stress echo".
Builds on `05-test-pages.md` (upload/extract/check/report) and `06-echo-report-and-viewer.md`
(DICOM measurements, ASE ranges, viewer).

## Two sources, one study
A stress echo study page pulls values from **both**:

1. **Stress ECG — uploaded PDF** from the treadmill/stress system (same extraction as the Stress
   test page): protocol, exercise time, stage, METs, resting and peak heart rate, % age-predicted
   maximum (computed), resting and peak BP, reason for stopping, symptoms, ST changes (leads, mm,
   timing), arrhythmias, recovery, **Duke treadmill score** (computed).
2. **Echo images — DICOM exam** from the echo machine (same reading as the Echo page):
   - rest and peak (and recovery if acquired) LVEF, LV end-systolic volume;
   - **17-segment wall motion** at rest and peak (1 normal · 2 hypokinetic · 3 akinetic ·
     4 dyskinetic), wall motion score index (computed), new or worsening segments highlighted;
   - any other measurements in the exam (e.g. Doppler, RVSP), graded with the ASE ranges.
   - Wall motion is usually scored by the reader, not the machine: the page lets the cardiologist
     click each segment at rest and peak (or opens the viewer side-by-side rest/peak).

Either source can arrive first; the study shows "Stress ECG: waiting for PDF" or
"Echo: waiting for exam" until both are in. Matching is by patient + date.

## Page layout
- Header: patient, date, protocol, reading cardiologist, progress (PDF in · Exam in · Values checked · Signed).
- **Stress ECG panel**: extracted values only (editable, page link, flags, "Not found"), with a
  **"View original PDF"** button to open the document when needed — the PDF itself is not shown on the page.
- **Echo panel**: rest vs peak clip thumbnails (open in viewer, rest/peak side by side), rest/peak
  LVEF and volumes, wall-motion grid for rest and peak with changed segments outlined.
- **Report builder**: one combined CHC stress echo report — exercise findings, ECG findings, echo
  findings, then **Interpretation** and **Conclusion** (e.g. ischaemia yes/no, territory),
  phrase library, Save draft · Preview PDF · **Sign & send** (enabled after both "Values checked").

## Rules
- Same as 05/06: nothing auto-accepted, everything audit-logged, CHC servers in Canada only,
  stock OpenO untouched.
- **Cardiologist sign-off needed** for: computed scores (Duke, % APMHR, WMSI), ischaemia wording,
  phrase library, how "new wall motion abnormality" is defined.

## Done when
- [ ] Stress ECG PDFs from each stress system model extract correctly
- [ ] Rest/peak values and any machine measurements come through from the echo exam
- [ ] Wall-motion scoring at rest and peak works and WMSI is computed correctly
- [ ] Combined signed report appears in stock OpenO and reaches the referrer
