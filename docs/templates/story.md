# US-XXX Story Title

## Status

planned

## Lane

tiny | normal | high-risk

## Product Contract

Describe the behavior this story must make true.

## Relevant Product Docs

- `docs/product/...`

## Acceptance Criteria

- Criterion 1.
- Criterion 2.
- Criterion 3.

## Design Notes

- Commands:
- Queries:
- API:
- Tables:
- Domain rules:
- UI surfaces:

## Validation

When updating durable proof status, use numeric booleans:
`scripts/bin/harness-cli story update --id <id> --unit 1 --integration 1 --e2e 0 --platform 0`.

| Layer | Expected proof / Command |
| --- | --- |
| Unit | JVM Unit Tests (e.g. `./gradlew testDebugUnitTest`) |
| Integration | Room DAO / Robolectric tests |
| E2E | Compose / Espresso UI Tests (e.g. `./gradlew connectedAndroidTest`) |
| Platform | Android Lint & ktlint formatting (e.g. `./gradlew lintDebug`) |
| Release | Release APK/Bundle optimization & R8 checks |

## Harness Delta

Document any harness updates made or proposed because of this story.

## Evidence

Add commands, reports, screenshots, or links after validation exists.
