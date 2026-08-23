## 2025-02-27 - [XSS Fix] `innerHTML` Injection in `appendMessage`
**Vulnerability:** A Cross-Site Scripting (XSS) vulnerability was present in the `appendMessage` function of `frontend/js/chat.js` due to the direct assignment of raw, unescaped text to `bubble.innerHTML` when `isRawHtmlForUser` was true.
**Learning:** Even internal formatting assumptions for variables intended as "raw HTML" shouldn't bypass basic security sanitation.
**Prevention:** Always use `DOMPurify.sanitize()` (or a similar sanitization utility) on any dynamically generated content inserted into the DOM via `innerHTML`, even if the source is intended to be safe UI output.
