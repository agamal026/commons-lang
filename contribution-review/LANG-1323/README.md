# LANG-1323: reflection type hash interoperability

This checkpoint preserves a reproduced defect, the scoped fix and focused
verification. The project's full default build and hosted runtime matrix are
still pending at this checkpoint. This is contributor-fork evidence, not an
upstream submission, maintainer acceptance or a reward.

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

## Independent review and remaining gates

The [independent automated review](source-review.json) read the exact source,
checked all 14 before-and-after cases, verified the unchanged test/POM hashes,
and reversed the two production edits to recover the original source bytes.
It found no blocking correctness or scope issue. This is automated review;
no personal human review or legal signature is asserted.

Complete the project's default Maven command and inspect its actual hosted
jobs before upstream submission. A separate owned validation base,
`30bc4f8db75d8f110f0cce87997ec48a0a5e00cc`, removes only four schedule lines in
two inherited workflow files before Actions is enabled. The proposed source
contains no workflow change; no additional periodic worker is installed.

The [project contribution guide](https://github.com/apache/commons-lang/blob/c89cdf682fe21b0c39759cf58186ed5336a105d4/CONTRIBUTING.md)
and [ASF generative tooling guidance](https://www.apache.org/legal/generative-tooling.html)
were reviewed. OpenAI ChatGPT (GPT-6 Astra) generated the new code, tests,
verification orchestration and explanatory text; a separate AI agent reviewed
the source and execution evidence. The commits include `Generated-by`
provenance. The existing Apache-2.0 source and test scaffolding retain their
license. Conditional project-level reward scope does not establish a paid
assignment, patch eligibility or earnings.
