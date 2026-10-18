# LANG-1323: completed local default Maven build

The coordinator reran the unchanged project default on exact source
`9f75a9a6c2fcbc3d287a170827f2efe861cbe90d`, from **20:25:37 to 20:30:51 UTC
on October 18, 2026**. Maven exited **0** and the complete console ends in
`BUILD SUCCESS`. Source, test and POM hashes match the initial failed attempt.

## Tests and default gates

An independent audit read all **316 JUnit XML reports**: **88,157 unique
testcase identities**, comprising **88,139 passed and 18 skipped**, with zero
failures/errors and no duplicate `(classname, name)` pairs. `TypeUtilsTest`
reported **510 tests, zero failures/errors and one existing skip**; all **14**
new hash-interoperability cases passed.

The unmodified default goal is:

```text
clean verify apache-rat:check checkstyle:check japicmp:cmp spotbugs:check pmd:check javadoc:javadoc
```

The complete console verifies progression through each required goal. RAT
reports no issues, Checkstyle reports zero violations, japicmp completes and
writes its reports, SpotBugs reports zero bug instances/errors, PMD completes,
and Javadoc generation succeeds. JaCoCo also reports that all configured
coverage checks were met. The final standalone Javadoc goal finds the output
from the successful lifecycle generation current and skips regenerating it.

## Runtime recovery and retained limits

The [initial default attempt](../default-maven-initial/README.md) remains
recorded as failed: the forked Javadoc JVM could not validate certificates for
three external documentation links. The retry propagated the **existing
platform Java trust store** through command-scoped `JAVA_TOOL_OPTIONS` to
child JVMs. Console option messages and all 316 XML property blocks confirm
that selection. Certificate validation stayed enabled; source and build
configuration were unchanged.

The successful build retains nonfatal diagnostics:

- SPDX could not resolve `spdx.org` and used its cached license metadata.
  Current remote license freshness is not established.
- SpotBugs analysis prints a missing `<bottom>` class before the configured
  check reports zero bug instances/errors; exhaustive analysis is not claimed.
- PMD warns about automatically selecting the current JDK auxiliary classpath.
- CycloneDX retains schema-keyword warnings. Existing remote-resource,
  site-descriptor and report-generation skips remain in the log.

These diagnostics were not suppressed and do not replace the actual Maven
exit code. No extra build or source edit was performed by the independent audit.

## Evidence

[verification.json](verification.json) contains the complete suite digest
manifest, unique-identity digest, all new-case and skipped-case details, exact
source hashes, execution clocks and line references for every default goal.
[maven.log](maven.log) preserves the full console with explicitly counted path
and routing placeholders. Original and normalized log hashes/sizes are
recorded separately. Raw XML runtime-property blocks are omitted publicly.

This completes local default-build validation. Hosted matrix execution,
upstream review/acceptance and benefits remain separate observations.
