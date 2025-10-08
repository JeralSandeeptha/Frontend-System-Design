# Integration Testing

🧠 What it is:
- Testing multiple units working together — e.g., a React component interacting with a service or API mock.

🎯 Purpose:
- Ensure components and modules work correctly when combined.

⚙️ Tools:

- `Jest` + `React Testing Library`
- `Cypress` Component Testing
- `Playwright` (for integrated UI testing)

```js
render(<LoginForm />);
fireEvent.change(screen.getByLabelText(/username/i), { target: { value: 'jeral' } });
fireEvent.change(screen.getByLabelText(/password/i), { target: { value: '1234' } });
fireEvent.click(screen.getByRole('button', { name: /login/i }));

expect(await screen.findByText(/welcome jeral/i)).toBeInTheDocument();
```