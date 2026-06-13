# Validation

## Proof Strategy

Explain what must pass before the story is done.

## Test Plan

| Layer | Android Test Cases / Strategy |
| --- | --- |
| Unit | JVM JUnit tests targeting domain rules and ViewModels |
| Integration | Robolectric JVM database or networking mock tests |
| E2E | Espresso or Compose UI instrumented tests (on physical/emulator device) |
| Platform | Android Lint static analysis, layout rendering compatibility across OS APIs |
| Performance | Startup times, memory leak profiling (LeakCanary), background battery drain |
| Security / Cryptography | Keystore encryption tests, secure network traffic checks (network security config) |

## Fixtures

List deterministic users, accounts, records, provider responses, or other
fixtures needed for repeatable proof.

## Commands

Add commands after scripts exist.

```text
TBD
```

## Acceptance Evidence

Add results after verification.
