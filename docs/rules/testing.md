# Testing — coverage gate

Apply when application code exists in `lib/` or shared modules.

## Gate (when configured)

Target: unit test coverage in `lib/` stays above **85%** across statements, branches, functions, and lines. Enforce via pre-push hook + vitest/jest thresholds when the test harness is bootstrapped.

## Rules

- Every new shared logic file in `lib/` must have a corresponding `.test.ts`
- Every new branch, gate, or edge case in existing `lib/` code needs a test
- Do NOT test: React components, hooks, server actions, files requiring Prisma/Stripe/Seam live calls — use integration tests separately when harness exists
- Do NOT mock the database in unit tests — skip files that genuinely require external deps rather than writing fake tests
- Multi-tenant logic: test tenant isolation boundaries explicitly

## Domain-specific test priorities

| Domain | Must test |
|---|---|
| Governance rails (CAP-5) | Threshold blocking, escalation routing |
| Module toggles (CAP-6) | AI vs human path selection per module |
| Multi-tenant (CAP-11) | Cross-tenant query rejection |
| Accounting (CAP-4) | Double-entry invariants, categorization rules |
| Leasing (CAP-2) | State machine transitions, gate conditions |

## Commands (when harness exists)

```bash
# Check coverage
npm run test:coverage

# Run tests only
npm test

# Run a specific file
npx vitest run lib/path/to/module.test.ts

# Type check
npx tsc --noEmit
```

## Completion gate

Before marking shared logic done:

- [ ] Tests added for new `lib/` modules
- [ ] Coverage gate passes (when configured)
- [ ] Type check passes with zero new errors in changed files
