# CHC front end for OpenO — requirements

**Owner:** Ali Zaidi   **Started:** 2026-11-25   **Status:** requirements + mockups, not built

CHC is adding its own branded web front end to OpenO. It runs **alongside** the stock OpenO
screens, never instead of them, so both can be used and compared side by side.

- Mockups (Design canvas): https://claude.ai/artifact/68K1sZNfPV9iuNeT1yAuqu
- Brand / design system: https://claude.ai/artifact/GA8VvDmv1jY95XBeXhKpqM — rules in `design/BRAND.md`

## Two front ends, one EMR

| | Stock OpenO ("Classic") | CHC front end ("CHC") |
|---|---|---|
| Where | Existing JSPs under `src/main/webapp/` | New, self-contained folder `src/main/webapp/chc/` (URL `/oscar/chc/`) |
| Data | Struts actions + JSP | OpenO's existing REST APIs (`/oscar/ws/services/...`) — same database, same records |
| Login | OpenO login | Same OpenO login/session — no second login |
| Look | OpenO default | CHC brand (`design/`) |

### Hard rules
1. **Do not modify, delete or restyle any stock OpenO screen, JSP, action or CSS.** The CHC front end
   is additive only. If a CHC screen needs something the stock code can't give, add a NEW REST
   endpoint (with `SecurityInfoManager.hasPrivilege()` checks); don't change existing ones' behaviour.
2. Every CHC screen links to its stock equivalent ("Open in Classic view") and the stock main
   screen gets one small, optional "Try CHC view" link — the only allowed touch to stock UI, behind
   the feature flag below.
3. Feature flag `chc.ui.enabled` (in `oscar.properties`, default `false`). Off = OpenO is exactly stock.
4. Same permissions as stock: a user who can't do something in Classic can't do it in CHC.
5. All OpenO security rules in `CLAUDE.md` apply: OWASP encoding, no PHI in logs, audit logging,
   CSRF protection on every write.
6. Test data only (devcontainer seed data). Never real patients.

## Screens (build in this order)

| # | Requirement | CHC screen | Stock OpenO equivalent | REST services (existing) |
|---|---|---|---|---|
| 1 | `requirements/01-schedule.md` | Today's schedule | `provider/appointmentprovideradminday.jsp` | `/schedule`, `/providerService`, `/inbox` |
| 2 | `requirements/02-new-encounter.md` | New encounter form (on schedule) | Add appointment + new demographic screens | `/demographics/search`, `/demographics`, `/schedule/add`, `/consults` |
| 3 | `requirements/03-patient-chart.md` | Patient chart | eChart (`casemgmt/`, encounter) | `/demographics/summary`, `/notes`, `/rx`, `/allergies`, `/labs`, `/document` |
| 4 | `requirements/04-referral-worklist.md` | Fax referral worklist | Inbox / document manager | CHC fax intake service (sibling repo `chc-platform`) + `/document` |
| 5 | `requirements/05-test-pages.md` | Test pages: Echo, Holter, ECG, Stress test, Stress echo — PDF upload, value extraction, report builder | Document upload + eForms / letters | chc-platform Module 3 (reporting) + `/document` |
| 6 | `requirements/06-echo-report-and-viewer.md` | Echo report from DICOM measurements + online DICOM viewer (measure, mark up) | Document upload / external PACS | chc-platform Modules 2 & 3 |
| 7 | `requirements/07-stress-echo.md` | Stress echo: stress ECG PDF + DICOM echo values, wall motion rest/peak, combined report | Document upload | chc-platform Module 3 |
| 8 | `requirements/08-consult-note.md` | Consult note: billing + diagnosis codes on top, drag-to-reorder sections, quick phrases per box (add/edit/remove), custom templates | eChart encounter note + billing | `/notes`, `/billing`, `/consults` |

## Comparing the two
- Each requirement file has a **"Compared with stock"** section: what Classic does today and what CHC changes.
- Test plan for each screen: do the same task in Classic and CHC with the same test patient; the
  saved data must be identical (CHC only changes how it looks and flows, not what is stored).

## Open questions
- Framework for `/chc/`: plain JSP + modern CSS (closest to OpenO, simplest) or a React app built
  into the WAR. Decide with the engineer before building screen 1.
- Licensed brand font (Bunken Tech Sans Pro): confirm the web licence allows self-hosting in the EMR.
  Don't commit the font files until confirmed; fall back to system sans.
