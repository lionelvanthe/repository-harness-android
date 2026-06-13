# Feature Intake

Every implementation prompt enters the intake gate before code changes. A new
project spec also enters through this gate before it becomes product docs,
stories, or implementation work.

The human does not need to classify risk. The harness does.

## Intake Flow

```text
User prompt
    |
    v
Classify input type
    |
    v
Restate as work item
    |
    v
Find affected product docs and stories
    |
    v
Run risk checklist
    |
    v
Choose lane: tiny, normal, or high-risk
```

## Input Types

Use the input type to decide where the work should land before choosing the risk
lane.

| Type | Use when | Typical artifact |
| --- | --- | --- |
| New spec | Turning a user-provided project spec into harness-ready docs | Product docs, candidate epics, decisions |
| Spec slice | Implementing selected behavior from an accepted spec | Story packet |
| Change request | Changing, fixing, or refining accepted behavior | Story packet or direct patch |
| New initiative | Adding a larger product area that needs multiple stories | Initiative notes plus story packets |
| Maintenance request | Changing technical, operational, or dependency behavior | Story packet, validation report, or decision |
| Harness improvement | Improving how humans and agents collaborate | Direct docs update or `scripts/bin/harness-cli backlog add` |

Do not create or extend a monolithic spec by default after intake. Use product
docs, stories, decisions, and initiative notes as the living surface.

## Lanes

### Tiny

Use for low-risk docs, resource files (e.g., `strings.xml`, colors, theme styles), layout previews, drawable assets, code formatting, or narrow pure helper functions.

Also use for initial project configuration when the work is limited to adding standard Gradle dependencies (libraries), setup config files, or verifying build compatibility without creating domain modules, local SQLite schema (Room), network call clients, or background scheduling.

Requirements:

- Record the intake row before implementation; tiny work skips story packet overhead, not durable task classification.
- Patch directly.
- Keep affected docs current.
- Run available quick checks (e.g., `./gradlew lintDebug` or `./gradlew testDebugUnitTest`).
- Update the harness only if friction was found.

### Normal

Use for story-sized behavior with bounded blast radius (e.g., adding a single Jetpack Compose screen, a new ViewModel state flow, exposing a new API service endpoint, or writing a simple Room database DAO method).

Requirements:

- Create or update one story file from `docs/templates/story.md`.
- Link relevant product docs.
- Add or update validation expectations (e.g., JVM unit tests or Robolectric tests).
- Implement the smallest vertical slice.
- Record or update proof status with `scripts/bin/harness-cli story add` and `scripts/bin/harness-cli story update`.

### High-Risk

Use when the work can affect user data storage, security keystores, app permissions, background battery/memory usage, complex UI state serialization, deep link intent filters, or multi-threaded background synchronization.

Requirements:

- Create a story folder using `docs/templates/high-risk-story/`.
- Fill in `execplan.md`, `overview.md`, `design.md`, and `validation.md`.
- Ask for human confirmation before implementation if direction is ambiguous.
- Record a durable decision when database schema (Room migrations), encryption methods, custom permissions, background jobs (WorkManager), or validation requirements change. Use a `docs/decisions/NNNN-*.md` file, then add or refresh the durable row with `scripts/bin/harness-cli decision add`.

---

## Risk Checklist

Mark one flag for each item that applies:

| Risk flag | Applies when the work touches |
| --- | --- |
| Auth | Biometrics, OAuth redirects, session tokens, Keystore access, EncryptedSharedPreferences |
| Permissions | Requesting runtime permissions (e.g., Camera, Location, Bluetooth, Android 13+ Notifications) |
| Local Storage | Room database schemas, SQLite migrations, database seed data changes |
| Background Work | WorkManager, Foreground Services, AlarmManager, broadcast receivers, battery optimization gates |
| External SDKs | Firebase, Google Play Services, AdMob, third-party payment gateways, analytics SDK dependencies |
| Public Intents | Deep links, custom schemes, AndroidManifest intent-filters, ContentProviders, exported components |
| UI State / Lifecycle | Configuration changes, ViewModel SavedStateHandle state recovery, complex Compose navigation |
| Existing behavior | Changes to already implemented or unit/UI tested business logic |
| Weak proof | Unclear or missing JVM Unit or Espresso UI tests around the affected area |
| Multi-module | More than one Gradle module or product subdomain changes at once |

---

## Classification

```text
0-1 flags:
  tiny or normal, based on code impact

2-3 flags:
  normal with stronger validation (e.g., adding Robolectric tests in addition to Unit tests)

4+ flags:
  high-risk (requires full story folder and proof strategy)

Any hard gate:
  high-risk unless the human explicitly narrows scope
```

Hard gates:
- Local storage migrations (Room migration paths).
- Native app permissions (camera, location, background sync).
- Auth & secure storage (Keystore configuration).
- Modifying AndroidManifest.xml exported components.
- Removing or weakening validation requirements.

---

## Output

At the end of intake, the agent should be able to say:

```text
Lane: normal
Reason: touches local storage (Room DAO update) and UI state lifecycle.
Docs: database-schema, main-feed-ui.
Story: docs/stories/epics/E02-data-sync/US-014-cache-feed-items.md.
Validation: Unit tests, Robolectric JVM tests.
```
