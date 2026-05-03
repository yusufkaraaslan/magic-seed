# Wizard: Design

> **Profile:** Mobile App

## When to Use
New screens, features, or system changes.

## Output
```
docs/features/{name}/
├── DESIGN.md
├── TDD.md
├── EDGE-CASES.md
├── diagrams/
└── issues/
    ├── 01-{descriptive-name}.md    # Dynamically named (e.g., login-screen)
    ├── 02-{descriptive-name}.md
    └── ...                         # AI proposes based on feature analysis
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
**Purpose:** Create implementation issues based on feature analysis.

**Process:**
1. AI analyzes feature and proposes issues (kebab-case names):
   - Examples: `login-screen.md`, `user-profile-widget.md`, `push-notification-service.md`
2. Present to developer with rationale
3. Developer approves/modifies/redefines
4. Create issue files with frontmatter + tags
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
