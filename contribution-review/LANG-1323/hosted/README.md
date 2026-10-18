# Hosted verification of the Commons Lang hash-code fix

All **18 ordinary Maven matrix jobs succeeded**, with complete console logs independently checked. The **experimental Java 28-ea job failed** under the upstream workflow's `continue-on-error: true`. Consequently, the Java CI run's aggregate success must not be described as all 19 jobs passing. The coordinator's read-only browser extraction identifies 40 Chuvash date-parser failures in the experimental run. An exact unchanged-upstream control reproduces all 40 final failure summaries with the same actual Java build, establishing that those failures predate the fix. The connector could not retrieve that job's complete log; selected exact browser lines are archived separately.

This receipt covers the owned draft [PR 1](https://github.com/agamal026/commons-lang/pull/1). It is an automated independent evidence review and does not assert human approval or upstream acceptance.

## Exact source and checkout

- Source head: `9f75a9a6c2fcbc3d287a170827f2efe861cbe90d`.
- Owned CI base: `30bc4f8db75d8f110f0cce87997ec48a0a5e00cc`.
- Tested merge: `22d6135d62cc957e77777fcd9c1ebb8a4cc783c5`.
- Merge tree: `e0532a88f9d9ae3263815f71a122bfdb5f273105`.

Each of the 22 complete logs from the three main workflows records the exact tested merge after checkout. A separate complete control log records the explicitly requested unchanged Apache upstream checkout. A local Git comparison independently confirmed that the merge has the same production file, test file, POM, and Maven workflow as the source head. The only source-head-to-merge differences are four removed schedule lines in the CodeQL and Scorecards workflows on the owned CI base. See [owned merge receipt](../owned-merge.json) and [structured hosted audit](verification.json).

## Maven jobs and test results

All ordinary jobs ran the unchanged upstream command:

```text
mvn --errors --show-version --batch-mode --no-transfer-progress -Ddoclint=all
```

This selects the POM's default goals: `clean verify apache-rat:check checkstyle:check japicmp:cmp spotbugs:check pmd:check javadoc:javadoc`. The Java 8 profile omits the final `javadoc:javadoc` goal; Javadoc jar generation still runs in the lifecycle.

Every successful job below reports **0 test failures and 0 test errors** in both the whole-suite and TypeUtils summaries. The total includes skipped cases. TypeUtils is shown as total / skipped.

| Actual Java | Runner target | Vendor | Maven | Whole-suite total | Whole-suite skipped | TypeUtils total / skipped | Actual job |
|---|---|---|---|---:|---:|---:|---|
| 1.8.0_504 | macos-latest | Azul Systems, Inc. | 3.10.0 | 24,766 | 2,466 | 461 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871801) |
| 1.8.0_504 | ubuntu-latest | Temurin | 3.10.0 | 24,692 | 2,466 | 461 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871627) |
| 1.8.0_504 | windows-latest | Temurin | 3.9.16 | 24,692 | 2,466 | 461 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871749) |
| 11.0.32.1 | macos-latest | Eclipse Adoptium | 3.10.0 | 68,247 | 18 | 506 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871736) |
| 11.0.32.1 | ubuntu-latest | Eclipse Adoptium | 3.10.0 | 68,247 | 18 | 506 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871669) |
| 11.0.32.1 | windows-latest | Eclipse Adoptium | 3.9.16 | 68,247 | 18 | 506 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871699) |
| 17.0.20.1 | macos-latest | Eclipse Adoptium | 3.10.0 | 88,157 | 18 | 510 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871764) |
| 17.0.20.1 | ubuntu-latest | Eclipse Adoptium | 3.10.0 | 88,157 | 18 | 510 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871619) |
| 17.0.20.1 | windows-latest | Eclipse Adoptium | 3.9.16 | 88,157 | 18 | 510 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871644) |
| 21.0.12.1 | macos-latest | Eclipse Adoptium | 3.10.0 | 92,019 | 18 | 524 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871769) |
| 21.0.12.1 | ubuntu-latest | Eclipse Adoptium | 3.10.0 | 92,019 | 18 | 524 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871672) |
| 21.0.12.1 | windows-latest | Eclipse Adoptium | 3.9.16 | 92,019 | 18 | 524 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871779) |
| 25.0.4.1 | macos-latest | Eclipse Adoptium | 3.10.0 | 98,609 | 18 | 528 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871811) |
| 25.0.4.1 | ubuntu-latest | Eclipse Adoptium | 3.10.0 | 98,609 | 18 | 528 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871686) |
| 25.0.4.1 | windows-latest | Eclipse Adoptium | 3.9.16 | 98,609 | 18 | 528 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871507) |
| 27 | macos-latest | Eclipse Adoptium | 3.10.0 | 100,926 | 70 | 551 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871866) |
| 27 | ubuntu-latest | Eclipse Adoptium | 3.10.0 | 100,926 | 70 | 551 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871666) |
| 27 | windows-latest | Eclipse Adoptium | 3.9.16 | 100,926 | 70 | 551 / 1 | [Success](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871751) |

The different counts are preserved exactly from each runtime's console; they are not added together as unique test coverage. In particular, macOS Java 8 used Azul and reported 24,766 cases, while its Temurin counterparts reported 24,692.

The logs verify RAT, Checkstyle, japicmp, SpotBugs, PMD, and JaCoCo checks and final `BUILD SUCCESS`. All jobs report zero Checkstyle violations, zero SpotBugs bug instances and errors, and satisfied JaCoCo coverage checks. Animal Sniffer runs in the Java 8 profile. Selected gate executions and supporting log lines are retained per job in [verification.json](verification.json).

The passing hosted console reports TypeUtils class totals; it does **not enumerate the 14 new parameterized invocations**. Their individual fail-before/pass-after proof is in the local JUnit XML reviewed in [source-review.json](../source-review.json). No hosted per-case result is inferred from a class-level summary.

### Experimental failure

[Java 28-ea job 117779871695](https://github.com/agamal026/commons-lang/actions/runs/39162942665/job/117779871695), attempt 1, failed in `Build with Maven`. The step ran from 20:22:02 to 20:26:02 UTC on 2026-10-18. Actual job metadata establishes the failure. The [browser receipt](experimental-java28-browser.json) records Eclipse Temurin 28-beta (28.0.0-ea.20.0.ea), Maven 3.10.0, and Ubuntu. Its final Results excerpt has 100,926 tests, 40 failures, 0 errors, and 18 skips: 4 failures in FastDateParserJava15BugTest and 36 in FastDateParserTest, involving Chuvash locales cv, cv_RU, and cv_RU_#Cyrl. TypeUtils reports 551 cases, 0 failures/errors, and 1 skip. JaCoCo also reports unsupported class-file major version 72, but this is not the sole fatal result. The receipt preserves selected exact lines from the browser raw document, not a complete log archive or raw-byte digest. The [unchanged upstream control](https://github.com/agamal026/commons-lang/actions/runs/39163962620) completed at c89cdf682fe21b0c39759cf58186ed5336a105d4 using the same actual Temurin 28-beta build and Maven 3.10.0. Its complete log reports 52,400 tests, 40 failures, 0 errors, and 0 skips across the same two date classes. All 40 final failure-summary lines match the experimental excerpt exactly and in order after removal of timestamps and [ERROR] prefixes. The two date-test source blobs are identical between the base and the fix. See [java28-control.json](java28-control.json) and `job-117782918411.log` in [the complete-log archive](complete-logs.tar.gz). This narrow control did not run TypeUtils or the full baseline suite, and it contains no unsupported-major-72 instrumentation messages; that separate diagnostic remains distinct.

No workflow rerun or CI change was performed by this auditor.

## CodeQL and Dependency Review

[CodeQL run 39162942652](https://github.com/agamal026/commons-lang/actions/runs/39162942652) completed successfully for Java and JavaScript using CodeQL CLI 2.27.2. Both full logs show successful result upload. The Java autobuild explicitly used `-DskipTests` and several plugin skip flags and printed `Tests are skipped`; it is an analysis run, not an additional test or default-build pass. Successful analysis is not a claim that the repository has zero vulnerabilities.

[Dependency Review run 39162942724](https://github.com/agamal026/commons-lang/actions/runs/39162942724) retains both actual attempts. Job 117779871772 failed because Dependency graph was not enabled. The coordinator retried that single failed job after enabling the prerequisite. Retry job 117780547870, attempt 2, succeeded and reported no low-or-higher vulnerable packages and no denied packages for the reviewed dependency diff. Both complete logs are retained.

## Evidence files and limits

[log-manifest.json](log-manifest.json) lists the 23 complete logs (22 main-workflow logs and one unchanged-upstream control), their retrieval times, byte counts, SHA-256 digests, and Git blob hashes. [LOG_SHA256SUMS](LOG_SHA256SUMS) is suitable for verifying the archived logs. The 23 log files are packaged in [complete-logs.tar.gz](complete-logs.tar.gz); `file` names in the JSON receipts refer to archive members. Extract the archive in this directory with `tar -xzf complete-logs.tar.gz`, then run `sha256sum -c LOG_SHA256SUMS`. No log content was changed during packaging. The public copies are unchanged from the archived UTF-8 encoding of the connector-decoded text, preserving the returned BOM and line endings. The connector does not expose original transport bytes.

The build logs retain nonfatal compiler, SPDX, classpath, metadata-certificate, and missing-analysis-class warnings. In particular, the Java 8 SPDX plugin warns about the license-list version format; several SpotBugs runs mention a missing `<bottom>` class before reporting zero errors and succeeding. These warnings did not fail the ordinary builds. An unchanged control was not run to establish whether each warning predates this patch.

Final run and actual-job metadata were observed at 20:29:46–20:29:47 UTC on 2026-10-18. The structured audit contains the exact per-call timestamps. The experimental failure diagnosis was observed by the coordinator at 20:36:48 UTC. The control metadata was observed at 20:38:56 UTC and its full log retrieved at 20:39:02 UTC. The hosted audit is complete for this exact source, with the experimental failure and collection limit retained.
