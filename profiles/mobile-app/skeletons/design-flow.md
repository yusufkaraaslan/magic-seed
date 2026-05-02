# Flow: Design

> **Profile:** Mobile App

## When to Use
New screens, tasks, or system changes.

## Output
```
flow-storage/tasks/{task-name}/
├── design/
│   ├── task-design.md
│   ├── task-technical-design.md
│   ├── task-edge-cases.md
│   └── diagrams/
└── implement/
    └── flow-plan/
        ├── task-flow-01-{descriptive-name}.md   # Dynamically named (e.g., login-screen)
        ├── task-flow-02-{descriptive-name}.md
        └── ...                                  # AI proposes based on task analysis
```

## Phases

This profile follows the canonical **2-phase, 2-gate** structure from `universal/workflow-structure.md` (and `profiles/generic/skeletons/design-flow.md`). Mobile-app specifics are in the sub-tasks.

### Phase 1: SPECIFY *(STANDARD gate)*
- **1.1 UNDERSTAND** — Load context, explore codebase (screens/, widgets/, services/, navigation/).
- **1.2 DESIGN (Visual-First)** — Generate screen flow + widget/component diagrams (class + package + sequence as needed). Render every `.puml` to `.svg`.
- **1.3 SPECIFY** — Create docs with **mobile-app specifics**:
  - Screen architecture
  - State management
  - Navigation flow
  - Platform-specific code
  - Accessibility
  - Performance
- → **Gate 1: Design package review** — diagrams + spec docs together.

### Phase 2: COMMIT *(CRITICAL gate)*
- **2.1 PLAN** — Decompose into implementation task flows (kebab-case names):
  - Examples: `login-screen.md`, `user-profile-widget.md`, `push-notification-service.md`
  - Create files with frontmatter + tags
  - Generate task-flow dependency diagram
  - Suggested: offer to add `tests.md` as optional task flow
- **2.2 FINALIZE** — Sign off task-design.md (immutable per Rule 9).
- **2.3 COMMIT** *(executes only after the gate is accepted)* — Stage `flow-storage/tasks/{task-name}/`; commit `design({task-name}): sign off task-design.md and {N} task flows`. Skip 2.3 if `--no-commit`.
- → **Gate 2: Plan + final approval** — presented BEFORE 2.3 executes git commit.

## Mobile Patterns

```dart
// Flutter: Screen with state management
class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Home')),
      body: Consumer<HomeViewModel>(
        builder: (context, viewModel, child) {
          if (viewModel.isLoading)
            return Center(child: CircularProgressIndicator());
          return ListView.builder(
            itemCount: viewModel.items.length,
            itemBuilder: (context, index) => 
              ListTile(title: Text(viewModel.items[index].title)),
          );
        },
      ),
    );
  }
}
```
