# Flow: Implement

> **Profile:** Web Frontend

## When to Use
After design-flow.

## Phases

### Phase 1: READ
Load context, read issue.

### Phase 2: PLAN
Plan component implementation:
- Component structure
- State management
- API calls
- Styling

### Phase 3: IMPLEMENT
Create frontend code:
```tsx
// Component
export const FeatureComponent: React.FC = () => {
  const [data, setData] = useState<Data>();
  
  useEffect(() => {
    fetchData().then(setData);
  }, []);
  
  return (
    <div className="feature">
      {data ? <DataView data={data} /> : <Loading />}
    </div>
  );
};
```

### Phase 4: AUTO-VALIDATE
- [ ] No `any` types
- [ ] Accessibility attributes
- [ ] Responsive design
- [ ] Error handling

### Phase 5: DOC-SYNC
Update docs.

### Phase 6: DEVELOPER REVIEW
Present for approval.

### Phase 7: UPDATE ISSUE
Transform to knowledge record.

### Phase 8: COMMIT
Commit.

## Resume Logic
Check issue status.
