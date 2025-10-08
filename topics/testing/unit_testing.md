# Unit Testing

🧠 What it is:
- Testing individual, isolated pieces of code — usually small functions, React components, or utilities — to ensure they behave as expected.

🎯 Purpose:
- Catch bugs early by verifying logic in the smallest possible units.

⚙️ Common Tools:

- `Jest` (most popular for JS/TS)
- `Vitest` (fast alternative for Vite-based projects)
- `Testing` Library (React/Vue/Angular Testing Library) for component behavior

```js
// sum.js
export const sum = (a, b) => a + b;

// sum.test.js
import { sum } from './sum';
test('adds 1 + 2 = 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```