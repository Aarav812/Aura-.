## 2024-05-18 - Accessible Labels for Icon-Only Buttons and Inputs
**Learning:** Icon-only buttons without text content and form inputs without proper `<label>` elements are inaccessible to screen reader users. Simply using `placeholder` on inputs or assuming icon meaning is visually obvious is not sufficient.
**Action:** Always ensure icon-only buttons include `aria-label` and `title` attributes. Ensure all form inputs have associated `<label>` tags or `aria-label` attributes for screen readers.
