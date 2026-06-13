# Agent Instructions

This is an Android codebase harness. When working on this project, adhere to the following principles:
- **Language**: Kotlin is the preferred language. Ensure pure Kotlin is used for the Domain layer modules.
- **UI Framework**: Jetpack Compose for all screens and UI components.
- **Dependency Injection**: Use Hilt. Prefer constructor injection and scope components appropriately.
- **Local Storage**: Room database. Maintain schema exports and write migration unit tests for schema changes.
- **Validation**:
  - Run unit tests: `./gradlew testDebugUnitTest`
  - Run static checks and lints: `./gradlew lintDebug`
  - Run instrumented UI tests: `./gradlew connectedAndroidTest` (requires emulator/device)

<!-- HARNESS:BEGIN -->
## Harness

This repo uses Harness. Before work, read:

- `README.md`
- `docs/HARNESS.md`
- `docs/FEATURE_INTAKE.md`
- `docs/ARCHITECTURE.md`
- `docs/CONTEXT_RULES.md`
- `scripts/bin/harness-cli query matrix` on macOS/Linux, or `.\scripts\bin\harness-cli.exe query matrix` on Windows

Use the Rust Harness CLI at `scripts/bin/harness-cli` on macOS/Linux or
`scripts/bin/harness-cli.exe` on Windows as the main operational tool.
<!-- HARNESS:END -->
