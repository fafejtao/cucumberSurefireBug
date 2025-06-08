# Cucumber not executed with maven-surefire-plugin 3.5.3

This repository demonstrates a regression introduced in `maven-surefire-plugin` version **3.5.3** when used with **Cucumber + JUnit Platform**.

## ❗ Problem

With version **3.5.3**, Cucumber tests are **not executed at all** — Maven reports **zero tests run**, even though scenarios exist and are correctly configured.  
In version **3.5.2**, everything works as expected.

This is a silent failure: no errors, no warnings, and the build passes, even if tests should fail.

## ✅ Working with surefire 3.5.2

When run with:
```
./mvnw test
```
Output (as expected):
```
[ERROR] Failures: 
[ERROR]   expected: <11> but was: <15>
[INFO] 
[ERROR] Tests run: 3, Failures: 1, Errors: 0, Skipped: 0
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
```

## ❌ Broken with surefire 3.5.3
```
./mvnw test
```
Output (no cucumber tests executed)
```
[INFO] Running cz.fafejta.SimpleTest
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.015 s -- in cz.fafejta.SimpleTest
[INFO] Running cz.fafejta.RunCucumberTest
[INFO] Tests run: 0, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.063 s -- in cz.fafejta.RunCucumberTest
[INFO] 
[INFO] Results:
[INFO] 
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
```

You can switch the version in pom.xml.

# Notes
The issue only affects Cucumber scenarios executed via JUnit Platform (e.g. `@Suite` + `@IncludeEngines("cucumber")`)

Plain JUnit Jupiter tests are not affected.

The issue is not caused by configuration – same project, same scenario, only the plugin version differs.