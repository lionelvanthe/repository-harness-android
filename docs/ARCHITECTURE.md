# Architecture

This document defines the Android architecture guidelines and boundary rules that implementation should follow.

## Architecture Pattern: Clean Architecture + MVVM/MVI

To ensure testability, maintainability, and clear separation of concerns, the project follows Clean Architecture layered in four logical layers:

```text
Domain Layer (Pure Kotlin/Java)
  <- Data / Repository Layer (Room, Retrofit, Android libraries)
      <- Presentation Layer (Jetpack Compose, ViewModels)
          <- UI / Surface Components (Activities, Fragments, Platform Services)
```

### 1. Domain Layer (Core Business Logic)
* **What it contains:** Entities, Value Objects, Use Cases/Interactors, Repository interfaces.
* **Constraints:** Must be a **pure Kotlin library** module. It must have **zero dependencies** on the Android framework, SQLite drivers, UI packages, or third-party serialization libraries (e.g., GSON, kotlinx.serialization).

### 2. Data / Repository Layer (Infrastructure & Storage)
* **What it contains:** Repository implementations, Room DAOs and Databases, API service clients (Retrofit/Ktor), local preferences (Preferences DataStore, EncryptedSharedPreferences).
* **Constraints:** Implements interfaces defined in the Domain layer. Responsible for caching strategies, network-to-local synchronization, and mapping data transfer objects (DTOs) to Domain entities.

### 3. Presentation Layer (UI State Management)
* **What it contains:** ViewModels, UI State definitions, UI Events (MVI), StateFlow/SharedFlow publishers.
* **Constraints:** ViewModels survive configuration changes. They must never reference Android `Context` or View classes directly to prevent memory leaks and ensure JVM testability.

### 4. UI / Surface Components Layer (Visual representation)
* **What it contains:** Jetpack Compose screens/composables, Activities, Fragments, Custom Views.
* **Constraints:** Dumb UI layer. It only listens to UI State emitted by ViewModels and passes user actions back.

---

## Dependency Rule

Outer layers must depend only on inner layers. Dependencies must flow inwards.

| Layer | May depend on | Must not depend on |
| --- | --- | --- |
| **Domain** | Nothing project-external except core language utilities | Android SDK, Room, Retrofit, Jetpack Compose, ViewModels, DI concrete components |
| **Data / Repository** | Domain, standard libraries (Room, Retrofit, DataStore) | Jetpack Compose, ViewModels, Activities, Fragments |
| **Presentation** | Domain (Use Cases), Repository interfaces | Compose UI components directly, Activities, Fragments |
| **UI / Component** | Presentation (ViewModels), Jetpack Compose | Domain internals directly (should communicate via ViewModel/Use Cases) |

---

## Parse-First Boundary Rule

Raw or untrusted data must be parsed, validated, and normalized at the system boundaries before entering the domain layer.

Android boundaries include:
* **Network Responses:** Raw JSON payloads from REST APIs or WebSockets.
* **Local Input:** User input from text fields, forms, or file pickers.
* **System Intents & Deep Links:** Bundle extras, URI query parameters, and custom scheme payloads.
* **Sensors/OS Events:** Location updates, Bluetooth payloads, broadcast receiver extras.

Target flow:
```text
Raw/Untrusted Input
  -> Parser / Validator (e.g., kotlinx.serialization, custom validation)
  -> Typed Data Transfer Object (DTO) / Intent Extra Wrapper
  -> Repository mapper
  -> Domain-safe Type (e.g., UserId, EmailAddress, Amount)
```

Domain models should use custom Kotlin Value Classes (e.g., `@JvmInline value class Email(val value: String)`) to enforce domain rules compile-time rather than carrying raw `String` or `Int` types through the application.

---

## Asynchronous Execution & Concurrency

* **Coroutines & Flow:** Use Kotlin Coroutines for async tasks and `Flow` / `StateFlow` for streaming updates.
* **Dispatchers:** Always inject Coroutine dispatchers (`Dispatchers.IO`, `Dispatchers.Default`, `Dispatchers.Main`) to allow easy testing with `TestDispatcher`. Never hardcode dispatchers inside business logic.
* **Lifecycle Awareness:** Collect UI states safely using lifecycle-aware collectors like `collectAsStateWithLifecycle()` in Compose.

---

## Dependency Injection (DI)

* Use **Hilt** (or Koin) as the Dependency Injection framework.
* Define scopes correctly: `@Singleton` for global data/network sources, `@ActivityRetainedScoped` or `@ViewModelScoped` for presentation scopes.
