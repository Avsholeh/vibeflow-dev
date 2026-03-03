# VibeFlow Example: Todo App

This example demonstrates the complete output of the VibeFlow workflow for a Todo app.

## Generated Files Structure

```
specs/
├── overview.md
├── roadmap.md
├── data-shape.md
└── features/
    ├── task-list/
    │   ├── spec.md
    │   ├── data.json
    │   └── models.md
    ├── add-edit-task/
    │   ├── spec.md
    │   ├── data.json
    │   └── models.md
    ├── categories/
    │   ├── spec.md
    │   ├── data.json
    │   └── models.md
    ├── due-dates-reminders/
    │   ├── spec.md
    │   ├── data.json
    │   └── models.md
    └── progress-dashboard/
        ├── spec.md
        ├── data.json
        └── models.md
```

---

## 1. Product Overview (`specs/overview.md`)

```markdown
# Product Overview

## App Name
TodoApp

## Description
A clean, focused task manager for organizing daily tasks and staying productive.

## Problems
- Tasks get lost across multiple apps
- No clear priorities or due dates
- Hard to track progress

## Solutions
- Simple task capture and organization
- Priority levels and due dates
- Progress tracking and completion

## Target Users
- Professionals managing tasks
- Students organizing assignments
- Anyone who needs task tracking

## Key Features
1. Task list with filters
2. Quick task creation
3. Task categories/tags
4. Due dates and reminders
5. Progress tracking
```

---

## 2. Roadmap (`specs/roadmap.md`)

```markdown
---
version: "1.0"
appName: "TodoApp"
lastUpdated: "2025-03-01"
phases:
  - id: "phase-1"
    name: "MVP Foundation"
    description: "Core features for initial release"
defaultPriority: "P1"
defaultEffort: "medium"
---

# Product Roadmap

## Features

### 1. Task List
- **ID:** F001
- **Priority:** P0
- **Effort:** medium
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core, ui

Main list with filters and search. Displays all tasks with their status, priority, and due dates.

### 2. Add/Edit Task
- **ID:** F002
- **Priority:** P0
- **Effort:** medium
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** core, ui

Quick task creation with details. Allows setting title, description, priority, due date, and category.

### 3. Categories
- **ID:** F003
- **Priority:** P1
- **Effort:** small
- **Status:** pending
- **Dependencies:** none
- **Phase:** phase-1
- **Tags:** ui, data

Organize tasks by category. Create, edit, and delete task categories with colors and icons.

### 4. Due Dates & Reminders
- **ID:** F004
- **Priority:** P1
- **Effort:** medium
- **Status:** pending
- **Dependencies:** F002
- **Phase:** phase-1
- **Tags:** ui, data

Set due dates and get notified. Configure notifications for upcoming and overdue tasks.

### 5. Progress Dashboard
- **ID:** F005
- **Priority:** P1
- **Effort:** large
- **Status:** pending
- **Dependencies:** F001, F002
- **Phase:** phase-1
- **Tags:** ui, analytics

Visual progress, completion stats. Charts showing completion rates, task distribution by category, and trends.

---

## 3. Data Shape (`specs/data-shape.md`)

```markdown
# Data Shape

## Entities

### Task
A task represents a single to-do item that the user wants to complete.

**Properties:**
- `title` (string) - The task name
- `description` (string, optional) - Detailed task description
- `completed` (boolean) - Whether the task is done
- `dueDate` (datetime, optional) - When the task is due
- `priority` (enum: low, medium, high, urgent) - Task importance
- `category` (reference to Category) - Task categorization
- `createdAt` (datetime) - When the task was created
- `updatedAt` (datetime) - When the task was last modified

### Category
Categories help organize tasks into groups.

**Properties:**
- `name` (string) - Category name
- `color` (string) - Display color (hex)
- `icon` (string) - Icon identifier

### Reminder
Reminders are notifications for tasks with due dates.

**Properties:**
- `task` (reference to Task) - The task to remind about
- `dateTime` (datetime) - When to show the reminder
- `repeat` (enum: none, daily, weekly, monthly) - Repeat pattern

## Relationships

- Task belongs to Category (many-to-one)
- Category has many Tasks
- Task has many Reminders
- Reminder belongs to Task
```

---

## 4. Feature Spec Example (`specs/features/task-list/spec.md`)

```markdown
# Task List Spec

## Metadata
- **Feature ID:** F001
- **Priority:** P0
- **Effort:** medium
- **Phase:** MVP Foundation
- **Dependencies:** []

## Purpose
Display all tasks in a filterable, searchable list. This is the main screen where users view and manage their tasks.

## User Flows
1. **View all tasks** — User opens app, sees list of all tasks
2. **Filter by status** — User taps "Active" or "Completed" filter
3. **Search tasks** — User types in search box to find tasks
4. **Complete task** — User taps checkbox to mark task done
5. **Tap task** — User navigates to task detail screen

## Screens
- **Task List Screen** — Main screen with task list
- **Task Detail Screen** — View full task details

## States
- **Empty** — No tasks, show empty state illustration
- **Loading** — Shimmer skeleton during initial load
- **Error** — Error message on fetch failure
- **Success** — Tasks displayed in list

## Interactions
- **Tap checkbox** — Toggle task completed status with animation
- **Tap task** — Navigate to task detail screen
- **Pull to refresh** — Reload task list
- **Swipe left** — Quick delete action

## Data Requirements
- Task (read, update)
- Category (read-only for display)
```

---

## 5. Sample Data Example (`specs/features/task-list/data.json`)

```json
{
  "tasks": [
    {
      "id": "T001",
      "title": "Complete project proposal",
      "description": "Write and submit the Q2 project proposal document",
      "completed": false,
      "dueDate": "2025-03-15T17:00:00Z",
      "priority": "high",
      "category": "Work",
      "createdAt": "2025-03-01T09:00:00Z",
      "updatedAt": "2025-03-01T09:00:00Z"
    },
    {
      "id": "T002",
      "title": "Buy groceries",
      "description": "Milk, eggs, bread, vegetables",
      "completed": false,
      "dueDate": "2025-03-02T18:00:00Z",
      "priority": "medium",
      "category": "Personal",
      "createdAt": "2025-03-01T10:30:00Z",
      "updatedAt": "2025-03-01T10:30:00Z"
    },
    {
      "id": "T003",
      "title": "Review PR #42",
      "description": "Code review for the new feature branch",
      "completed": true,
      "dueDate": "2025-02-28T17:00:00Z",
      "priority": "urgent",
      "category": "Work",
      "createdAt": "2025-02-27T14:00:00Z",
      "updatedAt": "2025-02-28T16:30:00Z"
    }
  ],
  "categories": [
    {
      "id": "C001",
      "name": "Work",
      "color": "#6366F1",
      "icon": "briefcase"
    },
    {
      "id": "C002",
      "name": "Personal",
      "color": "#10B981",
      "icon": "user"
    },
    {
      "id": "C003",
      "name": "Shopping",
      "color": "#F59E0B",
      "icon": "shopping_cart"
    }
  ]
}
```

---

## 6. Models Example (`specs/features/task-list/models.md`)

```dart
// Domain Models

class Task {
  final String id;
  final String title;
  final String? description;
  final bool completed;
  final DateTime? dueDate;
  final TaskPriority priority;
  final String? categoryId;
  final DateTime createdAt;
  final DateTime updatedAt;

  const Task({
    required this.id,
    required this.title,
    this.description,
    required this.completed,
    this.dueDate,
    required this.priority,
    this.categoryId,
    required this.createdAt,
    required this.updatedAt,
  });

  Task copyWith({
    String? id,
    String? title,
    String? description,
    bool? completed,
    DateTime? dueDate,
    TaskPriority? priority,
    String? categoryId,
    DateTime? createdAt,
    DateTime? updatedAt,
  }) {
    return Task(
      id: id ?? this.id,
      title: title ?? this.title,
      description: description ?? this.description,
      completed: completed ?? this.completed,
      dueDate: dueDate ?? this.dueDate,
      priority: priority ?? this.priority,
      categoryId: categoryId ?? this.categoryId,
      createdAt: createdAt ?? this.createdAt,
      updatedAt: updatedAt ?? this.updatedAt,
    );
  }
}

enum TaskPriority {
  low,
  medium,
  high,
  urgent;

  int get value => index;
}

class Category {
  final String id;
  final String name;
  final String color;
  final String icon;

  const Category({
    required this.id,
    required this.name,
    required this.color,
    required this.icon,
  });
}

// Props Classes

class TaskListScreenProps {
  final List<Task> tasks;
  final Map<String, Category> categories;
  final void Function(String taskId) onToggleComplete;
  final void Function(String taskId) onDeleteTask;
  final void Function(String taskId) onTapTask;
  final VoidCallback onAddTask;
  final bool isLoading;
  final String? errorMessage;

  const TaskListScreenProps({
    required this.tasks,
    required this.categories,
    required this.onToggleComplete,
    required this.onDeleteTask,
    required this.onTapTask,
    required this.onAddTask,
    required this.isLoading,
    this.errorMessage,
  });
}
```

---

## Implementation Structure

When this feature is implemented via `/vibeflow:feature`, it generates:

```
lib/features/task-list/
├── domain/
│   ├── models/
│   │   ├── task.dart
│   │   ├── task_priority.dart
│   │   └── category.dart
│   └── repositories/
│       └── task_repository.dart
├── data/
│   ├── datasources/
│   │   └── task_local_datasource.dart
│   └── repositories/
│       └── task_repository_impl.dart
├── providers/
│   └── task_list_provider.dart
├── screens/
│   ├── task_list_screen.dart
│   ├── task_list_view.dart
│   └── task_list_page.dart
├── widgets/
│   ├── task_card.dart
│   ├── task_filter_chip.dart
│   └── empty_state.dart
└── routes.dart
```

---

## Using This Example

1. **Reference:** See what files VibeFlow generates
2. **Structure:** Understand the feature-first architecture
3. **Data format:** Learn how to structure sample data
4. **Spec format:** Follow the feature spec template

To build your own app:
1. Run `/vibeflow:new` and describe your app
2. VibeFlow will generate similar files based on your description
3. Use `/vibeflow:feature` to implement each feature
