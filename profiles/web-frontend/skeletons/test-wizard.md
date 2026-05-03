# Wizard: Test

> **Profile:** Web Frontend

## When to Use
After implement-wizard or standalone.

## Test Examples

```tsx
// Unit test
import { render, screen, fireEvent } from '@testing-library/react';

test('increments counter', () => {
  render(<Counter />);
  fireEvent.click(screen.getByText('Increment'));
  expect(screen.getByText('1')).toBeInTheDocument();
});

// Integration test
test('fetches and displays user', async () => {
  render(<UserProfile userId="123" />);
  expect(await screen.findByText('John Doe')).toBeInTheDocument();
});
```

## Customization
Add visual regression tests, accessibility tests.
