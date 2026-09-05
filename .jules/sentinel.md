## 2024-05-18 - Math.random() usage for sensitive identifiers
**Vulnerability:** The codebase was using `Math.random()` to generate unique identifiers (e.g., `generateId()`). `Math.random()` is not cryptographically secure, and its outputs can be predicted by an attacker, leading to ID collisions or predictability vulnerabilities.
**Learning:** This existed because `Math.random()` is convenient and built into older JavaScript environments, often used without considering the security implications in modern applications.
**Prevention:** Always use `window.crypto.randomUUID()` when available, or `window.crypto.getRandomValues()` as a secure fallback, for generating identifiers, tokens, or any random values where unpredictability is required.
