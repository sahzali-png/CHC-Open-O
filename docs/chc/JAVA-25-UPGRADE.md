# CHC upgrade: Java 21 → Java 25 LTS

**Branch:** `upgrade/java-25`   **Owner:** Ali Zaidi (product owner/tester)   **Started:** 2026-11-25

## Why
OpenO already runs on Java 21. Java 25 is the current long-term-support release (Sept 2025,
Oracle premier support to Sept 2030). Free Oracle updates for JDK 21 ended in Oct 2026; OpenJDK
builds (e.g. Temurin) still cover 21, so this is planned maintenance, not an emergency.

## Already changed on this branch (NOT yet built or tested)
- `pom.xml`: compiler `source/target 21` → `<release>25</release>`; javadoc `source 25`
- `.devcontainer/development/Dockerfile`: `tomcat:9.0.97-jdk21-temurin` → `tomcat:9.0-jdk25-temurin`
- `.github/workflows/dependency-submission.yml`: `java-version: '25'`

## Task for Claude Code (paste this prompt)

> Finish the Java 25 upgrade on branch `upgrade/java-25`, following `CLAUDE.md` and this file.
> 1. Rebuild the devcontainer, then run `make install --run-tests` and record the baseline result.
> 2. Fix every compile error and warning caused by Java 25. Update build plugins and test libraries
>    that don't support Java 25 (check at least: jacoco-maven-plugin 0.8.11, mockito-core 5.8.0 and
>    its byte-buddy, maven-surefire-plugin 3.2.5, any bytecode/proxy libraries used by Spring 5.3 and
>    Hibernate 5.6). Use the newest patch versions that support Java 25; explain each bump.
> 3. Check Tomcat 9 and the `CATALINA_OPTS --add-opens` flags still start the app cleanly on Java 25.
> 4. Run all modern and legacy tests. Every test that passed on Java 21 must pass on Java 25.
> 5. Start the app and smoke-test with test data: login, open a patient chart, add a note,
>    prescribe a drug, view a lab result, upload a document, create a billing invoice.
> 6. Write a plain-language summary for Ali: what changed, test results before/after, anything
>    that needs the engineer's review, and step-by-step instructions for him to re-test.

## Ali's test checklist (after Claude Code finishes)
- [ ] App starts in the devcontainer, login works
- [ ] Patient search, open chart, add encounter note
- [ ] Prescription: add drug, interaction warning appears, print/fax Rx
- [ ] Labs: open a test lab result (OLIS/HL7 sample)
- [ ] Documents: upload PDF, view it, assign to patient
- [ ] HRM: open a test HRM report
- [ ] Billing: create an Ontario invoice
- [ ] Test counts on Java 25 ≥ Java 21 baseline, no new failures

## What comes after (bigger, separate projects)
- Spring 5.3 is past open-source support (ended 2024). Moving to Spring 6 requires the
  `javax.*` → `jakarta.*` change across ~650 files and Tomcat 10. Plan as its own phase.
- Hibernate 5.6 → 6.x after Spring 6.
- Re-validation with OntarioMD after any change to certified functionality (Ali to coordinate).
