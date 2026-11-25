# CHC upgrade: Java 21 → Java 25 LTS

**Branch:** `upgrade/java-25`   **Owner:** Ali Zaidi (product owner/tester)   **Started:** 2026-11-25
**Status (2026-11-25):** built and tested on Java 25. Needs Ali's re-test in the devcontainer
([JAVA-25-TEST-GUIDE.md](JAVA-25-TEST-GUIDE.md)) and an engineer's review of the items in
"Needs the engineer's review" below.

## Why
OpenO already runs on Java 21. Java 25 is the current long-term-support release (Sept 2025,
Oracle premier support to Sept 2030). Free Oracle updates for JDK 21 ended in Oct 2026; OpenJDK
builds (e.g. Temurin) still cover 21, so this is planned maintenance, not an emergency.

## Summary in plain language
- OpenO now **runs on Java 25** (the devcontainer uses Tomcat 9.0.124 on Temurin JDK 25).
- The code is still **compiled to the Java 21 format** (`<release>21</release>`). Java 25 runs
  Java 21-format code natively, so we get Java 25's support window and security fixes.
  Compiling to the Java 25 format is **not possible yet**: OpenO's Spring 5.3 cannot read Java 25
  class files and the app fails to start (proved below). That step belongs to the Spring 6 project.
- Four test/runtime libraries were updated so they understand Java 25 (table below).
- **No test that passed on Java 21 fails on Java 25.** The same handful of tests fail on both, and
  they were already broken before this work.
- The app starts cleanly on Java 25 and the core screens work with test data.

## What changed (and why)

| File | Change | Why |
|---|---|---|
| `pom.xml` compiler | `<release>25</release>` → `<release>21</release>` | Spring 5.3.39's built-in ASM rejects class files newer than Java 21 (major version 65). With release 25, every `component-scan`/`packagesToScan` fails: *"Unsupported class file major version 69"*. Raise this only after Spring 6. |
| `pom.xml` javadoc | `source 25` → `source 21` | Match the compiler level. |
| `pom.xml` jacoco-maven-plugin | 0.8.11 → **0.8.16** | 0.8.11 supports up to Java 21; Java 25 support arrived in 0.8.14. 0.8.16 is the newest 0.8.x. |
| `pom.xml` mockito-core, mockito-junit-jupiter | 5.8.0 → **5.24.0** | Newest Mockito 5.x; built for current JDKs. |
| `pom.xml` dependencyManagement: byte-buddy, byte-buddy-agent | 1.12.18 / 1.14.10 → **1.18.17** (pinned) | Byte Buddy generates classes at runtime. **Hibernate 5.6 uses it in production** for lazy-loading proxies; Mockito uses it in tests. Maven had been picking Hibernate's old 1.12.18 (Java 20 era) for both. 1.18.x supports Java 25 natively. |
| `pom.xml` dependencyManagement: aspectjweaver | 1.9.7 → **1.9.27** (pinned) | Evaluates the Spring AOP pointcuts in `applicationContext.xml`, `spring_jpa.xml`, `spring_ws.xml` at runtime. 1.9.7 predates Java 17; 1.9.25+ supports Java 25. |
| `pom.xml` surefire `argLine` | added `-XX:+EnableDynamicAgentLoading` | Mockito attaches a Java agent at runtime. JDK 21+ warns about this, and a future JDK will block it unless allowed explicitly. |
| `dependencies-lock*.json` | regenerated | Only the four version bumps above changed. Nothing else moved. |
| `.devcontainer/development/Dockerfile` | `tomcat:9.0-jdk25-temurin` → **`tomcat:9.0.124-jdk25-temurin-noble`** | The floating tag can move to a newer Ubuntu (a `resolute`/26.04 variant now exists), but the Dockerfile's apt package names are for 24.04 "noble". Pinned like the original `9.0.97`. |
| Dockerfile `CATALINA_OPTS` | added `--enable-native-access=ALL-UNNAMED` | Tomcat's JNI loader triggers a Java 22+ "restricted method" warning; a future JDK will block it. Tomcat recommends this flag. Existing `--add-opens java.base/java.net` kept. |
| `CLAUDE.md` | stack description updated | Java 25 runtime, Java 21 bytecode, Tomcat 9.0.124. |

**Deliberately not changed:**
- maven-surefire-plugin 3.2.5: it only launches the test JVM and works on 25.
- maven-compiler-plugin 3.13.0: it passes `--release` through to javac.
- JUnit 5.10.1: runs fine on 25. JUnit 6 is a major upgrade for another day.
- cglib, javassist: no issues found.

## Test results

Same machine, same MariaDB 10.11 test database, same commands as `make install --run-tests`.

| Run | Modern tests (JUnit 5, H2) | Legacy tests (JUnit 4, MariaDB) |
|---|---|---|
| **Before**: Java 21.0.13, pre-upgrade commit `136996a9b` | 282 run, **9 fail** | 3,154 run, **8 fail**, 3 skipped |
| As first committed (`release 25`) on Java 25 | 282 run, **96 fail** (Spring can't load the app) | not run |
| **After**: this branch on Java 25.0.5 | 282 run, **9 fail** (same 9) | 3,154 run, **8 fail** (same 8), 3 skipped |

Pre-existing failures (identical before and after, unrelated to Java):
- Modern: `DashboardManagerEncryptionUnitTest` (7: null mock / unused stubbing),
  `DemographicDaoModernTest.testSpringUtilsIntegration`, `TicklerList2ActionTest.shouldShowAll_whenLengthIsNegative`.
- Legacy: `EDTBaseTest` (3), `FhirMessageBuilderTest` (2), `PatientTest.testGetFhirpatient`,
  `PreventionReportBuilderTest.reportRunner`, `QueueDocumentLinkDaoTest.testGetActiveQueueDocLink`.

Other checks:
- **Compiler output** (warnings/errors from `javac`) is identical on 21 and 25 (none).
- **Annotation processors:** JDK 23+ no longer runs processors found on the classpath automatically
  (log4j-core, openjpa ship some). Compared the compiled output on 21 vs 25: identical file set, so
  OpenO never relied on them.
- **Tomcat 9.0.124 on JDK 25** with the devcontainer `server.xml` and `CATALINA_OPTS`: app deploys
  in ~28 s, no SEVERE errors, login page HTTP 200.
- **Smoke test** (Playwright, synthetic dev data, `openodoc` user):

| Workflow | Result |
|---|---|
| Login → schedule | ✅ |
| Patient search ("FAKE-G") → patient record | ✅ |
| Open eChart, type a note, Sign/Save/Exit | ✅ note saved and signed in `casemgmt_note` |
| Rx screen | ✅ opens. ⚠️ Drug search not tested: needs the separate DrugRef container, not running in the sandbox |
| Inbox, HL7 lab result (urinalysis sample) | ✅ renders with abnormal flags |
| Patient documents list | ✅. ⚠️ Upload not tested |
| Ontario billing form for the patient | ✅ opens. ⚠️ Invoice not submitted |
| HRM report | ⚠️ not tested |

Items marked ⚠️ are in Ali's test guide.

## Needs the engineer's review
1. **Bytecode stays at Java 21.** This is the main judgement call. Moving to `release 25` requires Spring 6
   (`javax` → `jakarta`, Tomcat 10). Nothing in this branch blocks that.
2. **Byte Buddy 1.18.17 under Hibernate 5.6.15 in production.** Hibernate was built against 1.12.x. The
   APIs it uses are stable, all DAO tests pass, and the smoke test saved a note through Hibernate.
   Still, lazy-loading paths deserve attention in QA.
3. **CI may not exercise Java 25 yet.** `maven-project.yml` pulls the prebuilt
   `ghcr.io/<owner>/openo-tomcat-dev:latest`, which stays JDK 21 until `container-images.yml` rebuilds
   it from this Dockerfile on main/develop. Because bytecode is 21, CI stays green on the old image,
   but that proves nothing about 25. `.github/` was not edited (CLAUDE.md protects it). Leftovers there:
   the step name "Set up JDK 21" in `dependency-submission.yml`, and the "Java 21" comment in
   `container-images.yml`.
4. **Pre-existing build fragility (not caused by Java 25):** `com.github.librepdf:openpdf:2.4.0` is
   recorded in the lock files as the **jitpack** build (which pulls in `pdf-renderer`, used by
   `ManageDocument2Action` via `com.sun.pdfview`). Maven Central now hosts a *different* `openpdf:2.4.0`,
   and Central is checked first. A machine with an empty `~/.m2` gets Central's jar, fails the lock
   check, then fails to compile. Existing devcontainers work only because their cache holds the jitpack
   copy. Suggested fix: depend on `org.swinglabs:pdf-renderer` explicitly, or pin the jitpack artifact.
5. **Outbound TLS to provincial systems.** JDK 24/25 tightened TLS defaults (for example RSA
   key-exchange cipher suites and SHA-1 handshake signatures are off by default). OLIS, MCEDT/EDT,
   HRM, Teleplan and fax endpoints could not be reached from the sandbox. Test each in staging on
   Java 25 before production.
6. **Production servers:** whatever runs OpenO outside the devcontainer needs JDK 25 and the
   `--enable-native-access=ALL-UNNAMED` flag in its Tomcat `setenv.sh`/`CATALINA_OPTS`.
7. **Harmless warnings on Java 25** (worth tracking, no action needed now):
   - `sun.misc.Unsafe` deprecation from Netty.
   - Deprecated three-letter time-zone IDs ("EST", "CST"…) enumerated by a library at startup.
   - ~126 javadoc "unknown tag" warnings for legacy comment tags (`@Comment`, `@todo`…), from the
     documentation step only.
   - Maven 3.9's own Guice prints an `Unsafe` warning on JDK 25. Maven 3.9.12+/4 fixes it.
8. Security Manager (removed in JDK 24), `Thread.stop/suspend/resume`, `Subject.getSubject`: grepped,
   none used by OpenO.

## Ali's test checklist
Step-by-step instructions: **[JAVA-25-TEST-GUIDE.md](JAVA-25-TEST-GUIDE.md)**.
- [ ] Devcontainer rebuilds; `java -version` shows 25; app starts, login works
- [ ] Patient search, open chart, add encounter note
- [ ] Prescription: add drug, interaction warning appears, print/fax Rx
- [ ] Labs: open a test lab result (OLIS/HL7 sample)
- [ ] Documents: upload PDF, view it, assign to patient
- [ ] HRM: open a test HRM report
- [ ] Billing: create an Ontario invoice
- [ ] Test counts on Java 25 ≥ Java 21 baseline, no new failures (expected numbers in the guide)

## What comes after (bigger, separate projects)
- Spring 5.3 is past open-source support (ended 2024). Moving to Spring 6 requires the
  `javax.*` → `jakarta.*` change across ~650 files and Tomcat 10. Plan as its own phase.
  **Raising `<release>` to 25 is part of that phase.**
- Hibernate 5.6 → 6.x after Spring 6.
- Re-validation with OntarioMD after any change to certified functionality (Ali to coordinate).
