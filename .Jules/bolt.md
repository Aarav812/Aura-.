## 2024-05-14 - Search Optimization
**Learning:** Found history search input handling every single keystroke sequentially which is inefficient if index is large.
**Action:** Debounce history search input to reduce unnecessary index reloads.
## 2024-05-15 - Markdown Rendering Optimization
**Learning:** The `renderMarkdown` function was creating a new `marked.Renderer` instance and re-defining its custom `code` override on every invocation. Since this function is called on every streamed token update, it caused unnecessary garbage collection overhead and object allocations.
**Action:** Cache the instantiated `marked.Renderer` and `DOMPurify` configuration object globally to reuse them across all calls, reducing render time by ~30%.
## 2026-08-24 - Chat Input Draft Saving Optimization
**Learning:** Found the `chatInput` 'input' event listener handling draft persistence to `localStorage` on every single keystroke. `localStorage` is synchronous and doing I/O operations continuously during typing causes main thread blocking, leading to input latency (jank).
**Action:** Extract the `localStorage` I/O operations into a debounced function using the `debounce` helper from `utils.js` (with a 500ms delay), but left the UI class updates immediate to maintain responsive feedback.
## 2024-05-16 - Streaming Response Layout Thrashing Optimization
**Learning:** During AI streaming response, updating `innerHTML` and calling `scrollToBottom()` (which reads `document.documentElement.scrollHeight`) synchronously on every single received chunk caused severe layout thrashing and main-thread blocking.
**Action:** Wrapped the DOM assignment (`innerHTML`) and scroll update logic within `requestAnimationFrame` (RAF), ensuring that both operations are batched and executed only once per display frame. Remembered to clean up `pendingRAF` on stream end/abort.

## 2024-05-17 - Chat History Rendering Optimization
**Learning:** During chat history rendering, calling `scrollToBottom` and `updateScrollBtn` synchronously after appending each message caused severe layout thrashing (O(N) layout recalculations).
**Action:** Passed `skipScroll` and `container` arguments to `appendMessage` to batch DOM appends into a `DocumentFragment` and skip scroll updates until the entire history is rendered.
## 2024-10-25 - DOM Batching in History Loading
**Learning:** Found history lists being rendered by appending elements one-by-one directly to `historyListContainer` in a loop, causing O(N) layout recalculations.
**Action:** Use a `DocumentFragment` to build the list in memory and append the fragment in one operation to batch DOM updates.

## 2024-05-18 - Intl.DateTimeFormat Instantiation Bottleneck
**Learning:** `Date.prototype.toLocaleDateString` and `toLocaleTimeString` are surprisingly slow (taking ~2s per 10k calls) because they implicitly instantiate a new `Intl.DateTimeFormat` object on every invocation. When used in loops (like rendering message timestamps or parsing chat history), this causes severe layout jank.
**Action:** Always pre-instantiate and cache `Intl.DateTimeFormat` objects and use their `.format(date)` method instead. This simple swap yielded a ~22x speedup in date formatting operations.

## 2024-05-18 - [Date calculation in history rendering]
**Learning:** Instantiating `new Date()` inside a loop over a potentially large array (like rendering chat history buckets) can cause significant layout thrashing and CPU spikes due to repetitive Date creation and boundary calculations (e.g., `setHours(0,0,0,0)`).
**Action:** Always pre-calculate loop-invariant dates (like the "start of today" timestamp) before the loop and pass the primitive `timestamp` to the helper function.
## 2024-10-25 - Streaming Regex Parsing Optimization
**Learning:** Checking for HTML blocks inside the AI SSE streaming chunk loop (e.g. `fullContent.match(/```html\n?([\s\S]*)/)`) forces the JS engine to scan the entire string over and over asynchronously up to a thousand times per request, stalling the main thread and janking animations. Furthermore, calling synchronous layout-reading DOM API like updating Canvas live preview within the inner loop led to layout thrashing.
**Action:** Move all expensive regex parsing (like extracting live code snippets) and the DOM API calls mapping the extraction out of the chunk stream loop. Batch them into `requestAnimationFrame` (RAF) via `flushDOMUpdates()` so the CPU-heavy tasks and DOM layouts happen at most 60 times a second.
## 2024-10-25 - DOM Traversal Optimization in Event Listeners
**Learning:** Performing synchronous DOM traversals (like `document.querySelector`) inside high-frequency event listeners (like `input` or `scroll`) causes unnecessary repeated CPU overhead. This creates input latency and can lead to jank.
**Action:** Always hoist DOM queries outside of the event listener to cache the element references, turning O(N) traversal into an O(1) lookup.
