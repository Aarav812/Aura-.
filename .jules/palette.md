## 2024-05-17 - Added aria-hidden to icon elements
**Learning:** Found that material-symbols-outlined icon elements were missing aria-hidden="true" in index.html, chat.html, and chat.js, which could cause screen readers to announce the ligature text instead of ignoring the decorative icon.
**Action:** Used python script to add aria-hidden="true" to all <span class="material-symbols-outlined"> elements in the main html and js files.
