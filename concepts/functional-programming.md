# Functional Programming (FP)

## Definition
A paradigm where programs are built by composing pure functions. **Data goes in, data comes out, nothing else happens.**

---

## The Four Pillars

**1. Pure Functions** — same input always returns same output, no side effects.
```javascript
// ❌ depends on external state
function getTotal(price) { return price + (price * tax); }

// ✅ self-contained
function getTotal(price, tax) { return price + (price * tax); }
```

**2. Immutability** — never modify data, create new data with the change applied.
```javascript
// ❌ mutates original
account.balance = account.balance * 1.05;

// ✅ returns new object
return { ...account, balance: account.balance * 1.05 };
```
> *Memory tradeoff:* negligible for most backend work — the JVM GC is optimized for short-lived objects.

**3. First-Class Functions** — functions are values; store them, pass them, return them.
```javascript
const double = x => x * 2;
apply(100, double);        // 200
apply(100, x => x + 50);  // 150
```

**4. Function Composition** — build complex behavior by chaining small focused functions.
```javascript
const pipe = (...fns) => x => fns.reduce((v, f) => f(v), x);
const process = pipe(validate, applyTax, applyDiscount);
process(100); // 106.2
```

---

## Additional Rules
- **No shared state** — all data a function needs comes through parameters.
- **Higher-order functions** — functions that take or return other functions (`map`, `filter`, `reduce`).
- **Referential transparency** — a function call can be replaced by its return value without changing behavior. This works because pure functions are predictable: `add(2, 3)` always returns `5`, so anywhere you see `add(2, 3)` you could write `5` and nothing breaks — just like math. The moment a function depends on something external (a global variable, the clock, a database), this guarantee disappears.

---

## Why Use FP?
FP shines where **shared mutable state is the source of complexity**.

- **Easier to test** — pure functions need no mocking, no setup, no teardown.
- **Safe concurrency** — immutable data means no race conditions, no locks needed.
- **Bugs have nowhere to hide** — nothing changes unexpectedly; functions are readable in isolation.

---

## Languages

| Tier | Languages |
|---|---|
| Purely functional (compiler enforces purity) | Haskell, Elm, PureScript |
| Strong FP support | Scala, Clojure, F#, Elixir, Kotlin |
| Moderate FP support | Java 8+, TypeScript, JavaScript, C#, Python |

---

## Practical Takeaway
Use **OOP for structure** (classes, modules, encapsulation) and **FP for logic** (pure transformations, immutable data, composable functions). The language won't always enforce it — the discipline is yours to apply.