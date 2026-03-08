# VibeFlow Code Templates

This file contains all code templates used across VibeFlow. These are the single source of truth for code patterns.

---

## Main Entry Point Template

**File:** `lib/main.dart`

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';

import 'core/theme/app_theme.dart';
import 'presentation/providers/provider_registry.dart';
// import 'presentation/screens/[module]/[feature]_page.dart';

void main() async {
  WidgetsFlutterBinding.ensureInitialized();

  // TODO: Initialize data sources, services

  runApp(
    MultiProvider(
      providers: ProviderRegistry.getProviders(),
      child: const MyApp(),
    ),
  );
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: '[App Name]',
      debugShowCheckedModeBanner: false,
      theme: AppTheme.lightTheme,
      darkTheme: AppTheme.darkTheme,
      themeMode: ThemeMode.system,
      home: const [FirstFeature]Page(),
      // onGenerateRoute: AppRoutes.onGenerateRoute,
    );
  }
}
```

---

## Provider Registry Template

**File:** `lib/presentation/providers/provider_registry.dart`

```dart
import 'package:provider/provider.dart';
import 'package:flutter/material.dart';

// Import feature providers
// import '[feature]_provider.dart';

class ProviderRegistry {
  static List<SingleChildWidget> getProviders() {
    return [
      // Add feature providers here
      // ChangeNotifierProvider(
      //   create: (_) => [Feature]Provider(...),
      // ),

      // Core providers (settings, theme, etc.)
      // ChangeNotifierProvider(
      //   create: (_) => SettingsProvider(...),
      // ),
    ];
  }
}
```

**When Building Features:**
1. Add provider import at top of `provider_registry.dart`
2. Add provider to `getProviders()` list
3. No main.dart edits needed after initial setup

---

## Routing Template

**File:** `lib/presentation/routes/app_routes.dart`

```dart
import 'package:flutter/material.dart';
import '../screens/[module]/[feature]_screen.dart';
// ... other screen imports

class AppRoutes {
  static const String [FEATURE_ROUTE] = '/[module]/[feature]';
  // Add other routes

  static Map<String, WidgetBuilder> getRoutes() {
    return {
      [FEATURE_ROUTE]: (context) => const [Feature]Page(),
      // Add other routes
    };
  }

  static Route<T>? onGenerateRoute<T>(RouteSettings settings) {
    switch (settings.name) {
      case [FEATURE_ROUTE]:
        return MaterialPageRoute(builder: (context) => const [Feature]Page());
      // Add other cases
      default:
        return null;
    }
  }
}
```

**Navigation:**
```dart
Navigator.pushNamed(context, AppRoutes.[FEATURE_ROUTE]);
```

---

## Repository Interface Template

**File:** `lib/domain/repositories/[module]/[feature]_repository.dart`

```dart
import '../../entities/[entity].dart';

abstract class [Feature]Repository {
  Future<List<[Entity]>> get[Entity]List();
  Future<[Entity]> get[Entity]ById(String id);
  Future<[Entity]> create[Entity]([Entity] [entity]);
  Future<[Entity]> update[Entity]([Entity] [entity]);
  Future<void> delete[Entity](String id);
}
```

---

## Repository Implementation Template

**File:** `lib/data/repositories/[module]/[feature]_repository_impl.dart`

```dart
import '../../domain/entities/[entity].dart';
import '../../domain/repositories/[module]/[feature]_repository.dart';
import '../datasources/[module]/[feature]_datasource.dart';

class [Feature]RepositoryImpl implements [Feature]Repository {
  final [Feature]DataSource _dataSource;

  [Feature]RepositoryImpl(this._dataSource);

  @override
  Future<List<[Entity]>> get[Entity]List() async {
    return await _dataSource.get[Entity]List();
  }

  @override
  Future<[Entity]> get[Entity]ById(String id) async {
    return await _dataSource.get[Entity]ById(id);
  }

  @override
  Future<[Entity]> create[Entity]([Entity] [entity]) async {
    return await _dataSource.create[Entity]([entity]);
  }

  @override
  Future<[Entity]> update[Entity]([Entity] [entity]) async {
    return await _dataSource.update[Entity]([entity]);
  }

  @override
  Future<void> delete[Entity](String id) async {
    await _dataSource.delete[Entity](id);
  }
}
```

---

## Data Source Template

**File:** `lib/data/datasources/[module]/[feature]_datasource.dart`

```dart
import '../../domain/entities/[entity].dart';

abstract class [Feature]DataSource {
  Future<List<[Entity]>> get[Entity]List();
  Future<[Entity]> get[Entity]ById(String id);
  Future<[Entity]> create[Entity]([Entity] [entity]);
  Future<[Entity]> update[Entity]([Entity] [entity]);
  Future<void> delete[Entity](String id);
}
```

**Mock Implementation:**

**File:** `lib/data/datasources/[module]/[feature]_datasource_impl.dart`

```dart
import '../../domain/entities/[entity].dart';
import '[feature]_datasource.dart';

class [Feature]DataSourceImpl implements [Feature]DataSource {
  final List<[Entity]> _cache = [];

  @override
  Future<List<[Entity]>> get[Entity]List() async {
    return _cache;
  }

  @override
  Future<[Entity]> get[Entity]ById(String id) async {
    return _cache.firstWhere((e) => e.id == id);
  }

  @override
  Future<[Entity]> create[Entity]([Entity] [entity]) async {
    _cache.add([entity]);
    return [entity];
  }

  @override
  Future<[Entity]> update[Entity]([Entity] [entity]) async {
    final index = _cache.indexWhere((e) => e.id == [entity].id);
    _cache[index] = [entity];
    return [entity];
  }

  @override
  Future<void> delete[Entity](String id) async {
    _cache.removeWhere((e) => e.id == id);
  }
}
```

---

## Provider Template

**File:** `lib/presentation/providers/[feature]_provider.dart`

```dart
import 'package:flutter/foundation.dart';
import '../../domain/repositories/[module]/[feature]_repository.dart';
import '../../domain/entities/[entity].dart';

class [Feature]Provider extends ChangeNotifier {
  final [Feature]Repository _repository;

  [Feature]Provider(this._repository) {
    fetch[Entity]List();
  }

  // State
  List<[Entity]> _items = [];
  List<[Entity]> get items => _items;

  bool _isLoading = false;
  bool get isLoading => _isLoading;

  String? _error;
  String? get error => _error;
  bool get hasError => _error != null;

  // Operations
  Future<void> fetch[Entity]List() async {
    _setLoading(true);
    _clearError();

    try {
      _items = await _repository.get[Entity]List();
      _setLoading(false);
      notifyListeners();
    } catch (e) {
      _setError(e.toString());
      _setLoading(false);
      notifyListeners();
    }
  }

  Future<void> create[Entity]([Entity] item) async {
    _clearError();

    try {
      await _repository.create[Entity](item);
      await fetch[Entity]List();
    } catch (e) {
      _setError(e.toString());
      notifyListeners();
    }
  }

  Future<void> update[Entity]([Entity] item) async {
    _clearError();

    try {
      await _repository.update[Entity](item);
      await fetch[Entity]List();
    } catch (e) {
      _setError(e.toString());
      notifyListeners();
    }
  }

  Future<void> delete[Entity](String id) async {
    _clearError();

    try {
      await _repository.delete[Entity](id);
      await fetch[Entity]List();
    } catch (e) {
      _setError(e.toString());
      notifyListeners();
    }
  }

  // Private helpers
  void _setLoading(bool value) {
    _isLoading = value;
    notifyListeners();
  }

  void _setError(String error) {
    _error = error;
  }

  void _clearError() {
    _error = null;
  }
}
```

---

## Screen Templates

### Pure Screen Widget (Props-Driven)

**File:** `lib/presentation/screens/[module]/[feature]_screen.dart`

```dart
import 'package:flutter/material.dart';
import '../../widgets/[widget].dart';
import '../../domain/entities/[entity].dart';

class [Feature]Screen extends StatelessWidget {
  final List<[Entity]> items;
  final bool isLoading;
  final String? error;
  final VoidCallback onRefresh;
  final Function(String) onDelete;

  const [Feature]Screen({
    super.key,
    required this.items,
    required this.isLoading,
    required this.onRefresh,
    required this.onDelete,
    this.error,
  });

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('[Feature Name]'),
      ),
      body: _buildBody(context),
      floatingActionButton: FloatingActionButton(
        onPressed: () => _navigateToCreate(context),
        child: const Icon(Icons.add),
      ),
    );
  }

  Widget _buildBody(BuildContext context) {
    if (isLoading) {
      return const Center(child: CircularProgressIndicator());
    }

    if (error != null) {
      return Center(
        child: Text('Error: $error'),
      );
    }

    if (items.isEmpty) {
      return const Center(
        child: Text('No items found'),
      );
    }

    return ListView.builder(
      itemCount: items.length,
      itemBuilder: (context, index) {
        final item = items[index];
        return [Widget]Card(
          item: item,
          onDelete: () => onDelete(item.id),
        );
      },
    );
  }

  void _navigateToCreate(BuildContext context) {
    Navigator.pushNamed(context, '/[module]/[feature]/create');
  }
}
```

### Development View Wrapper (Sample Data)

**File:** `lib/presentation/screens/[module]/[feature]_view.dart`

```dart
import 'package:flutter/material.dart';
import '[feature]_screen.dart';
import '../../domain/entities/[entity].dart';

class [Feature]View extends StatelessWidget {
  const [Feature]View({super.key});

  static List<[Entity]> get _sampleData => [
    [Entity](id: '1', name: 'Sample 1'),
    [Entity](id: '2', name: 'Sample 2'),
  ];

  @override
  Widget build(BuildContext context) {
    return [Feature]Screen(
      items: _sampleData,
      isLoading: false,
      onRefresh: () => debugPrint('Refresh'),
      onDelete: (id) => debugPrint('Delete: $id'),
    );
  }
}
```

### Production Page Wrapper (Provider)

**File:** `lib/presentation/screens/[module]/[feature]_page.dart`

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import '[feature]_screen.dart';
import '../../providers/[feature]_provider.dart';

class [Feature]Page extends StatelessWidget {
  const [Feature]Page({super.key});

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider(
      create: (_) => [Feature]Provider(
        // Repository injected via Provider or DI
        context.read<[Feature]Repository>(),
      ),
      child: Consumer<[Feature]Provider>(
        builder: (context, provider, child) {
          return [Feature]Screen(
            items: provider.items,
            isLoading: provider.isLoading,
            error: provider.error,
            onRefresh: () => provider.fetch[Entity]List(),
            onDelete: (id) => provider.delete[Entity](id),
          );
        },
      ),
    );
  }
}
```

---

## Design Tokens Template

**File:** `lib/core/theme/design_tokens.dart` (GENERATED - DO NOT EDIT)

```dart
import 'package:flutter/material.dart';

/// Design tokens generated from specs/design_system/
///
/// Regenerate with: dart run build_runner build --delete-conflicting-outputs
abstract class DesignTokens {
  // Primary Colors
  static const Color primaryLight = Color(0xFF[PRIMARY_LIGHT]);
  static const Color primaryDark = Color(0xFF[PRIMARY_DARK]);
  static const Color onPrimaryLight = Color(0xFF[ON_PRIMARY_LIGHT]);
  static const Color onPrimaryDark = Color(0xFF[ON_PRIMARY_DARK]);

  // Semantic Colors
  static const Color successLight = Color(0xFF[SUCCESS_LIGHT]);
  static const Color successDark = Color(0xFF[SUCCESS_DARK]);
  static const Color warningLight = Color(0xFF[WARNING_LIGHT]);
  static const Color warningDark = Color(0xFF[WARNING_DARK]);
  static const Color errorLight = Color(0xFF[ERROR_LIGHT]);
  static const Color errorDark = Color(0xFF[ERROR_DARK]);
  static const Color infoLight = Color(0xFF[INFO_LIGHT]);
  static const Color infoDark = Color(0xFF[INFO_DARK]);

  // Custom Colors
  static const Color priorityHigh = Color(0xFF[PRIORITY_HIGH]);
  static const Color priorityMedium = Color(0xFF[PRIORITY_MEDIUM]);
  static const Color priorityLow = Color(0xFF[PRIORITY_LOW]);

  // Typography
  static const String primaryFont = '[PRIMARY_FONT]';
  static const String secondaryFont = '[SECONDARY_FONT]';
  static const String tertiaryFont = '[TERTIARY_FONT]';

  // Spacing
  static const double xs = 4.0;
  static const double sm = 8.0;
  static const double md = 16.0;
  static const double lg = 24.0;
  static const double xl = 32.0;
  static const double xxl = 48.0;

  // Border Radius
  static const double smRadius = 4.0;
  static const double mdRadius = 12.0;
  static const double lgRadius = 16.0;
  static const double fullRadius = 9999.0;
}
```

---

## App Theme Template

**File:** `lib/core/theme/app_theme.dart`

```dart
import 'package:flutter/material.dart';
import 'package:google_fonts/google_fonts.dart';
import 'design_tokens.dart';

class AppTheme {
  static ThemeData get lightTheme {
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(
        seedColor: DesignTokens.primaryLight,
        brightness: Brightness.light,
      ),
      textTheme: _buildTextTheme(Brightness.light),
      extensions: [
        _CustomColors.light,
      ],
    );
  }

  static ThemeData get darkTheme {
    return ThemeData(
      useMaterial3: true,
      colorScheme: ColorScheme.fromSeed(
        seedColor: DesignTokens.primaryDark,
        brightness: Brightness.dark,
      ),
      textTheme: _buildTextTheme(Brightness.dark),
      extensions: [
        _CustomColors.dark,
      ],
    );
  }

  static TextTheme _buildTextTheme(Brightness brightness) {
    final isLight = brightness == Brightness.light;
    final onColor = isLight
        ? const Color(0xFF[ON_BACKGROUND_LIGHT])
        : const Color(0xFF[ON_BACKGROUND_DARK]);

    return GoogleFonts.getFont(
      DesignTokens.primaryFont,
      textTheme: TextTheme(
        displayLarge: TextStyle(
          fontSize: 57,
          fontWeight: FontWeight.w400,
          letterSpacing: -0.25,
          height: 64 / 57,
          color: onColor,
        ),
        displayMedium: TextStyle(
          fontSize: 45,
          fontWeight: FontWeight.w400,
          letterSpacing: 0,
          height: 52 / 45,
          color: onColor,
        ),
        displaySmall: TextStyle(
          fontSize: 36,
          fontWeight: FontWeight.w400,
          letterSpacing: 0,
          height: 44 / 36,
          color: onColor,
        ),
        headlineLarge: TextStyle(
          fontSize: 32,
          fontWeight: FontWeight.w600,
          letterSpacing: 0,
          height: 40 / 32,
          color: onColor,
        ),
        headlineMedium: TextStyle(
          fontSize: 28,
          fontWeight: FontWeight.w600,
          letterSpacing: 0,
          height: 36 / 28,
          color: onColor,
        ),
        headlineSmall: TextStyle(
          fontSize: 24,
          fontWeight: FontWeight.w600,
          letterSpacing: 0,
          height: 32 / 24,
          color: onColor,
        ),
        titleLarge: TextStyle(
          fontSize: 22,
          fontWeight: FontWeight.w500,
          letterSpacing: 0,
          height: 28 / 22,
          color: onColor,
        ),
        titleMedium: TextStyle(
          fontSize: 16,
          fontWeight: FontWeight.w500,
          letterSpacing: 0.15,
          height: 24 / 16,
          color: onColor,
        ),
        titleSmall: TextStyle(
          fontSize: 14,
          fontWeight: FontWeight.w500,
          letterSpacing: 0.1,
          height: 20 / 14,
          color: onColor,
        ),
        bodyLarge: TextStyle(
          fontSize: 16,
          fontWeight: FontWeight.w400,
          letterSpacing: 0.5,
          height: 24 / 16,
          color: onColor,
        ),
        bodyMedium: TextStyle(
          fontSize: 14,
          fontWeight: FontWeight.w400,
          letterSpacing: 0.25,
          height: 20 / 14,
          color: onColor,
        ),
        bodySmall: TextStyle(
          fontSize: 12,
          fontWeight: FontWeight.w400,
          letterSpacing: 0.4,
          height: 16 / 12,
          color: onColor,
        ),
        labelLarge: TextStyle(
          fontSize: 14,
          fontWeight: FontWeight.w500,
          letterSpacing: 0.1,
          height: 20 / 14,
          color: onColor,
        ),
        labelMedium: TextStyle(
          fontSize: 12,
          fontWeight: FontWeight.w500,
          letterSpacing: 0.5,
          height: 16 / 12,
          color: onColor,
        ),
        labelSmall: TextStyle(
          fontSize: 11,
          fontWeight: FontWeight.w500,
          letterSpacing: 0.5,
          height: 16 / 11,
          color: onColor,
        ),
      ),
    );
  }
}

@immutable
class _CustomColors extends ThemeExtension<_CustomColors> {
  const _CustomColors({
    required this.success,
    required this.warning,
    required this.error,
    required this.info,
    required this.priorityHigh,
    required this.priorityMedium,
    required this.priorityLow,
  });

  final Color success;
  final Color warning;
  final Color error;
  final Color info;
  final Color priorityHigh;
  final Color priorityMedium;
  final Color priorityLow;

  static const light = _CustomColors(
    success: DesignTokens.successLight,
    warning: DesignTokens.warningLight,
    error: DesignTokens.errorLight,
    info: DesignTokens.infoLight,
    priorityHigh: DesignTokens.priorityHigh,
    priorityMedium: DesignTokens.priorityMedium,
    priorityLow: DesignTokens.priorityLow,
  );

  static const dark = _CustomColors(
    success: DesignTokens.successDark,
    warning: DesignTokens.warningDark,
    error: DesignTokens.errorDark,
    info: DesignTokens.infoDark,
    priorityHigh: DesignTokens.priorityHigh,
    priorityMedium: DesignTokens.priorityMedium,
    priorityLow: DesignTokens.priorityLow,
  );

  @override
  _CustomColors copyWith({
    Color? success,
    Color? warning,
    Color? error,
    Color? info,
    Color? priorityHigh,
    Color? priorityMedium,
    Color? priorityLow,
  }) {
    return _CustomColors(
      success: success ?? this.success,
      warning: warning ?? this.warning,
      error: error ?? this.error,
      info: info ?? this.info,
      priorityHigh: priorityHigh ?? this.priorityHigh,
      priorityMedium: priorityMedium ?? this.priorityMedium,
      priorityLow: priorityLow ?? this.priorityLow,
    );
  }

  @override
  _CustomColors lerp(covariant ThemeExtension<_CustomColors>? other, double t) {
    if (other is! _CustomColors) {
      return this;
    }
    return _CustomColors(
      success: Color.lerp(success, other.success, t)!,
      warning: Color.lerp(warning, other.warning, t)!,
      error: Color.lerp(error, other.error, t)!,
      info: Color.lerp(info, other.info, t)!,
      priorityHigh: Color.lerp(priorityHigh, other.priorityHigh, t)!,
      priorityMedium: Color.lerp(priorityMedium, other.priorityMedium, t)!,
      priorityLow: Color.lerp(priorityLow, other.priorityLow, t)!,
    );
  }
}
```

---

## File Structure Reference

```
lib/
├── domain/
│   ├── entities/
│   │   └── [entity].dart
│   └── repositories/
│       └── [module]/
│            └── [feature]_repository.dart
├── data/
│   ├── repositories/
│   │   └── [module]/
│   │        └── [feature]_repository_impl.dart
│   └── datasources/
│       └── [module]/
│            └── [feature]_datasource.dart
├── presentation/
│   ├── screens/
│   │   └── [module]/
│   │        ├── [feature]_screen.dart
│   │        ├── [feature]_view.dart
│   │        └── [feature]_page.dart
│   ├── widgets/
│   │   └── [widget].dart
│   ├── providers/
│   │   └── [feature]_provider.dart
│   └── routes/
│       └── app_routes.dart
└── core/
    ├── theme/
    │   └── app_theme.dart
    └── constants/
        └── [constants].dart
```

**Key Principles:**
- Domain entities in flat `entities/` folder
- Repository interfaces grouped by module
- Repository implementations grouped by module
- Data sources grouped by module
- Screens grouped by module
- Shared widgets flat in `widgets/`
- Providers flat in `providers/`
