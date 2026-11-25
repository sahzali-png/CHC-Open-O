# Java 25 upgrade: test guide for Ali

**Branch:** `upgrade/java-25`. Background and engineering details: [JAVA-25-UPGRADE.md](JAVA-25-UPGRADE.md).

This guide re-checks the upgrade on **your** devcontainer. It takes about **1.5–2 hours**, mostly
waiting for the build and tests. Use only the synthetic development data (patients named `FAKE-…`).
Never use real patient data.

**Pass rule:** everything below behaves the same as it does today on Java 21, and the automated test
counts match the "Expected" numbers in Part 2.

---

## Part 1: Get the branch and rebuild the devcontainer (≈20 min)

1. In VS Code, open the Open-O folder. In the terminal, run:
   ```bash
   git fetch origin upgrade/java-25
   ```
   ```bash
   git checkout upgrade/java-25
   ```
   ```bash
   git pull
   ```
2. Rebuild the container so it picks up the Java 25 image:
   **Ctrl/Cmd + Shift + P → "Dev Containers: Rebuild Container"**.
   - ⚠️ **Do not** clear the Maven cache volume (`open-o_m2-volume`) for this test. See Troubleshooting A.
3. When the container is up, check Java in the VS Code terminal:
   ```bash
   java -version
   ```
   ✅ Expected: `openjdk version "25.0.x"` and `Temurin`. If it still says 21, the rebuild didn't
   happen. Repeat step 2.

## Part 2: Automated tests (≈30–45 min, mostly waiting)

4. Run the full build with all tests:
   ```bash
   make clean
   ```
   ```bash
   make install --run-tests
   ```
5. The output has two summary lines starting with `Tests run:`. One is for the modern tests (first)
   and one for the legacy tests (second, near the end). Compare them with:

   | Suite | Expected on Java 25 (same as Java 21) |
   |---|---|
   | Modern | `Tests run: 282, Failures: 2, Errors: 7, Skipped: 0` |
   | Legacy | `Tests run: 3154, Failures: 8, Errors: 0, Skipped: 3` |

   These 17 failures are **old, known problems** that also fail on Java 21. They are listed in
   JAVA-25-UPGRADE.md under "Test results".
   ✅ Pass: same or better numbers. ❌ Fail: any *new* test name in the failure list. Copy the
   lines starting with `[ERROR]   ` and send them to the engineer.
   - The modern tests use a built-in database. The legacy tests use the `db` container.
   - If your numbers differ from the table *only* because your database has extra local changes,
     note it. The engineer can compare.

## Part 3: Start the app (≈5 min)

6. Start the app with a fresh build, then open the log:
   ```bash
   make install
   ```
   ```bash
   server log
   ```
   ✅ Expected: a line `Server startup in [...] milliseconds` and **no** lines containing `SEVERE`.
   Lines starting with `WARNING:` that mention `sun.misc.Unsafe` or "three-letter time zone ID" are
   expected and harmless. Press **Ctrl+C** to stop watching the log (the app keeps running).
7. Open <http://localhost:8080/oscar> and log in with the dev account:
   username `openodoc`, password `openo2025`, PIN `2025`.
   - If it asks you to change the password, that's a setting in the dev data, not part of this
     upgrade. Set any new dev password and continue.
   ✅ Expected: the day schedule opens.

## Part 4: Hands-on checks (≈40 min)

Do each check and tick it in the checklist in JAVA-25-UPGRADE.md. If something fails, take a
screenshot, note the time, and save the last 200 lines of the log:
```bash
tail -200 /usr/local/tomcat/logs/catalina.out > ~/java25-problem.txt
```

| # | Check | Steps | Expected |
|---|---|---|---|
| 1 | **Patient search & chart** | Click **Search** → type `FAKE-J` → Enter → open **FAKE-Jones, FAKE-Jacky** (#1). Then open the **E-Chart** (encounter) link. | Demographic record shows; eChart opens with the left/right panels (Preventions, Medications, …). |
| 2 | **Encounter note** | In the eChart, click in the note box, type `Java 25 test note`, click the **Sign, Save and Exit** icon on the note toolbar (hover over the icons to see their names). Reopen the eChart. | The note appears at the top of the notes list, signed by you. |
| 3 | **Prescription** | From the eChart open **Medications/Rx**. Search a drug (e.g. `acetaminophen`), add it, then add a second drug that interacts (e.g. `warfarin` + `ibuprofen`). Save, then **Print preview** and **Fax** (fax only if the dev fax is set up). | Drug search returns results; an **interaction warning** appears; the Rx prints/previews as a PDF. *Drug search needs the DrugRef container running. This was not testable in Claude's sandbox, so this check matters.* |
| 4 | **Lab result** | Click **Inbox** → open an HL7 lab (e.g. a urinalysis for "PATIENO TEST FAKE"). Try **Acknowledge**. | Lab displays with abnormal values in red; acknowledge works. |
| 5 | **Documents** | Open patient #1's chart → **Documents** → **Add/Upload**. Upload any small, non-patient PDF (e.g. a blank page). Open it to view. Then in the Inbox, assign an unassigned document to a patient. | Upload succeeds; PDF preview renders; assignment sticks. *Not tested by Claude.* |
| 6 | **HRM** | Patient #1's eChart → **HRM Documents** panel → open a report. | HRM report text displays. *Not tested by Claude.* |
| 7 | **Ontario billing** | From the schedule (or patient #1's record) click **B/Billing**. Enter service code `A007A`, diagnosis `401`, click **Next**, then **Save**. Open the patient's **Billing History**. | Invoice saves and appears in history with the correct fee. *Claude only opened the form; please complete a full invoice.* |
| 8 | **Bonus (optional)** | If you use Claude Code in the devcontainer: run `/ui-tests:test-fullsuite`. It runs the 9 scripted UI tests in `docs/ui-tests/` and compares screenshots. | All 9 tests complete. Note any that differ from their gold-standard screenshots. |

## Part 5: Report back

Send the engineer (or paste into the PR):
1. The two `Tests run:` lines from Part 2.
2. ✅/❌ for each row in Part 4, with screenshots and `~/java25-problem.txt` for any ❌.
3. Anything that *looked* different from Java 21, even if it worked (slower screens, layout changes,
   different date/number formatting).

Date/number formatting is worth a glance: Java 25 ships newer locale data, which can occasionally
change how a date or currency is printed.

---

## Troubleshooting

**A. Build fails with "Dependencies differ … openpdf … wrong integrity" or
"package com.sun.pdfview does not exist".**
This is an older, unrelated build problem, not Java 25. It happens when the Maven cache is empty and
the PDF library downloads from a different server than the lock file expects. Don't clear the cache.
If it was already cleared, ask the engineer; the fix is in JAVA-25-UPGRADE.md, "Needs the engineer's
review", item 4.

**B. `java -version` still shows 21.** The container wasn't rebuilt. Use "Dev Containers: Rebuild
Container", not "Reopen".

**C. Docker can't find `tomcat:9.0.124-jdk25-temurin-noble`.** Check your internet connection to
Docker Hub. The tag existed on 2026-11-25. If it has been withdrawn, tell the engineer.

**D. Login works but a screen shows "HTTP Status 500".** Save the log (command in Part 4) and report
it. That's exactly the kind of problem this test is meant to find.

**E. Going back to Java 21** (e.g. to compare behaviour): check out the branch you normally use
(for example `git checkout main`), then rebuild the container again.
