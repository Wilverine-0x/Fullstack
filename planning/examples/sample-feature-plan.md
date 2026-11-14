# Sample Feature Implementation Plan: Auth Token Cache

> **Goal:** Provide an in-memory, thread-safe token cache with automatic TTL expiration and refresh triggers.  
> **Architecture:** Singleton token manager exposing reactive observable state with mutex-protected background refresh.  
> **Tech Stack:** TypeScript, Vitest, Node.js  
> **Spec Reference:** `docs/specs/2026-10-17-token-cache-design.md`  

---

## Global Constraints
- Zero external runtime dependencies (use native `crypto` and `Map`).
- TypeScript strict mode with 100% test branch coverage on cache eviction logic.

---

## Review Focus
1. Expired token requested concurrently by multiple threads: Must coalesce into a single refresh call.
2. Refresh API returns HTTP 500: Cache must retain stale token if still within grace period, otherwise emit error.
3. Clock skew / system time rollback: Use monotonic timer (`process.hrtime`) for TTL rather than `Date.now()`.
4. High cache eviction churn: Ensure entries delete cleanly without memory leak.
5. Malformed payload from auth provider: Reject without crashing host process.

---

## Tasks

### Task 1: Token Cache Core Storage & Expiration
**Files:**
- Create: `src/auth/token-cache.ts`
- Test: `tests/auth/token-cache.test.ts`

**Interfaces:**
- Consumes: None
- Produces: `export class TokenCache { set(key: string, token: string, ttlMs: number): void; get(key: string): string | null; }`

- [ ] **Step 1: Write failing test**
```typescript
import { describe, it, expect } from 'vitest';
import { TokenCache } from '../../src/auth/token-cache';

describe('TokenCache', () => {
  it('returns null for nonexistent or expired key', async () => {
    const cache = new TokenCache();
    cache.set('session', 'abc-123', 50);
    expect(cache.get('session')).toBe('abc-123');
    await new Promise((r) => setTimeout(r, 60));
    expect(cache.get('session')).toBeNull();
  });
});
```

- [ ] **Step 2: Run test to verify failure**
  Run: `npx vitest run tests/auth/token-cache.test.ts`
  Expected: FAIL with `TokenCache is not defined`

- [ ] **Step 3: Implement minimal code in `src/auth/token-cache.ts`**
  Implement map-based storage tracking monotonic expiration timestamp.

- [ ] **Step 4: Verify test passes**
  Run: `npx vitest run tests/auth/token-cache.test.ts`
  Expected: PASS

- [ ] **Step 5: Commit**
  Run: `git add src/auth/token-cache.ts tests/auth/token-cache.test.ts && git commit -m "feat(auth): implement core token cache with TTL expiration"`
