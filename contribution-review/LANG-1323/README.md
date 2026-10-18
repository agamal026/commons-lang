# LANG-1323: reflection type hash interoperability

This checkpoint preserves the reproduced defect, scoped fix, completed local
default build and all 18 successful ordinary hosted Maven jobs. The tolerated
experimental Java 28 job failed in 40 date-parser cases; an exact unchanged-
upstream control reproduced all 40 failures. This is contributor-fork validation
evidence. Upstream submission, maintainer acceptance and rewards are separate
events.

## Exact source

| Role | Commit |
| --- | --- |
| Upstream master | `c89cdf682fe21b0c39759cf58186ed5336a105d4` |
| Regression-only checkpoint | `203b78fc407c5a651a42abbcd0d69d36ad348558` |
| Fixed source | `9f75a9a6c2fcbc3d287a170827f2efe861cbe90d` |
| Fixed tree | `8e7432f90518a7173a88ed4cc945303293f9e9c9` |

The proposed upstream diff contains two files, 69 additions and 10 deletions.
The production change is four added lines and ten removed lines in two private
`hashCode()` methods. The 65 added test lines are unchanged between the failing
and passing runs. Canonical commit, tree, branch and complete changed-file
readbacks matched the local source.

## Defect and correction

[LANG-1323](https://issues.apache.org/jira/browse/LANG-1323) reports that built
types compare equal to the corresponding JDK reflection types but have different
hashes. A map or set containing one representation can consequently fail lookup
with the other representation.

The fix returns the component type's hash for a generic array, and combines the
argument-array, owner and raw-type hashes for a parameterized type. Equality,
constructor validation, defensive copies and public APIs remain unchanged.
The existing wildcard correction from
[PR #1807](https://github.com/apache/commons-lang/pull/1807) is preserved. Its
author explicitly identified these two remaining implementations as
[separate work](https://github.com/apache/commons-lang/pull/1807#discussion_r4177989738).

Java SE does not specify the exact hash algorithm for these reflection
interfaces. This change targets interoperability with actual JDK reflection;
the regression compares against the executing runtime instead of hard-coding
numeric hashes or accessing `sun.*` implementation classes. No OpenJDK
implementation source was copied into this contribution.

## Before and after

Both executions used Eclipse Temurin 17.0.20.1+1 and Apache Maven 3.9.16 on
Linux amd64. The project's POM and Maven workflow were unchanged.

| Execution | UTC interval | Actual result |
| --- | --- | --- |
| Regression-only source, unchanged production | 20:07:07–20:08:25, October 18, 2026 | 14 tests, 14 failures, 0 errors, 0 skips |
| Complete `TypeUtilsTest` with fixed production | 20:10:39–20:11:12, October 18, 2026 | 510 tests, 0 failures, 0 errors, 1 existing skip |

The baseline selected `TypeUtilsTest#testHashCodeMatchesJdk`; the fixed run
selected the entire `TypeUtilsTest` class. Each command used Maven's `test`
goal, with private routing and local-cache arguments explicitly elided in the
receipts. Neither focused run is represented as the required default build.

Every baseline case first passed symmetric equality and then failed all five
assertions: equal hashes, `HashMap` lookup in each direction and `HashSet`
lookup in each direction. This is **14 JUnit failures containing 70 assertion
failures**, not 70 test cases. The passing run executed all 14 new invocations
without skips. Its one existing disabled test is
`testIsAssignableGenericArrayTypeToWildcardType`, marked `@Disabled("TODO")`.

The 14 shapes cover type-variable arrays, two-dimensional arrays, parameterized
array components, generic array arguments, top-level and nested parameterized
types, static and parameterized owners, a nongeneric inner member, type-variable
arguments and wildcard compositions.

[baseline.json](baseline.json) and [fixed-typeutils.json](fixed-typeutils.json)
retain all testcase identities and outcomes, complete source hashes, timing,
exit codes and the original JUnit XML digests. Local JVM/Maven property blocks
and repeated stack traces are omitted. The baseline console capture stops after
the antrun heading; its complete fresh JUnit XML, independently inspected, is the
case-level evidence. No full baseline console audit is claimed. The
[passing console](fixed-typeutils.log) includes the final result and
`BUILD SUCCESS`. Only absolute source-checkout and Maven-cache directory
prefixes are replaced with `<source-checkout>` and `<local-cache>`.
`fixed-typeutils.json` records the explicit placeholder map and replacement
counts, retains the original raw capture SHA-256 and size, and separately
identifies the published normalized log. All other log bytes are unchanged.

Three earlier setup attempts stopped before any tests. They are retained as
limits in [setup-limits.json](setup-limits.json): routing configuration, a
damaged local JDK extraction and the vendor runtime's missing platform routing
CA. A separately extracted JDK matched every regular archive file. Maven uses
the existing platform Java trust store with certificate validation enabled.
These setup failures are distinct from the reproduced code defect.

## Complete default build

The final local default Maven build ran from **20:25:37 to 20:30:51 UTC on
October 18, 2026**, using Temurin 17.0.20.1+1 and Maven 3.9.16. It completed
successfully on the fixed source with the production, test and POM bytes
unchanged. The POM selected:

```text
clean verify apache-rat:check checkstyle:check japicmp:cmp spotbugs:check pmd:check javadoc:javadoc
```

Fresh JUnit XML covers **316 suites and 88,157 distinct class/name pairs**:
88,139 passed, 18 skipped, zero failures and zero errors. `TypeUtilsTest`
contains 510 cases, zero failures/errors and one existing skip; all 14 new
invocations passed. The build completed the license, style, compatibility,
static-analysis, coverage and documentation checks selected by the default
goals. The final standalone Javadoc invocation reused documentation generated
earlier in the same build.

The [final default-build packet](default-maven-final/README.md) retains the
complete normalized console, all 316 suite summaries and original XML digests,
source hashes, commands, timestamps and nonfatal warnings. These include cached
SPDX license data, a missing `<bottom>` analysis class, and the PMD auxiliary
classpath warning; the corresponding checks completed successfully, within
their recorded scope.

The [initial default-build attempt](default-maven-initial/README.md), from
20:20:26 to 20:23:39 UTC, is also retained. Its full test suite had the same
88,157/0/0/18 totals, but the build failed in forked Javadoc on HTTPS certificate
validation. The final run propagated the existing platform trust store to that
child JVM through `JAVA_TOOL_OPTIONS`, without disabling TLS validation or
changing source, POM or build goals. The initial failed attempt is not counted
as a successful default build.

## Hosted matrix and experimental control

The [hosted verification packet](hosted/README.md) records every actual job and
its exact checkout, runtime, command, result and supporting console lines.
All **18 ordinary jobs** passed the upstream default Maven command: Java 8, 11,
17, 21, 25 and 27 on Ubuntu, Windows and macOS. Counts vary with the runtime and
are preserved per job. The Java 8 profile omits only the final standalone
Javadoc goal; Javadoc jar generation and Animal Sniffer still ran.

The experimental Java 28-ea job is configured upstream with
`continue-on-error: true`. It failed with 40 Chuvash date-parser cases while
`TypeUtilsTest` passed (551 cases, zero failures/errors, one skip). Consequently,
the Java CI workflow's aggregate success is not an all-19-jobs pass.

The [dedicated control](hosted/java28-control.json) checked out the exact
unchanged upstream base, `c89cdf682fe21b0c39759cf58186ed5336a105d4`, and used
the same Temurin build `28.0.0-ea.20.0.ea` and Maven 3.10.0. It ran only the two
failing date-parser classes: 52,400 cases, 40 failures, zero errors/skips. All
40 final failure summaries match the experimental run exactly and in order
after removal of timestamps and `[ERROR]` prefixes. Both date-test files are
unchanged between base and fix. This establishes that those failures predate
the contribution. The narrow control does not reproduce the full experimental
run's separate JaCoCo class-version-72 diagnostics and does not claim to cover
the full baseline suite.

Both CodeQL jobs completed and uploaded results; their Java autobuild skipped
tests and is recorded as analysis evidence. Dependency Review passed on its
single-job retry after the fork's Dependency graph prerequisite was enabled.
The initial failed attempt remains archived.

The packet contains 23 complete connector-decoded job logs in
[complete-logs.tar.gz](hosted/complete-logs.tar.gz), with per-member hashes in
[the manifest](hosted/log-manifest.json). The very large experimental log could
not be retrieved completely through the connector. Its
[browser receipt](hosted/experimental-java28-browser.json) retains selected
exact results and diagnostics, without claiming a full log archive or digest.

## Independent review and contribution scope

The [independent automated review](source-review.json) read the exact source,
checked all 14 before-and-after cases, verified the unchanged test/POM hashes,
and reversed the two production edits to recover the original source bytes.
It found no blocking correctness or scope issue. This is automated review;
no personal human review or legal signature is asserted.

The default build and actual hosted jobs have been inspected. A separate owned validation base,
`30bc4f8db75d8f110f0cce87997ec48a0a5e00cc`, removes only four schedule lines in
two inherited workflow files before Actions is enabled. The proposed source
contains no workflow change; no additional periodic worker is installed.

The [final evidence review](final-evidence-review.json) independently audits the
focused regression, local full builds and hosted experimental control. The
[pre-submission screen](pre-submission-screen.json) records the dated issue,
policy and competing-PR check; it does not imply issue assignment.

The [project contribution guide](https://github.com/apache/commons-lang/blob/c89cdf682fe21b0c39759cf58186ed5336a105d4/CONTRIBUTING.md)
and [ASF generative tooling guidance](https://www.apache.org/legal/generative-tooling.html)
were reviewed. OpenAI ChatGPT (GPT-6 Astra) generated the new code, tests,
verification orchestration and explanatory text; a separate AI agent reviewed
the source and execution evidence. The commits include `Generated-by`
provenance. The existing Apache-2.0 source and test scaffolding retain their
license. Conditional project-level reward scope does not establish a paid
assignment, patch eligibility or earnings.

## Continuation audit

A fresh independent automated audit on October 18 at 21:05 UTC recovered the
exact source and completed executions without rebuilding them. All 38 evidence
checks passed, including the source/merge comparison, all 23 archived logs,
the 14-case baseline, the complete local XML, and the experimental control.
See [continuation-audit.json](continuation-audit.json). This audit is an evidence
review, not 38 new software tests, human approval or an upstream acceptance.
