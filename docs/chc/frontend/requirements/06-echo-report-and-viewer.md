# 06 · Echo report page and online DICOM viewer (CHC front end)

Requested by Ali Zaidi, 2026-11-25. Mockups: Design canvas artboards "5 · Echo report" and
"6 · Echo viewer". Back end: chc-platform `docs/requirements/0004-echo-measurements-and-viewer.md`.

## A. Echo report page

### Measurements come from the exam itself
- When an echo exam arrives from the machine (DICOM), **every measurement the sonographer took**
  is read from the exam's structured measurement data (DICOM SR) — no retyping, no PDF.
- Each measurement shows as an **editable text field** on the report page, with its unit,
  the normal range, and a flag word (Normal / Mild / Moderate / Severe, or High / Low).
- Fields are grouped by section, in report order:
  **Left ventricle** (LVIDd, LVIDs, IVSd, LVPWd, LV mass index, LVEF + method, GLS) ·
  **Diastolic function** (E, A, E/A, e′ septal/lateral, E/e′, DT, LA volume index, TR velocity) ·
  **Right ventricle** (RV basal diameter, TAPSE, S′, FAC, RVSP) · **Atria** (LA volume index, RA area) ·
  **Aortic valve** (peak velocity, mean gradient, AVA, DVI, regurgitation grade) ·
  **Mitral valve** (MR grade, EROA, mean gradient) · **Tricuspid / pulmonic** ·
  **Aorta** (root, ascending) · **Pericardium / other**.
- Measurements the exam doesn't contain show empty (never guessed). Any field can be typed in.
- A value changed by hand shows "edited" next to it, and the original machine value stays in the audit log.
- Measurements made later in the viewer (part B) can be sent into these fields with one click.

### Text boxes to explain what the values mean
- **One "Comments" box per section** (e.g. Left ventricle: "Normal size with mild concentric
  hypertrophy; normal systolic function"), plus **Summary / Conclusion** at the end.
- Quick-insert phrases per section (cardiologist-approved library).
- Optional "Suggest wording from values" button: fills the comment boxes with standard phrases
  based on the flags; always editable and never signed without the cardiologist reading it.

### Report flow
Same as other tests (05): statuses, **Values checked**, Save draft, Preview PDF, **Sign & send**
(saves to OpenO chart, faxes to the referring doctor). Thumbnail images can be added to the report.

## B. Online DICOM viewer (doctors and technicians)

Opens from the Echo page, the patient chart and the schedule. Runs in the browser — nothing to install.

### Viewing
- Study browser: all of the patient's echo (and other DICOM) studies; thumbnails of each clip.
- Play cine loops (play/pause, frame-by-frame, speed), 1 / 2 / 4 clips side by side,
  compare with a previous study, zoom, pan, brightness/contrast, full screen.

### Measuring
- Tools: length/distance, area (trace/ellipse), volume (biplane Simpson's), angle,
  Doppler velocity and VTI trace, time interval, heart-rate from two beats.
- Each measurement is labelled (e.g. "LVOT diameter") and appears in a measurement list.
- **Send to report**: puts the measurement into the matching field on the Echo report page.

### Marking up
- Arrows, text labels, freehand drawing, highlight regions, with a choice of colours.
- Mark-ups are saved with the study (never baked into the original images) and can be shown/hidden.

### Working together
- Several people can open the same study at once and see each other's cursor, measurements and
  mark-ups live (e.g. a technologist and a cardiologist reviewing together from different clinics).
- Comments pinned to a frame ("@Dr. Sample — check this jet"), with replies; people get notified.
- "Follow me" mode: everyone sees what the presenter sees.

### Rules
- Same login and permissions as OpenO; every open, measurement, mark-up and comment is audit-logged.
- Images stay on CHC's Canadian servers; viewing is over encrypted connections only.
- **Regulatory:** using the viewer for primary diagnosis may need a Health Canada medical device
  licence. First version is a **review and collaboration** viewer; confirm with a regulatory
  consultant before primary reads.

## Done when
- [ ] Measurements from each echo machine model at CHC appear in the right fields (tested with de-identified exams)
- [ ] Viewer measurements sent to the report land in the right field
- [ ] Two people in the same study see each other's mark-ups live
- [ ] Signed echo report appears in stock OpenO and reaches the referrer
- [ ] Cardiologist champion signed off ranges, flags, phrase library
