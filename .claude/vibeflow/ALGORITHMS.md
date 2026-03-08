# VibeFlow Algorithms & Formulas

This file contains all algorithms, calculations, and reference data used across VibeFlow commands.

---

## Progress Calculation

### Formula

```
progress = (spec_complete * 30) +
           (sample_data * 30) +
           (types_documented * 12) +
           (entities_defined * 24) +
           (screens_implemented * 12)
```

### Status Determination

| Progress Range | Status |
|----------------|--------|
| 0% | `pending` |
| 1-49% | `started` |
| 50-99% | `in_progress` |
| 100% | `done` |

### File Checks

**Spec Status (60% total):**
- `/specs/modules/[module]/[feature_slug]/spec.md` exists → +30%
- `/specs/modules/[module]/[feature_slug]/data.json` exists → +30%
- `/specs/modules/[module]/[feature_slug]/models.md` exists → +12%

**Implementation Status (40% total):**
- `/lib/domain/entities/` has entity .dart files → +24%
- `/lib/presentation/screens/` has screen .dart files → +12%

---

## Feature Slug Conversion

Convert feature names to slugs using these rules:
1. Lowercase everything
2. Replace spaces with underscore
3. Remove special characters
4. Keep words intact

**Examples:**
- "Dashboard" → `dashboard`
- "User Management" → `user_management`
- "Add Transaction Flow" → `add_transaction_flow`
- "Due Dates & Reminders" → `due_dates_reminders`

**Feature path construction:**
- **Spec path:** `[module]/[feature_slug]` → `specs/modules/tasks/add_task/`
- **Implementation:** Distributed across layers (no feature folders)

---

## Module Inference

Modules are auto-inferred from feature names using these patterns:

| Feature Name Pattern | Inferred Module |
|---------------------|-----------------|
| "Task List", "Add Task", "Complete Task" | `tasks` |
| "User Profile", "User Settings" | `user` |
| "Dashboard", "Home Screen" | `dashboard` |
| "Authentication", "Login", "Register" | `auth` |
| "Settings", "Preferences" | `settings` |
| "Notifications", "Alerts" | `notifications` |

**Module Slug Conversion:**
1. Lowercase everything
2. Remove plural 's' if present
3. Replace spaces with underscore
4. Keep words intact

**Examples:**
- "Task Management" → `task_management`
- "User Profile" → `user`
- "App Settings" → `settings`
- "Notifications" → `notification`

---

## Feature Inference Database

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

## Entity Deduplication & Merge Algorithm

When generating entities in `lib/domain/entities/`:

1. **Check if entity exists:**
   ```
   lib/domain/entities/[entity_name].dart
   ```

2. **If entity doesn't exist:**
   - Create new entity file with all properties
   - Proceed to next entity

3. **If entity exists:**
   - Read the existing entity file
   - Parse current properties (name, type, nullable)
   - Compare with new properties from spec

4. **Merge Decision:**
   - **If properties match exactly:** Use existing entity, skip generation
   - **If new properties added:** Append new properties to existing entity
   - **If properties conflict:** Ask user using AskUserQuestion:
     ```
     "Entity [EntityName] already exists with different properties:
     Existing: [list existing]
     New: [list new]
     What would you like to do?"
     Options:
     1. "Keep existing" — Use existing entity as-is
     2. "Merge properties" — Add new properties to existing
     3. "Replace with new" — Overwrite existing entity
     ```

5. **After merge:**
   - Update the entity file with merged properties
   - Add comment if properties were merged: `// Merged on [date]`

**Example Merge:**
```dart
// Existing: task.dart
class Task {
  final String id;
  final String title;
  final bool isCompleted;
}

// After merge (adding description, createdAt):
class Task {
  final String id;
  final String title;
  final bool isCompleted;
  final String? description;      // NEW
  final DateTime createdAt;        // NEW
}
```

---

## Dependency Resolution (Topological Sort)

Used in `/vibeflow:plan` to resolve feature dependencies:

```python
def resolve_dependencies(features):
    """
    features: dict of {id: {data: {...}, dependencies: [ids]}}
    Returns: ordered list of feature IDs
    """
    visited = set()
    temp_visited = set()
    result = []

    def visit(feature_id):
        if feature_id in temp_visited:
            raise ValueError(f"Circular dependency detected: {feature_id}")
        if feature_id in visited:
            return

        temp_visited.add(feature_id)

        for dep_id in features[feature_id].get('dependencies', []):
            if dep_id in features:
                visit(dep_id)

        temp_visited.remove(feature_id)
        visited.add(feature_id)
        result.append(feature_id)

    for feature_id in features:
        if feature_id not in visited:
            visit(feature_id)

    return result
```

**Error Handling:**
- If circular dependency detected: alert user and break cycle
- If dependency ID not found: warn user but continue

---

## Priority Ordering

Features are ordered by priority after dependency resolution:

1. **P0** — Critical features (build first)
2. **P1** — Important features (build second)
3. **P2** — Nice to have (build last)

Within each priority level, maintain dependency order.

---

## Common Errors & Solutions

| Error | Cause | Solution |
|-------|-------|----------|
| Feature spec not found | `/specs/roadmap.md` missing | Run `/vibeflow:new` first |
| Design system exists | User already has colors/typography | Ask to replace or keep |
| Circular dependencies | Feature A depends on B, B depends on A | Break cycle, remove one dependency |
| Invalid dependency | Feature ID doesn't exist | Check roadmap for correct IDs |
| All features done | No pending features | All features marked as `done` |
| Dependencies Required | Feature depends on incomplete features | Build dependencies first |
