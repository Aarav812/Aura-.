## 2024-05-18 - Accessible Labels for Icon-Only Buttons and Inputs
**Learning:** Icon-only buttons without text content and form inputs without proper `<label>` elements are inaccessible to screen reader users. Simply using `placeholder` on inputs or assuming icon meaning is visually obvious is not sufficient.
**Action:** Always ensure icon-only buttons include `aria-label` and `title` attributes. Ensure all form inputs have associated `<label>` tags or `aria-label` attributes for screen readers.
## 2024-05-18 - Missing ARIA Labels on Dynamically Created Icon Buttons
**Learning:** Icon-only action buttons dynamically created via JS (e.g., in `frontend/js/chat.js` for the AI response action bar) often lack `aria-label` attributes, making them inaccessible to screen readers.
**Action:** Always ensure that dynamically created DOM elements, especially icon-only buttons, have an `aria-label` attribute explicitly set (e.g., `button.setAttribute('aria-label', 'Description')`) alongside any `title` attribute.
