# New Flutter App

You are helping the user create a new Flutter app by describing their idea. You'll generate the complete product specification from their description.

## Step 1: Check Current State

Check if any spec files exist in `specs/` directory.

**If specs already exist:**

"I see you already have some project files set up.
Existing files found: [list of files]

**Options:**
1. Start fresh (overwrite existing specs)
2. Keep existing and add new features
3. Cancel and manage existing files manually"

Wait for user choice. If they choose 1 or 2, proceed. If 3, stop.

**If directory is clean:**
Proceed to Step 2.

---

## Step 2: Gather App Information

Ask the user for essential information:

**Quick Questions:**

1. "What's your app called?"
   - Wait for app name
   - **Validation:** App name must:
     - Be 2-30 characters long
     - Start with a letter
     - Contain only letters, numbers, spaces, and hyphens
     - Not use reserved words (flutter, dart, test, main)
     - Not start with a number or special character
   - If validation fails, ask again with guidance

2. "Describe your app in one sentence. What does it do?"
   - Wait for description

3. "What's the main problem it solves?"
   - Wait for problem description

4. "Who are your target users?"
   - Wait for user description

5. "What are the 3-5 key features?"
   - Wait for feature list (or infer from description)

That's it! No more questions needed.

---

## Step 3: Generate Product Specs

**Immediately create** all spec files with smart defaults.

### 3.1: Product Overview

Create `specs/overview.md` with:

```markdown
# Product Overview

## App Name
[App Name]

## Description
[User's description]

## Problems
- [Main problem from user input]

## Solutions
- [How the app solves the problem]

## Target Users
[User's description]

## Key Features
[Infer from description or use user's list]

1. [Feature 1]
2. [Feature 2]
3. [Feature 3]
```

---

### 3.2: Roadmap

**Immediately create** `specs/roadmap.md` with simple format:

**Infer features from:**
- User's feature list (if provided)
- Key problems mentioned
- App description (extract verbs/nouns)
- Mobile app patterns (auth, dashboard, lists, forms, settings)

**Generate 3-6 features** using the simple format:

```markdown
# Product Roadmap

## Features

### 1. [Core Feature 1]
- **ID:** F001
- **Priority:** P0
- **Effort:** medium
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core

[Description...]

### 2. [Core Feature 2]
- **ID:** F002
- **Priority:** P0
- **Effort:** medium
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core

[Description...]

### 3. [Supporting Feature]
- **ID:** F003
- **Priority:** P1
- **Effort:** small
- **Status:** pending
- **Dependencies:** F001
- **Phase:** phase-1
- **Tags:** ui

[Description...]
```

**Feature Selection Logic:**
- Always include: core value loop feature
- Always include: data entry/management feature
- Always include: view/list feature
- Optional: auth (if user mentions accounts, login, profiles)
- Optional: settings (include by default for completeness)
- Optional: search/filter (if data-heavy)

**Priority Assignment:**
- Core value features → P0
- Supporting features → P1
- Nice-to-have → P2
- Nice-to-have → P2

---

### 3.3: Data Shape

**Immediately create** `specs/data_shape.md`:

**Infer entities from:**
- Feature descriptions in roadmap
- App description (nouns = potential entities)
- Common patterns (User, Item, Transaction, etc.)

```markdown
# Data Shape

## Entities

### [Entity 1]
[Description based on its role in the app]

### [Entity 2]
...

## Relationships

- [Entity 1] has many [Entity 2]
- [Entity 2] belongs to [Entity 1]
...
```

**Entity Inference Rules:**
- Main "noun" in app name → primary entity
- Features mentioning "create", "manage", "track" → entities
- Dashboard/reports → analytics entity
- Categories/tags → classification entity

---

### 3.4: Feature Specs

**Automatically create** `specs/features/[feature_slug]/spec.md` for each roadmap item:

```markdown
# [Feature Name] Spec

## Purpose
[Inferred from roadmap description]

## User Flows
1. **[Flow 1]** — [Typical user journey]
2. **[Flow 2]** — [Alternative path]

## Screens
- **[Screen 1]** — [Purpose]
- **[Screen 2]** — [Purpose]

## States
- **Empty** — [When no data exists]
- **Loading** — [During data fetch]
- **Error** — [On failure]
- **Success** — [Normal operation]

## Interactions
- **[Action]** — [Response]

## Data Requirements
- [Entity 1] (read, create)
- [Entity 2] (read-only)
```

**User Flow Inference:**
- Create screens → "New", "Edit", "Delete" flows
- List screens → "View", "Search", "Filter" flows
- Dashboard → "Summary", "Drill-down" flows
- Detail screens → "View details", "Related items" flows

---

### 3.5: Core Infrastructure

**Immediately create** core infrastructure files for the app:

Create the following files in `lib/`:

**DI Setup:**
- `lib/core/di/providers.dart` - App-wide Riverpod providers

**Base Classes:**
- `lib/features/core/base_view_model.dart` - Base AsyncNotifier class

**Routing:**
- `lib/core/routing/app_router.dart` - go_router configuration

**Network:**
- `lib/core/network/api_service.dart` - Dio HTTP client setup

**Theme:**
- `lib/core/theme/app_theme.dart` - Material 3 theme configuration

**Shared Widgets:**
- `lib/core/widgets/app_scaffold.dart` - Shared scaffold wrapper
- `lib/core/widgets/error_widget.dart` - Error display widget
- `lib/core/widgets/loading_widget.dart` - Loading indicator
- `lib/core/widgets/empty_state_widget.dart` - Empty state display

**Utilities:**
- `lib/core/utils/constants.dart` - App constants
- `lib/core/utils/extensions.dart` - Dart extensions
- `lib/core/utils/validators.dart` - Input validators

**Content for each file:**

`lib/core/di/providers.dart`:
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'package:riverpod_annotation/riverpod_annotation.dart';

part 'providers.g.dart';

/// App-wide Riverpod providers for dependency injection.
class AppProviders {
  // Add your app-level providers here
}
```

`lib/features/core/base_view_model.dart`:
```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';

/// Base ViewModel class providing common functionality for all ViewModels.
abstract class BaseViewModel<T> extends AsyncNotifier<T> {
  bool get isLoading => state.isLoading;
  bool get hasError => state.hasError;
  Object? get error => state.error;

  @protected
  Future<void> executeOperation(Future<T> Function() operation) async {
    state = const AsyncValue.loading();
    state = await AsyncValue.guard(operation);
  }

  Future<void> refresh() async {
    // Subclasses override for refresh logic
  }
}
```

`lib/core/routing/app_router.dart`:
```dart
import 'package:flutter/material.dart';
import 'package:go_router/go_router.dart';

class AppRouter {
  static final goRouter = GoRouter(
    debugLogDiagnostics: true,
    routes: [
      // Add routes here
    ],
    errorBuilder: (context, state) => Scaffold(
      appBar: AppBar(title: const Text('Error')),
      body: Center(
        child: Text('Page not found: ${state.uri}'),
      ),
    ),
  );
}
```

`lib/core/network/api_service.dart`:
```dart
import 'package:dio/dio.dart';

class ApiService {
  late final Dio _dio;

  ApiService({
    String baseUrl = 'https://api.example.com',
    Duration connectTimeout = const Duration(seconds: 30),
    Duration receiveTimeout = const Duration(seconds: 30),
  }) {
    _dio = Dio(BaseOptions(
      baseUrl: baseUrl,
      connectTimeout: connectTimeout,
      receiveTimeout: receiveTimeout,
      headers: {'Content-Type': 'application/json', 'Accept': 'application/json'},
    ));
  }

  Future<Response> get(String path, {Map<String, dynamic>? queryParameters}) =>
      _dio.get(path, queryParameters: queryParameters);

  Future<Response> post(String path, {dynamic data}) => _dio.post(path, data: data);

  Future<Response> put(String path, {dynamic data}) => _dio.put(path, data: data);

  Future<Response> delete(String path) => _dio.delete(path);

  Dio get dio => _dio;
}
```

`lib/core/theme/app_theme.dart`:
```dart
import 'package:flutter/material.dart';
import 'package:google_fonts/google_fonts.dart';

class AppTheme {
  static ThemeData get lightTheme => ThemeData(
        useMaterial3: true,
        brightness: Brightness.light,
        colorScheme: ColorScheme.fromSeed(seedColor: const Color(0xFF3B82F6)),
        textTheme: GoogleFonts.interTextTheme(),
      );

  static ThemeData get darkTheme => ThemeData(
        useMaterial3: true,
        brightness: Brightness.dark,
        colorScheme: ColorScheme.fromSeed(
          seedColor: const Color(0xFF3B82F6),
          brightness: Brightness.dark,
        ),
        textTheme: GoogleFonts.interTextTheme(ThemeData.dark().textTheme),
      );
}
```

`lib/core/widgets/app_scaffold.dart`:
```dart
import 'package:flutter/material.dart';

class AppScaffold extends StatelessWidget {
  const AppScaffold({
    super.key,
    this.title,
    required this.body,
    this.actions,
    this.floatingActionButton,
  });

  final String? title;
  final Widget body;
  final List<Widget>? actions;
  final Widget? floatingActionButton;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: title != null
          ? AppBar(title: Text(title!), actions: actions)
          : null,
      body: SafeArea(child: body),
      floatingActionButton: floatingActionButton,
    );
  }
}
```

`lib/core/widgets/error_widget.dart`:
```dart
import 'package:flutter/material.dart';

class AppErrorWidget extends StatelessWidget {
  const AppErrorWidget({
    super.key,
    required this.error,
    this.onRetry,
  });

  final String error;
  final VoidCallback? onRetry;

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          const Icon(Icons.error_outline, size: 64),
          const SizedBox(height: 16),
          Text('Something went wrong'),
          const SizedBox(height: 8),
          Text(error),
          if (onRetry != null) ...[
            const SizedBox(height: 24),
            ElevatedButton(
              onPressed: onRetry,
              child: const Text('Retry'),
            ),
          ],
        ],
      ),
    );
  }
}
```

`lib/core/widgets/loading_widget.dart`:
```dart
import 'package:flutter/material.dart';

class AppLoadingWidget extends StatelessWidget {
  const AppLoadingWidget({super.key, this.message});

  final String? message;

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          const CircularProgressIndicator(),
          if (message != null) ...[
            const SizedBox(height: 16),
            Text(message!),
          ],
        ],
      ),
    );
  }
}
```

`lib/core/widgets/empty_state_widget.dart`:
```dart
import 'package:flutter/material.dart';

class AppEmptyStateWidget extends StatelessWidget {
  const AppEmptyStateWidget({
    super.key,
    required this.message,
    this.actionLabel,
    this.onAction,
    this.icon = Icons.inbox,
  });

  final String message;
  final String? actionLabel;
  final VoidCallback? onAction;
  final IconData icon;

  @override
  Widget build(BuildContext context) {
    return Center(
      child: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Icon(icon, size: 64),
          const SizedBox(height: 16),
          Text(message),
          if (actionLabel != null && onAction != null) ...[
            const SizedBox(height: 24),
            ElevatedButton(
              onPressed: onAction,
              child: Text(actionLabel!),
            ),
          ],
        ],
      ),
    );
  }
}
```

`lib/core/utils/constants.dart`:
```dart
class AppConstants {
  static const String appName = 'App Name';
  static const String apiBaseUrl = 'https://api.example.com';
  static const Duration apiTimeout = Duration(seconds: 30);
  static const int defaultPageSize = 20;
}
```

`lib/core/utils/extensions.dart`:
```dart
import 'package:flutter/material.dart';

extension StringExtensions on String {
  String capitalize() {
    if (isEmpty) return this;
    return '${this[0].toUpperCase()}${substring(1)}';
  }

  bool get isBlank => trim().isEmpty;
}

extension BuildContextExtensions on BuildContext {
  ColorScheme get colorScheme => Theme.of(this).colorScheme;
  TextTheme get textTheme => Theme.of(this).textTheme;
  Size get screenSize => MediaQuery.of(this).size;
  double get screenWidth => screenSize.width;
  double get screenHeight => screenSize.height;

  void showSnackBar(String message) {
    ScaffoldMessenger.of(this).showSnackBar(
      SnackBar(content: Text(message)),
    );
  }
}
```

`lib/core/utils/validators.dart`:
```dart
String? validateEmail(String? value) {
  if (value == null || value.isEmpty) return 'Email is required';
  if (!RegExp(r'^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$').hasMatch(value)) {
    return 'Please enter a valid email';
  }
  return null;
}

String? validatePassword(String? value) {
  if (value == null || value.isEmpty) return 'Password is required';
  if (value.length < 8) return 'Password must be at least 8 characters';
  return null;
}

String? validateRequired(String? value, [String fieldName = 'This field']) {
  if (value == null || value.trim().isEmpty) return '$fieldName is required';
  return null;
}
```

---

## Step 4: Design System Research & Generation

"Your app structure is ready! Let's set up your design system.

I'll analyze your app type and target users to research and recommend theme options."

Use `/vibeflow:theme` command to research and generate your design system.

Would you like to:
1. "Generate design system now" — Run `/vibeflow:theme`
2. "Skip for now" — You can run `/vibeflow:theme` later

**If 1:** Run the theme command workflow
**If 2:** Continue to final summary

---

## Step 5: Final Summary

Present a comprehensive summary:

"✅ Your [App Name] app is ready!

**📱 App:** [App Name]
**🎨 Design:** [Theme selected / Not set]
**📋 Features:** [N] features defined
**📁 Structure:** Feature-first architecture ready

**Generated Files:**

**Specification:**
- `specs/overview.md` — Product vision
- `specs/roadmap.md` — Feature roadmap with metadata
- `specs/data_shape.md` — Data entities
- `specs/features/*/spec.md` — [N] feature specifications
- `specs/design_system/*` — Design tokens (if theme selected)

**Core Infrastructure:**
- `lib/core/di/providers.dart` — DI setup
- `lib/features/core/base_view_model.dart` — Base ViewModel
- `lib/core/routing/app_router.dart` — Routing
- `lib/core/network/api_service.dart` — HTTP client
- `lib/core/theme/app_theme.dart` — Theme configuration
- `lib/core/widgets/*` — Shared widgets
- `lib/core/utils/*` — Utilities

**Next Steps:**

1. **Set up your design system:**
   ```bash
   /vibeflow:theme
   ```

2. **Plan your sprint:**
   ```bash
   /vibeflow:sprint
   ```

3. **Build features:**
   ```bash
   /vibeflow:feature
   ```

4. **Track progress:**
   ```bash
   /vibeflow:status
   ```

**Quick Reference:**
- Set up design: `/vibeflow:theme`
- Plan sprint: `/vibeflow:sprint`
- Build feature: `/vibeflow:feature`
- View progress: `/vibeflow:status`

You're all set! Start building your first feature."

---

## Supporting Sections

### Feature Inference Database

Use these patterns to infer features from app descriptions:

| Keywords in Description | Suggested Features |
|------------------------|-------------------|
| "track", "log", "record" | Dashboard, Add Entry, List View, Reports |
| "social", "community", "connect" | Feed, Post, Profile, Comments, Notifications |
| "shopping", "store", "buy" | Catalog, Cart, Checkout, Orders, Profile |
| "tasks", "todo", "manage" | Task List, Add Task, Categories, Reminders |
| "notes", "write", "document" | Note List, Editor, Folders, Search |
| "finance", "money", "budget" | Dashboard, Add Transaction, Categories, Reports, History |
| "fitness", "health", "workout" | Dashboard, Log Workout, Exercises, Progress, Goals |
| "learn", "education", "course" | Course List, Lesson View, Progress, Quizzes |
| "chat", "message", "conversation" | Chat List, Conversation View, Message Input, Media Sharing, Notifications |
| "travel", "booking", "reservation" | Search, Booking Flow, Trip Details, Confirmation, Itinerary |
| "food", "delivery", "restaurant" | Restaurant List, Menu View, Cart, Order Tracking, Reviews |
| "property", "real estate", "rent" | Property Search, Listing Details, Favorites, Contact Agent, Map View |
| "event", "ticket", "concert" | Event Discovery, Event Details, Ticket Purchase, Calendar Integration, QR Tickets |
| "weather", "forecast", "temperature" | Current Weather, Forecast View, Location Search, Alerts, Settings |
| "music", "audio", "playlist" | Library, Player Controls, Playlist Management, Search, Downloads |
| "map", "navigation", "direction" | Map View, Route Planning, Location Search, Turn-by-Turn, Saved Places |
| "sell", "marketplace", "listing" | Product Feed, Seller Dashboard, Messaging, Reviews, Wallet |
| "doctor", "medical", "appointment" | Appointment Booking, Doctor Search, Medical Records, Prescriptions, Video Consult |
| "game", "quiz", "trivia" | Game Lobby, Gameplay, Score Tracking, Leaderboards, Achievements |
| "news", "article", "content" | Article Feed, Categories, Bookmarks, Sharing, Notifications |
| "portfolio", "showcase", "gallery" | Project Gallery, Project Details, About Section, Contact, Categories |
| "survey", "form", "questionnaire" | Form Builder, Survey List, Response Collection, Analytics, Export |

---

## Important Notes

- **AskUserQuestion Tool:** ALWAYS use AskUserQuestion when asking the user questions — never ask through text
- **Immediate file writing:** Never show drafts, always write directly
- **Spec structure:** Always use exact spec structure for consistency (see CLAUDE.md)
- **Inference over questioning:** For custom apps, extract info from user input, don't ask everything
- **App name required:** Always ask for app name to personalize
- **Theme system:** Use `/vibeflow:theme` for research-based theme recommendations
- **Progress tracking:** Mention `/vibeflow:status` in summary
- **First feature:** Always suggest which feature to start with
- **Reference:** All specification formats are in CLAUDE.md
