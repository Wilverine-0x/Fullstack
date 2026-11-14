# Error Handling & Troubleshooting Deep Reference

This reference covers advanced patterns, algorithms, and worked implementations for building fault-tolerant applications and websites.

---

## 1. Retry with Exponential Backoff and Full Jitter

Blind immediate retries can overwhelm an already struggling backend (the *thundering herd* problem). Always combine exponential backoff with randomized jitter.

### Formula
$$ \text{Delay} = \text{random}(0, \min(\text{MaxDelay}, \text{BaseDelay} \times 2^{\text{attempt}})) $$

### TypeScript / JavaScript Implementation
```typescript
interface RetryOptions {
  maxAttempts?: number;
  baseDelayMs?: number;
  maxDelayMs?: number;
  shouldRetry?: (error: unknown) => boolean;
}

export async function retryWithBackoff<T>(
  operation: () => Promise<T>,
  options: RetryOptions = {}
): Promise<T> {
  const {
    maxAttempts = 3,
    baseDelayMs = 200,
    maxDelayMs = 3000,
    shouldRetry = () => true,
  } = options;

  let attempt = 0;

  while (attempt < maxAttempts) {
    try {
      return await operation();
    } catch (error) {
      attempt++;
      if (attempt >= maxAttempts || !shouldRetry(error)) {
        throw error;
      }

      // Calculate exponential backoff with full jitter
      const exponential = Math.min(maxDelayMs, baseDelayMs * Math.pow(2, attempt));
      const jitteredDelay = Math.random() * exponential;

      await new Promise((resolve) => setTimeout(resolve, jitteredDelay));
    }
  }

  throw new Error("Retry loop exhausted");
}
```

---

## 2. Circuit Breaker Pattern

Prevent cascades of failure across services by temporarily halting calls when failure rates cross a defined threshold.

### State Transitions
- **CLOSED:** Normal execution. Failures are counted.
- **OPEN:** Calls fail fast without hitting the downstream service. A cooldown timer starts.
- **HALF-OPEN:** A limited number of trial calls are permitted. If they succeed, state resets to CLOSED; if any fail, state returns to OPEN.

```typescript
export class CircuitBreaker {
  private failureCount = 0;
  private lastFailureTime = 0;
  private state: 'CLOSED' | 'OPEN' | 'HALF_OPEN' = 'CLOSED';

  constructor(
    private failureThreshold = 5,
    private cooldownPeriodMs = 15000
  ) {}

  async execute<T>(action: () => Promise<T>): Promise<T> {
    const now = Date.now();

    if (this.state === 'OPEN') {
      if (now - this.lastFailureTime > this.cooldownPeriodMs) {
        this.state = 'HALF_OPEN';
      } else {
        throw new Error('CircuitBreaker is OPEN: fast failure to protect downstream service');
      }
    }

    try {
      const result = await action();
      if (this.state === 'HALF_OPEN') {
        this.reset();
      }
      return result;
    } catch (err) {
      this.recordFailure(now);
      throw err;
    }
  }

  private recordFailure(timestamp: number) {
    this.failureCount++;
    this.lastFailureTime = timestamp;
    if (this.failureCount >= this.failureThreshold || this.state === 'HALF_OPEN') {
      this.state = 'OPEN';
    }
  }

  private reset() {
    this.state = 'CLOSED';
    this.failureCount = 0;
  }
}
```

---

## 3. Frontend & Web UI Error Boundaries (React)

Never allow a single component render failure to crash the entire web application to a blank white screen.

```tsx
import React, { Component, ErrorInfo, ReactNode } from 'react';

interface Props {
  children: ReactNode;
  fallback?: ReactNode;
}

interface State {
  hasError: boolean;
  error: Error | null;
}

export class ErrorBoundary extends Component<Props, State> {
  public state: State = { hasError: false, error: null };

  public static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  public componentDidCatch(error: Error, errorInfo: ErrorInfo) {
    // Send telemetry to error tracking service (e.g. Sentry, Datadog)
    console.error('Uncaught error caught by boundary:', error, errorInfo);
  }

  public render() {
    if (this.state.hasError) {
      if (this.props.fallback) return this.props.fallback;

      return (
        <div role="alert" className="p-6 m-4 rounded-lg border border-red-200 bg-red-50 text-red-900">
          <h2 className="text-lg font-semibold mb-2">Something went wrong</h2>
          <p className="text-sm mb-4">This section encountered an unexpected error.</p>
          <button
            onClick={() => this.setState({ hasError: false, error: null })}
            className="px-4 py-2 bg-red-600 text-white rounded text-sm hover:bg-red-700 transition"
          >
            Try again
          </button>
        </div>
      );
    }

    return this.props.children;
  }
}
```

---

## 4. Top-Level Process Protection (Node.js)

Prevent unhandled exceptions from terminating backend services without proper cleanup:

```typescript
process.on('uncaughtException', (err: Error) => {
  logger.fatal({ err }, 'Uncaught exception occurred. Initiating graceful shutdown.');
  // Flush logs and close open database pools before exiting
  gracefulShutdown().finally(() => process.exit(1));
});

process.on('unhandledRejection', (reason: unknown) => {
  logger.error({ reason }, 'Unhandled Promise rejection detected.');
});
```

---

## 5. Web API HTTP Status Code Mapping Matrix

Always map internal exceptions to clean, predictable HTTP status codes:

| Exception Class | HTTP Status | Recommended Error Code |
| :--- | :--- | :--- |
| `ValidationError` | `400 Bad Request` | `VALIDATION_FAILED` |
| `AuthenticationError` | `401 Unauthorized` | `UNAUTHENTICATED` |
| `AuthorizationError` | `403 Forbidden` | `FORBIDDEN` |
| `ResourceNotFoundError` | `404 Not Found` | `NOT_FOUND` |
| `ConflictError` / State mismatch | `409 Conflict` | `STATE_CONFLICT` |
| `RateLimitExceededError` | `429 Too Many Requests` | `RATE_LIMITED` |
| `UpstreamTimeoutError` | `504 Gateway Timeout` | `UPSTREAM_TIMEOUT` |
| `ApplicationError` (unhandled) | `500 Internal Server Error` | `INTERNAL_SERVER_ERROR` |
