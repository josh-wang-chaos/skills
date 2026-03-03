# Good and Bad Tests

## Good Tests

**Integration-style**: Test through real interfaces, not mocks of internal parts.

```typescript
// GOOD: Tests observable behavior
test('user can checkout with valid cart', async () => {
  const cart = createCart();
  cart.add(product);
  const result = await checkout(cart, paymentMethod);
  expect(result.status).toBe('confirmed');
});
```

Characteristics:

- Tests behavior users/callers care about
- Uses public API only
- Survives internal refactors
- Describes WHAT, not HOW
- One logical assertion per test

## Bad Tests

### 1. Implementation-detail tests

Coupled to internal structure.

```typescript
// BAD: Tests implementation details
test('checkout calls paymentService.process', async () => {
  const mockPayment = jest.mock(paymentService);
  await checkout(cart, payment);
  expect(mockPayment.process).toHaveBeenCalledWith(cart.total);
});
```

Red flags:

- Mocking internal collaborators
- Testing private methods
- Asserting on call counts/order
- Test breaks when refactoring without behavior change
- Test name describes HOW not WHAT
- Verifying through external means instead of interface

```typescript
// BAD: Bypasses interface to verify
test('createUser saves to database', async () => {
  await createUser({ name: 'Alice' });
  const row = await db.query('SELECT * FROM users WHERE name = ?', ['Alice']);
  expect(row).toBeDefined();
});

// GOOD: Verifies through interface
test('createUser makes user retrievable', async () => {
  const user = await createUser({ name: 'Alice' });
  const retrieved = await getUser(user.id);
  expect(retrieved.name).toBe('Alice');
});
```

### 2. Render / UI tests

Do not write tests that render a component and check what's in the DOM. React rendering is not our code — don't test it. These tests verify static markup, break on every copy change, and pass even when the component is functionally broken.

```tsx
// BAD: All of these are render tests — do not write them
test('renders card title', () => {
  render(<KpiGrid />);
  expect(screen.getByText('Commissions')).toBeInTheDocument();
});
test('shows 6 skeleton loaders', () => {
  render(<Dashboard />);
  expect(document.querySelectorAll('.animate-pulse')).toHaveLength(6);
});
test('has a refresh button', () => {
  render(<Table />);
  expect(screen.getByRole('button', { name: /refresh/i })).toBeInTheDocument();
});
```

**What to test instead**: hooks, stores, pure functions, and user interactions that trigger logic.

```typescript
// GOOD: Hook integration test
test('fetches filtered data when product param is set', async () => {
  const { result } = renderHook(() => useSummary({ product: 'casino' }), { wrapper });
  await waitFor(() => expect(result.current.data).toBeDefined());
  expect(result.current.data.sum_sign_ups).toBeLessThan(1923);
});

// GOOD: Store state test
test('updates product when filter is clicked', async () => {
  const user = userEvent.setup();
  renderWithProviders(<Filters />);
  await user.click(screen.getByRole('radio', { name: 'Casino' }));
  expect(useStore.getState().product).toBe('casino');
});

// GOOD: Pure function test
test('returns 0% delta when previous period is zero', () => {
  expect(computeDelta(100, 0)).toBe(0);
});
```
