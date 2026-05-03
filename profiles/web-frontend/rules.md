# Rules: Web Frontend

## Architecture Rules

### Rule W1: Component Purity

Presentational components should not import state management.

```tsx
// ✓ Good: Pure component
interface ButtonProps {
  label: string;
  onClick: () => void;
}

export const Button: React.FC<ButtonProps> = ({ label, onClick }) => {
  return <button onClick={onClick}>{label}</button>;
};

// ✗ Bad: Component imports Redux
export const Button = () => {
  const dispatch = useDispatch(); // Wrong layer
  // ...
};
```

### Rule W2: Custom Hooks for Logic

Extract reusable logic into custom hooks.

```tsx
// ✓ Good: Custom hook
function useCounter() {
  const [count, setCount] = useState(0);
  const increment = () => setCount(c => c + 1);
  return { count, increment };
}

// Used in component
export const Counter = () => {
  const { count, increment } = useCounter();
  return <button onClick={increment}>{count}</button>;
};
```

## Code Style

### Rule W3: Naming Conventions

- **Components:** PascalCase (`UserProfile.tsx`)
- **Hooks:** camelCase with `use` prefix (`useAuth.ts`)
- **Utilities:** camelCase (`formatDate.ts`)
- **Constants:** UPPER_SNAKE_CASE

### Rule W4: Props Interface

Always define props interface.

```tsx
// ✓ Good
interface UserCardProps {
  user: User;
  onEdit: (user: User) => void;
}

export const UserCard: React.FC<UserCardProps> = ({ user, onEdit }) => {
  // ...
};
```

## Performance

### Rule W5: Memoize Expensive Computations

Use `useMemo` and `useCallback` appropriately.

```tsx
// ✓ Good
const filteredUsers = useMemo(() => {
  return users.filter(u => u.isActive);
}, [users]);

const handleClick = useCallback(() => {
  onSelect(user);
}, [user, onSelect]);
```

### Rule W6: Lazy Load Routes

```tsx
const Dashboard = lazy(() => import('./pages/Dashboard'));

<Route path="/dashboard" element={<Suspense><Dashboard /></Suspense>} />
```

## Accessibility

### Rule W7: Semantic HTML

Use proper HTML elements.

```tsx
// ✗ Bad
<div onClick={handleClick}>Click me</div>

// ✓ Good
<button onClick={handleClick}>Click me</button>
```

### Rule W8: ARIA Attributes

```tsx
// ✓ Good
<button aria-label="Close dialog" onClick={onClose}>
  <CloseIcon />
</button>
```

## Testing

### Rule W9: Test Behavior, Not Implementation

```tsx
// ✓ Good: Test what user sees
fireEvent.click(screen.getByText('Submit'));
expect(screen.getByText('Success!')).toBeInTheDocument();

// ✗ Bad: Test implementation details
expect(wrapper.instance().state.isSubmitting).toBe(true);
```

## Validation Checklist

- [ ] Components are pure (no state imports)
- [ ] Custom hooks for reusable logic
- [ ] Props interfaces defined
- [ ] Expensive computations memoized
- [ ] Routes lazy loaded
- [ ] Semantic HTML used
- [ ] ARIA attributes present
- [ ] Tests check behavior not implementation
- [ ] No `any` types (TypeScript)
- [ ] Error boundaries implemented
