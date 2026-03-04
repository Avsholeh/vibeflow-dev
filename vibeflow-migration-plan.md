# VibeFlow Architecture Migration Plan

## Context

VibeFlow currently follows a Clean Architecture pattern with feature-first organization. This migration aligns it with Flutter's official 2026 architecture guidelines which recommend:

1. **MVVM pattern** with explicit ViewModels (using Riverpod 3.x AsyncNotifier)
2. **Feature-first UI organization** (presentation layer grouped by feature)
3. **Layer-first data/domain organization** (grouped by type)
4. **Use cases layer** in domain for business logic encapsulation
5. **Formalized dependency injection** using Riverpod providers

### Why This Migration Matters

**Feature-First for UI (presentation/):**
- **Discoverability:** All UI code for a feature is in one place
- **Team Collaboration:** Teams can own entire features independently
- **Scalability:** Features can be extracted to separate packages if needed
- **Clear Boundaries:** Each feature has well-defined interfaces

**Layer-First for Data/Domain:**
- **Code Reuse:** Shared models and repositories across features
- **Testing:** Easier to mock and test at layer boundaries
- **Dependency Flow:** Clear dependency direction (UI → Domain → Data)
- **Separation of Concerns:** Each layer has a single responsibility

### Dependency Flow

```
UI Layer (features/*/presentation/)
    ↓ depends on
Domain Layer (domain/ + features/*/domain/)
    ↓ depends on
Data Layer (data/ + features/*/data/)
```

### State Management: Riverpod 3.x AsyncNotifier

The new pattern uses Riverpod's AsyncNotifier for clean async state management:

```dart
// Example ViewModel pattern
class LoginViewModel extends AsyncNotifier<LoginState> {
  @override
  LoginState build() => LoginState.idle;

  Future<void> login(String email, String password) async {
    state = const AsyncValue.loading();
    state = await AsyncValue.guard(() async {
      final useCase = ref.read(loginUseCaseProvider);
      return await useCase(email, password);
    });
  }
}

final loginViewModelProvider = AsyncNotifierProvider<LoginViewModel, LoginState>(
  LoginViewModel.new,
);
```

## Current Structure Analysis

**What VibeFlow Currently Has:**
```
lib/features/[feature_slug]/
├── domain/
│   ├── models/           # Domain entities
│   └── repositories/     # Repository interfaces
├── data/
│   ├── datasources/
│   └── repositories/     # Repository implementations
├── providers/            # State management (ChangeNotifier)
├── screens/              # UI screens
└── widgets/              # Feature-specific widgets
```

**What Flutter 2026 Recommends:**
```
lib/features/[feature_slug]/
├── presentation/
│   ├── pages/            # Full-screen widgets
│   ├── view_models/      # MVVM ViewModels (Riverpod AsyncNotifier)
│   └── widgets/          # Feature-specific UI components
├── domain/
│   ├── entities/         # Pure business models (renamed from models/)
│   ├── repositories/     # Repository interfaces
│   └── usecases/         # Business logic use cases (NEW)
└── data/
    ├── datasources/      # Remote/local data sources
    ├── models/           # DTOs for API/DB (NEW)
    └── repositories/     # Repository implementations
```

## Migration Strategy

### Phase 1: Update Folder Structure

**1.1 Rename folders in each feature:**
- `screens/` → `presentation/pages/`
- `widgets/` → `presentation/widgets/`
- `providers/` → `presentation/view_models/`
- `domain/models/` → `domain/entities/`

**1.2 Add new folders:**
- `domain/usecases/` - Business logic use cases
- `data/models/` - DTOs for API/Database responses

### Phase 2: Migrate State Management to MVVM with Riverpod

**2.1 Update ViewModels:**
- Migrate from ChangeNotifier to Riverpod 3.x AsyncNotifier
- Follow naming pattern: `[Feature]ViewModel` or `[Screen]ViewModel`
- Use AsyncValue for state management
- Implement proper loading, error, and success states

**2.2 Create Use Cases:**
- Extract business logic from ViewModels into use cases
- Each use case should handle a single business operation
- Use cases interact with repositories

**2.3 Update Repository Pattern:**
- Keep interfaces in `domain/repositories/`
- Keep implementations in `data/repositories/`
- Add DTO conversion in data layer

### Phase 3: Update UI Layer

**3.1 Update Screens to Pages:**
- Rename `*_screen.dart` to `*_page.dart`
- Rename `*_view.dart` to `*_view_page.dart`
- Rename `*_page.dart` to `*_screen.dart` (the production wrapper)

**3.2 Implement MVVM Pattern:**
- Pages use `ConsumerWidget` or `ConsumerStatefulWidget`
- Watch ViewModels using `ref.watch()`
- Call ViewModel methods using `ref.read()`

### Phase 4: Update Core Architecture

**4.1 Create DI Setup:**
- Add `lib/core/di/` folder for dependency injection
- Create `lib/core/di/providers.dart` for app-wide providers
- Use Riverpod providers for all dependencies

**4.2 Update Core Structure:**
```
lib/core/
├── di/
│   └── providers.dart        # App-wide Riverpod providers
├── theme/
│   └── app_theme.dart        # Theme configuration
├── routing/
│   └── app_router.dart       # Route configuration (go_router/auto_route)
├── widgets/
│   └── shared_widgets.dart   # Reusable widgets
├── utils/
│   └── constants.dart        # App constants
└── network/
    └── api_service.dart      # HTTP client setup
```

### Phase 5: Update VibeFlow Workflow Files

**5.1 Update CLAUDE.md:**
- Update architecture documentation to reflect MVVM pattern
- Update folder structure references
- Add Riverpod/AsyncNotifier patterns
- Update state management guidelines
- Add use cases pattern

**5.2 Update Commands:**
- `.claude/commands/vibeflow/feature.md` - Update to generate MVVM structure
- `.claude/commands/vibeflow/new.md` - Update spec templates
- `.claude/commands/vibeflow/validate.md` - Update validation rules
- `.claude/commands/vibeflow/status.md` - Update progress tracking

## Target Structure

### Complete lib/ Directory Tree

```
lib/
├── main.dart                          # App entry point with ProviderScope
├── app.dart                           # Root widget (MaterialApp/CupertinoApp)
│
├── core/                              # Shared app-wide code
│   ├── di/
│   │   └── providers.dart             # Riverpod providers for DI
│   ├── theme/
│   │   ├── app_theme.dart             # Theme configuration
│   │   ├── colors.dart                # Color tokens
│   │   └── typography.dart            # Typography setup
│   ├── routing/
│   │   ├── app_router.dart            # Route configuration (go_router)
│   │   └── route_guard.dart           # Navigation guards
│   ├── widgets/
│   │   ├── app_scaffold.dart          # Shared scaffold wrapper
│   │   ├── error_widget.dart          # Error display widget
│   │   ├── loading_widget.dart        # Loading indicator
│   │   └── empty_state_widget.dart    # Empty state display
│   ├── utils/
│   │   ├── constants.dart             # App constants
│   │   ├── extensions.dart            # Dart extensions
│   │   └── validators.dart            # Input validators
│   └── network/
│       ├── api_service.dart           # HTTP client (Dio/http)
│       ├── api_interceptor.dart       # Request/response interceptor
│       └── api_exception.dart         # Error handling
│
├── data/                              # Data layer (layer-first)
│   ├── datasources/
│   │   ├── local/
│   │   │   ├── local_storage_service.dart   # SharedPrefs/SQLite
│   │   │   └── cache_service.dart           # Caching layer
│   │   └── remote/
│   │       └── api_datasource.dart          # REST API client
│   ├── models/                         # DTOs (Data Transfer Objects)
│   │   ├── user_model.dart                   # API response models
│   │   └── auth_response_model.dart
│   └── repositories/                    # Concrete implementations
│       ├── auth_repository_impl.dart
│       └── user_repository_impl.dart
│
├── domain/                            # Domain layer (layer-first)
│   ├── entities/                      # Pure business models
│   │   ├── user.dart                  # Core domain entities
│   │   └── auth_session.dart
│   ├── repositories/                  # Abstract interfaces
│   │   ├── auth_repository.dart
│   │   └── user_repository.dart
│   └── usecases/                      # Business logic (NEW)
│       ├── login_usecase.dart
│       ├── logout_usecase.dart
│       ├── get_user_profile_usecase.dart
│       └── update_user_profile_usecase.dart
│
└── features/                          # UI layer (feature-first)
    ├── core/                          # Shared UI utilities
    │   ├── base_view_model.dart       # Base ViewModel class
    │   ├── base_page.dart             # Base page class
    │   └── base_state.dart            # Common state enums
    │
    ├── auth/                          # Feature: Authentication
    │   ├── presentation/
    │   │   ├── pages/
    │   │   │   ├── login_page.dart           # Login screen UI
    │   │   │   ├── register_page.dart        # Registration screen
    │   │   │   └── forgot_password_page.dart
    │   │   ├── view_models/
    │   │   │   ├── login_view_model.dart     # Login state & logic
    │   │   │   ├── register_view_model.dart
    │   │   │   └── auth_view_model.dart      # Shared auth VM
    │   │   └── widgets/
    │   │       ├── login_form.dart           # Reusable form widget
    │   │       └── social_login_button.dart
    │   ├── domain/
    │   │   ├── usecases/
    │   │   │   ├── login_usecase.dart
    │   │   │   ├── register_usecase.dart
    │   │   │   └── logout_usecase.dart
    │   │   └── repositories/
    │   │       └── auth_repository.dart      # Interface
    │   └── data/
    │       ├── datasources/
    │       │   └── auth_remote_datasource.dart
    │       ├── models/
    │       │   └── auth_response_model.dart   # DTO
    │       └── repositories/
    │           └── auth_repository_impl.dart
    │
    ├── home/                          # Feature: Home/Dashboard
    │   ├── presentation/
    │   │   ├── pages/
    │   │   │   └── home_page.dart            # Dashboard UI
    │   │   ├── view_models/
    │   │   │   └── home_view_model.dart      # Dashboard logic
    │   │   └── widgets/
    │   │       ├── quick_action_card.dart
    │   │       └── activity_feed.dart
    │   ├── domain/
    │   │   ├── usecases/
    │   │   │   ├── get_dashboard_data_usecase.dart
    │   │   │   └── refresh_dashboard_usecase.dart
    │   │   └── repositories/
    │   │       └── dashboard_repository.dart
    │   └── data/
    │       └── repositories/
    │           └── dashboard_repository_impl.dart
    │
    ├── profile/                       # Feature: User Profile
    │   ├── presentation/
    │   │   ├── pages/
    │   │   │   └── profile_page.dart         # Profile screen
    │   │   ├── view_models/
    │   │   │   └── profile_view_model.dart
    │   │   └── widgets/
    │   │       ├── profile_header.dart
    │   │       └── settings_list.dart
    │   └── domain/
    │       └── usecases/
    │           ├── get_profile_usecase.dart
    │           └── update_profile_usecase.dart
    │
    ├── settings/                      # Feature: App Settings
    │   ├── presentation/
    │   │   ├── pages/
    │   │   │   └── settings_page.dart        # Settings screen
    │   │   ├── view_models/
    │   │   │   └── settings_view_model.dart
    │   │   └── widgets/
    │   │       ├── settings_tile.dart
    │   │       └── theme_selector.dart
    │   └── domain/
    │       └── usecases/
    │           ├── update_settings_usecase.dart
    │           └── clear_cache_usecase.dart
    │
    └── itinerary/                     # Feature: Trip Itinerary (Compass-style)
        ├── presentation/
        │   ├── pages/
        │   │   ├── itinerary_list_page.dart   # List of trips
        │   │   └── itinerary_detail_page.dart # Trip details
        │   ├── view_models/
        │   │   ├── itinerary_list_view_model.dart
        │   │   └── itinerary_detail_view_model.dart
        │   └── widgets/
        │       ├── trip_card.dart
        │       ├── destination_item.dart
        │       └── itinerary_timeline.dart
        ├── domain/
        │   ├── entities/
        │   │   ├── trip.dart
        │   │   └── destination.dart
        │   ├── usecases/
        │   │   ├── get_trips_usecase.dart
        │   │   ├── get_trip_detail_usecase.dart
        │   │   ├── create_trip_usecase.dart
        │   │   └── delete_trip_usecase.dart
        │   └── repositories/
        │       └── itinerary_repository.dart
        └── data/
            ├── datasources/
            │   ├── itinerary_remote_datasource.dart
            │   └── itinerary_local_datasource.dart
            ├── models/
            │   ├── trip_model.dart              # DTO
            │   └── destination_model.dart
            └── repositories/
                └── itinerary_repository_impl.dart
```

## Files to Update

### Critical VibeFlow Workflow Files

1. **[CLAUDE.md](CLAUDE.md)** - Update architecture documentation
2. **[`.claude/commands/vibeflow/feature.md`](.claude/commands/vibeflow/feature.md)** - Update feature generation
3. **[`.claude/commands/vibeflow/new.md`](.claude/commands/vibeflow/new.md)** - Update new app template
4. **[`.claude/commands/vibeflow/validate.md`](.claude/commands/vibeflow/validate.md)** - Update validation
5. **[`.claude/commands/vibeflow/status.md`](.claude/commands/vibeflow/status.md)** - Update progress tracking

### New Files to Create

1. **`lib/core/di/providers.dart`** - DI setup for app-wide Riverpod providers
2. **`lib/features/core/base_view_model.dart`** - Base ViewModel class
3. **`lib/core/routing/app_router.dart`** - Routing configuration
4. **`lib/core/network/api_service.dart`** - HTTP client setup

## Verification

After migration, verify:
1. Run `/vibeflow:new` - Should create spec files with updated structure
2. Run `/vibeflow:feature` - Should generate MVVM-compliant feature
3. Run `/vibeflow:validate` - Should validate new structure
4. Run `/vibeflow:status` - Should track progress with new paths
5. Check generated files follow Riverpod AsyncNotifier pattern
6. Verify dependency flow: UI → Domain → Data
