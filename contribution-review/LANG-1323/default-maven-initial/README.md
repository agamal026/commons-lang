# LANG-1323: initial default Maven attempt

This packet preserves the initial failed default build on source
`9f75a9a6c2fcbc3d287a170827f2efe861cbe90d`. The coordinator executed it on
October 18, 2026, from **20:20:26 to 20:23:39 UTC**; Maven exited **1**.
An independent agent audited the complete console, all 316 captured JUnit XML
reports and source hashes. The audit did not run another build.

## Executed results

The XML contains **88,157 unique testcase identities**, with **88,139 passed,
18 skipped, zero failures and zero errors**. No duplicate `(classname, name)`
pairs were found. `TypeUtilsTest` reports **510 tests, zero failures/errors and
one existing skip**; all 14 new hash-interoperability invocations passed.

The initial enforcer, RAT and build-plan checks completed, followed by source
and test compilation, the complete Surefire run and creation of the main, test,
source and test-source JARs. Javadoc then failed while fetching the Commons IO,
Commons Text and Jakarta external documentation links. Each error reports a
PKIX certificate-chain validation failure.

The default build therefore **did not pass**. Successful completion of `verify`
and the later standalone RAT, Checkstyle, japicmp, SpotBugs, PMD and final
Javadoc goals is not established by this attempt.

## Runtime limit and separate retry

The coordinator identified that `MAVEN_OPTS` selected the existing platform
trust store for Maven, while the forked Javadoc JVM did not inherit that
launcher configuration. A scoped `JAVA_TOOL_OPTIONS` propagation of the same
trust store is the planned runtime correction. Certificate validation remains
enabled. Its outcome belongs to a separate receipt; this failure is retained.
No project or test changes are part of that correction.

## Evidence and normalization

[verification.json](verification.json) records the exact command, source and
POM hashes, execution clocks, plugin-stage evidence, all 14 new-case details,
all 18 skip records, and the totals and raw digest of every XML report. Runtime
property blocks from raw JUnit XML are omitted from this public packet.

[maven.log](maven.log) retains the complete console with only the explicitly
listed checkout/JDK/cache path prefixes and Javadoc proxy-host/port argument
values replaced by placeholders. The receipt keeps both the original and
published log hashes and sizes, plus replacement counts. Test outcomes, stage
ordering, errors, JVM option names and timestamps are preserved.

This is local validation evidence. It does not establish upstream submission,
maintainer acceptance, personal human review or a realized benefit.
