# Flow: Implement

> **Profile:** Mobile App

## When to Use
After design-flow.

## Phases

### Phase 1: READ
Load context.

### Phase 2: PLAN
Plan implementation:
- Models
- Screens/widgets
- State management
- API integration

### Phase 3: IMPLEMENT
```dart
// Flutter: Task implementation
class FeatureViewModel extends ChangeNotifier {
  List<Item> _items = [];
  bool _isLoading = false;
  
  List<Item> get items => _items;
  bool get isLoading => _isLoading;
  
  Future<void> loadItems() async {
    _isLoading = true;
    notifyListeners();
    
    _items = await _repository.fetchItems();
    _isLoading = false;
    notifyListeners();
  }
}
```

### Phase 4: AUTO-VALIDATE
- [ ] Platform-specific code isolated
- [ ] Accessibility labels
- [ ] Responsive design
- [ ] Error handling

### Phase 5: DOC-SYNC
Update docs.

### Phase 6: DEVELOPER REVIEW
Present.

### Phase 7: UPDATE ISSUE
Knowledge record.

### Phase 8: COMMIT
Commit.

## Resume Logic
Check issue status.
