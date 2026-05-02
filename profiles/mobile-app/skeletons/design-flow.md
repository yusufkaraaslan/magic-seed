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

### Phase 1: UNDERSTAND
Load context.

### Phase 2: DESIGN (Visual-First)
Generate screen flow and widget/component diagrams.

### Phase 3: SPECIFY
Create docs with mobile specifics:
- Screen architecture
- State management
- Navigation flow
- Platform-specific code
- Accessibility
- Performance

### Phase 4: PLAN
**Purpose:** Create implementation task flows based on task analysis.

**Process:**
1. AI analyzes task and proposes task flows (kebab-case names):
   - Examples: `login-screen.md`, `user-profile-widget.md`, `push-notification-service.md`
2. Present to developer with rationale
3. Developer approves/modifies/redefines
4. Create task flow files with frontmatter + tags
5. Create issue dependency diagram

**Suggested:** Offer to add tests.md as optional issue

### Phase 5: FINALIZE
Lock design.

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
