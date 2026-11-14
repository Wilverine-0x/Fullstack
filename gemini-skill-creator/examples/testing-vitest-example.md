# Example: Vitest Testing Skill Reference

This demonstrates a completed `SKILL.md` constructed using the `gemini-skill-creator` standards.

```markdown
---
name: testing-vitest-components
description: Automates React component testing with Vitest and Testing Library. Use when writing unit tests, adding component test coverage, or validating React components.
---

# Testing Vitest Components

Guides the automated creation and execution of unit/component tests for React components using Vitest and @testing-library/react.

## When to Use This Skill
- When user asks to write tests for a React component
- When validating component behavior with Vitest
- When setting up mock interactions with userEvent

## Workflow
- [ ] 1. Identify component props, state, and user interactions to test
- [ ] 2. Check if Vitest and Testing Library dependencies exist in `package.json`
- [ ] 3. Write test file following the `<Component>.test.tsx` naming convention
- [ ] 4. Run `npm test -- --run` to execute test suite
- [ ] 5. Verify coverage and fix failing assertions

## Instructions & Core Patterns

### Component Test Standard
```tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { describe, it, expect, vi } from 'vitest';
import { Button } from './Button';

describe('Button component', () => {
  it('renders correctly with label', () => {
    render(<Button label="Submit" />);
    expect(screen.getByRole('button', { name: /submit/i })).toBeInTheDocument();
  });

  it('triggers onClick handler when clicked', async () => {
    const handleClick = vi.fn();
    const user = userEvent.setup();
    render(<Button label="Submit" onClick={handleClick} />);
    
    await user.click(screen.getByRole('button', { name: /submit/i }));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

### Fragile Commands
```bash
# Run single test file
npx vitest run src/components/Button.test.tsx
```
```
