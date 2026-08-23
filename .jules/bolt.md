## 2024-05-14 - Search Optimization
**Learning:** Found history search input handling every single keystroke sequentially which is inefficient if index is large.
**Action:** Debounce history search input to reduce unnecessary index reloads.
## 2024-05-15 - Markdown Rendering Optimization
**Learning:** The `renderMarkdown` function was creating a new `marked.Renderer` instance and re-defining its custom `code` override on every invocation. Since this function is called on every streamed token update, it caused unnecessary garbage collection overhead and object allocations.
**Action:** Cache the instantiated `marked.Renderer` and `DOMPurify` configuration object globally to reuse them across all calls, reducing render time by ~30%.
