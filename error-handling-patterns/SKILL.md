---
name: error-handling-patterns
description: Master error handling and troubleshooting patterns across languages and web apps, including exceptions, Result types, error propagation, retry strategies, and graceful degradation. Use when debugging issues, fixing broken applications, designing APIs, or improving application reliability.
---

# Error Handling & Troubleshooting Patterns

Build resilient applications and systematically troubleshoot, diagnose, and fix failures across web frontends, backend services, and APIs. Effective troubleshooting pairs scientific root-cause isolation with robust error-handling architecture to ensure bugs are permanently resolved.

## When to Use This Skill
- Troubleshooting and fixing broken features, runtime crashes, or API failures in applications and websites.
- Implementing error handling in new features or refactoring fragile code.
- Designing error-resilient APIs and client-server communication contracts.
- Debugging production exceptions, uncaught promise rejections, or blank-screen errors.
- Implementing retry, timeout, fallback, and circuit-breaker patterns.
- Creating actionable error messages for end users while preserving deep diagnostic telemetry for developers.

---

## Troubleshooting & Resolution Workflow

Follow this 5-stage loop whenever diagnosing and fixing application or website issues:

- [ ] **Stage 1: Reproduce & Capture Context**
  - Isolate the minimum reproduction steps and payload.
  - Inspect full stack traces, HTTP status codes, console logs, network payloads, and environment variables.
  - Determine whether the error is client-side (UI/network), server-side (logic/database), or integration-related.
- [ ] **Stage 2: Categorize Failure Domain**
  - **Recoverable:** Network timeout, rate limit, validation error, missing resource (handle gracefully, retry, or prompt user).
  - **Unrecoverable:** Out of memory, stack overflow, database partition, severe hardware limit (fail fast, exit cleanly, or alert).
  - **Logic/Programming Bug:** Null/undefined dereference, type mismatch, off-by-one, race condition (patch root cause).
- [ ] **Stage 3: Trace Root Cause (Five Whys)**
  - Look beyond the immediate symptom. Trace upstream inputs to find where invalid state was first introduced.
  - Verify assumptions against real data: inspect schema contracts, database state, and third-party API specs.
- [ ] **Stage 4: Implement Resilient Pattern**
  - Choose the appropriate mechanism: Exception wrapping (`cause: error`), typed `Result<T, E>`, or graceful UI fallback.
  - Ensure resources (connections, handles, streams) are cleaned up via `finally` or disposables.
  - Preserve context: chain original errors without discarding inner stack traces.
- [ ] **Stage 5: Verify & Prevent Regression**
  - Test the happy path and specifically inject edge-case/fault scenarios (e.g. offline mode, 500 status, null payload).
  - Add regression unit/integration tests to prevent the bug from recurring.

---

## Core Concepts & Architectures

### 1. Error Handling Philosophies

| Approach | Typical Ecosystems | When to Use | Trade-offs |
| :--- | :--- | :--- | :--- |
| **Exceptions** | Python, Java, C#, JS/TS (`throw`) | Unexpected errors, exceptional runtime faults, deep bailouts. | Can disrupt control flow; requires discipline to catch at the appropriate boundary. |
| **Result Types** | Rust (`Result<T, E>`), Go (`val, err`), TS functional | Expected domain errors, validation failures, business logic branches. | Explicit and compiler-enforced; requires chaining/unwrapping logic. |
| **Error Codes** | C, low-level OS, POSIX | Low-level systems programming without runtime overhead. | Easy to ignore if callers fail to check return codes. |
| **Option / Maybe** | Rust (`Option<T>`), Swift, Kotlin, TS (`T \| null`) | Missing keys, optional query parameters, empty lookup results. | Clarifies intentional absence vs. execution error. |

### 2. Error Categorization & Action Plan

```
                   ┌─────────────────────────────────┐
                   │        Encountered Error        │
                   └────────────────┬────────────────┘
                                    │
           ┌────────────────────────┴────────────────────────┐
           ▼                                                 ▼
   [Recoverable Error]                             [Unrecoverable Error]
   - Network timeout                               - Out of memory / crash
   - Invalid form input                            - Fatal assertion failure
   - Rate limited (429)                            - Corrupted state
           │                                                 │
           ▼                                                 ▼
   - Exponential backoff retry                     - Log critical diagnostic
   - User feedback / actionable banner             - Fail fast / process restart
   - Fallback cache / degraded mode                - Graceful shutdown of handles
```

---

## Best Practices

1. **Fail Fast & Validate at Boundaries:** Validate user input and external payloads at the door (using Zod, Pydantic, or manual guards). Don't let corrupted data propagate deep into business logic.
2. **Preserve Context with Error Chaining:** Always wrap external or lower-level errors with contextual metadata and chain the original cause:
   - Python: `raise ServiceError(...) from err`
   - JavaScript/TypeScript: `new Error("Order processing failed", { cause: err })`
3. **Meaningful Error Messages:**
   - **For Users:** Plain, jargon-free explanation with an actionable next step (*"Unable to process payment. Please verify your card details or try another card."*).
   - **For Developers:** Exact entity ID, attempted operation, timestamp, and inner trace.
4. **Clean Up Resources:** Always release memory, database pools, and file handles using `finally`, Python `with`, or Go `defer`.
5. **Never Swallow Errors Silently:** Never write empty `catch` blocks (`catch (e) {}` or `except: pass`). If an error is truly benign, log an explanatory comment explaining why it is safe to ignore.
6. **Log at the Right Boundary:** Catch and handle where you have the context to recover. Catch and log once at the outer boundary rather than logging and re-throwing at every intermediate layer.

---

## Reference Implementation Patterns

### Pattern A: Comprehensive Python Service Error Handling
```python
import logging
from typing import Optional

logger = logging.getLogger(__name__)

class ApplicationError(Exception):
    """Base application exception for domain errors."""
    def __init__(self, message: str, code: str = "INTERNAL_ERROR", details: Optional[dict] = None):
        super().__init__(message)
        self.code = code
        self.details = details or {}

class ValidationError(ApplicationError):
    def __init__(self, message: str, field: str):
        super().__init__(message, code="VALIDATION_FAILED", details={"field": field})

class ExternalServiceError(ApplicationError):
    def __init__(self, message: str, service: str, details: Optional[dict] = None):
        super().__init__(message, code=f"{service.upper()}_ERROR", details=details)


def process_order(order_id: str, db, payment_service) -> dict:
    """Process order with strict validation, error wrapping, and clean resource cleanup."""
    # 1. Fail fast on invalid inputs
    if not order_id or not isinstance(order_id, str):
        raise ValidationError("Valid Order ID is required", field="order_id")

    # 2. Domain checks
    order = db.get_order(order_id)
    if not order:
        raise ApplicationError(f"Order '{order_id}' not found", code="NOT_FOUND")

    # 3. Handle external integration failures
    try:
        charge_result = payment_service.charge(order.total)
    except Exception as exc:
        logger.error("Payment gateway failure for order %s: %s", order_id, exc)
        raise ExternalServiceError(
            "Payment processing failed. Please verify funds and retry.",
            service="payment_gateway",
            details={"order_id": order_id, "amount": order.total}
        ) from exc

    order.status = "completed"
    order.payment_id = charge_result.id
    db.save(order)
    return {"order_id": order.id, "status": order.status}
```

### Pattern B: Typed Result Pattern in TypeScript
```typescript
export type Result<T, E = Error> =
  | { ok: true; value: T }
  | { ok: false; error: E };

export const Ok = <T>(value: T): Result<T, never> => ({ ok: true, value });
export const Err = <E>(error: E): Result<never, E> => ({ ok: false, error });

// Usage in async API client
export async function fetchUserPreferences(userId: string): Promise<Result<UserPrefs, NetworkError>> {
  if (!userId) {
    return Err(new NetworkError('User ID is required', 400));
  }

  try {
    const res = await fetch(`/api/users/${encodeURIComponent(userId)}/preferences`);
    if (!res.ok) {
      return Err(new NetworkError(`Failed with status ${res.status}`, res.status));
    }
    const data = await res.json();
    return Ok(data);
  } catch (cause) {
    return Err(new NetworkError('Network unreachable', 0, cause));
  }
}
```

---

## Common Pitfalls & Anti-Patterns

| Anti-Pattern | Why It Breaks Applications | Correct Pattern |
| :--- | :--- | :--- |
| **Catching generic `Exception` / `any`** | Masks syntax errors, typos, and memory panics. | Catch specific domain errors; let unexpected errors surface to top-level handlers. |
| **Empty catch block (`catch {}`)** | Causes silent bugs; leaves UI in an unresponsive zombie state. | Log the issue or present a fallback state to the user. |
| **Log and re-throw (`logger.error(e); throw e`)** | Creates noisy duplicate log lines for a single incident. | Either handle and log, OR enrich and re-throw. Log once at top boundary. |
| **Ignoring Async Promises** | Node.js process crashes or silent browser UI freeze. | Always handle `.catch()` or enclose within `try/catch` with `await`. |
| **Leaking raw database errors to users** | Security hazard (exposes table names, credentials, SQL queries). | Map internal errors to sanitized client codes (e.g., `INTERNAL_SERVER_ERROR`). |

---

## Detailed References
For advanced implementations including Circuit Breakers, Exponential Backoff with Jitter, and React Error Boundaries:
👉 Read **[references/details.md](references/details.md)**
