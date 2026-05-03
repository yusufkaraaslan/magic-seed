# Wizard: Docs

> **Profile:** Web Frontend

## When to Use
Documentation maintenance.

## Frontend Documentation

### Component Documentation Template

```markdown
# Component: {Name}

## Purpose
What this component does.

## Props
| Prop | Type | Required | Description |
|------|------|----------|-------------|
| {name} | {type} | {yes/no} | {desc} |

## Usage
```tsx
<Component
  prop1="value"
  prop2={42}
/>
```

## Accessibility
- ARIA roles
- Keyboard navigation
- Screen reader support

## Styling
- CSS classes
- Theme variables
- Responsive behavior
```

## Customization
Add Storybook references.
