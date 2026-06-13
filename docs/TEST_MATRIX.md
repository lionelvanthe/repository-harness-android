# Test Matrix

This file maps product behavior to proof.

No product behavior has been defined or implemented yet. Do not mark a row
implemented until tests or validation evidence exist.

## Status Values

| Status | Meaning |
| --- | --- |
| planned | Accepted as intended behavior, not implemented |
| in_progress | Actively being built |
| implemented | Implemented and proof exists |
| changed | Contract changed after earlier implementation |
| retired | No longer part of the product contract |

## Matrix

| Story | Contract | Unit | Integration | E2E | Platform | Status | Evidence |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TBD | Add rows when story packets are created | no | no | no | no | planned | none |

## Evidence Rules

- **Unit proof** covers pure Kotlin/Java domain logic, Use Cases, ViewModels, and data mappers (run via JVM-based JUnit tests: `./gradlew testDebugUnitTest`).
- **Integration proof** covers local SQLite storage caching, Room database migration verification, API networking mock calls (MockWebServer), or Robolectric tests running with Android framework mock interfaces on the JVM.
- **E2E proof** covers user-visible screen flows, animations, and input handling on Compose or XML views (run via Espresso or Compose UI instrumented tests on an emulator/device: `./gradlew connectedAndroidTest`).
- **Platform proof** covers Android Lint static analysis (./gradlew lintDebug), ktlint/detekt formatting, Proguard/R8 shrinking verification, APK size validation, and multi-API version compatibility smoke checks.
- A story can be implemented without every proof column if the story packet explains why.
