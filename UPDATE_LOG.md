# Cocos Creator MCP Bridge Update & Fix Log

This document records all functional updates, performance improvements, and critical bug fixes during the development cycle.

## I. New Features & Tools

### 1. `manage_shader` Tool (New)

- **Function**: Implements full lifecycle management for shader (`.effect`) assets.
- **Operations**: Supports `create` (with default template), `read`, `write`, `delete`, and `get_info`.
- **Significance**: Completes the asset management chain, allowing the workflow from code writing to material application to be fully driven by MCP.

### 2. Material Management Enhancement (`manage_material`)

- **2.4.x Deep Adaptation**: Completely refactored material storage structure to support Cocos Creator 2.4.x `_effectAsset` and `_techniqueData` formats.
- **New `update` Operation**: Supports incremental updates of material macro definitions (`defines`) and Uniform parameters (`props`) without overwriting the entire file.

### 3. Component Management Enhancement (`manage_components`)

- **Asset Array Support**: Overcame the challenge of assigning array properties like `materials` via UUIDs.
- **Intelligent Async Loading**: Implemented logic for concurrent loading of multiple asset UUIDs, automatically synchronizing to scene nodes upon completion.

---

## II. Critical Bug Fixes (Technical Post-mortem)

### 1. Material Displaying as Empty in Inspector

- **Cause**: Initial code used incorrect JSON fields (e.g., `effects`), which did not comply with 2.4.x private property serialization standards.
- **Fix**: Changed fields to `_effectAsset` (UUID reference) and `_techniqueData` (containing `props` and `defines`).

### 2. Sprite Material Assignment Failure

- **Cause**: Directly assigning a string array to `cc.Sprite.materials` caused internal engine type mismatches; also, modifying memory properties directly does not trigger editor UI refresh.
- **Fix**: Intercepted array-type asset assignments in `scene-script.js`, loaded asset objects via `cc.AssetLibrary` first, and then used the `scene:set-property` IPC message to force refresh the editor Inspector panel.

### 3. Scene Cloning & `Editor.assetdb` Compatibility

- **Cause**: The main process `Editor.assetdb` in Cocos 2.4.x lacks the `loadAny` method, causing the original `duplicate` logic to crash.
- **Fix**: Switched to using the Node.js native `fs` module to read the source file stream directly and create new assets.

---

## III. Documentation & Standardization

### 1. Full Localization (English)

- **Code Comments**: Converted all critical logic comments in `main.js` and `scene-script.js` to English.
- **JSDoc Additions**: Added detailed JSDoc parameter descriptions for core functions to improve code readability.
- **Log Output**: All console logs (`addLog`) and error prompts have been converted to English for easier troubleshooting.

### 2. AI Safety Rules

- **Rule Injection**: Injected [AI Safety Rules] into all MCP tool descriptions, emphasizing principles like "verify before operation" and "assign assets via UUID".
- **Schema Optimization**: Optimized tool description texts to provide clearer guidance in AI clients (e.g., Cursor).

---

## IV. Texture & Transform Updates

### 1. `manage_texture` Tool Enhancement

- **New `update` Operation**: Supports modifying existing texture types (e.g., `texture` -> `sprite`) and 9-slice margins (`border`).
- **Meta Loading Robustness**: Fixed an issue where `Editor.assetdb.loadMeta` returned null in some cases, adding a fallback mechanism to read `.meta` files from the filesystem.
- **Multi-version Compatibility**: Implemented automatic compatibility for 9-slice data writing for different Cocos Creator versions' `.meta` file structures (array vs. independent fields).

### 2. `update_node_transform` Tool Enhancement

- **New Size Control**: Added `width` and `height` parameters, allowing AI to directly adjust node size (crucial for testing 9-slice stretching effects).

### 3. Critical Bug Fixes

- **Batch Property Application Interruption**: Fixed an issue in `scene-script.js` where the `applyProperties` function incorrectly used `return` when handling Asset type properties, causing subsequent properties (like `type`) to be ignored. It now uses `continue` to ensure all properties are applied correctly.

### 6.2 Menu Mapping Cleanup

- **Redundancy Removal**: Cleaned up outdated or unstable menu mappings in `execute_menu_item` (e.g., `File/Save`, `Edit/Delete`, etc.).
- **Standardized Operations**: Forced AI guidance to use `delete-node:UUID` or dedicated MCP tools (`save_scene`, `manage_undo`), improving the stability of automated workflows.

## VI. Summary

This update not only fixes material and asset synchronization bugs that hindered productivity but also greatly enhances the developer (and AI assistant) experience in the Cocos Creator 2.4.x environment by introducing `manage_shader` and comprehensive documentation localization. The cleanup of the menu execution tool further standardizes automated operation workflows, reducing potential instability.

---

## VII. Concurrency Safety & Anti-freeze Mechanism (2025-02-12)

### 1. Command Queue (CommandQueue) — Core Anti-freeze Refactor

- **Problem**: When an AI client sends consecutive rapid requests like `delete-node` → `refresh_editor` → `search_project`, multiple requests enter `handleMcpCall` concurrently. `AssetDB.refresh()` competes with subsequent operations for I/O and IPC channels, leading to main thread blockages and an unresponsive Scene panel.
- **Fix**: Introduced an `enqueueCommand` / `processNextCommand` queue mechanism at the HTTP `/call-tool` entry point. All MCP tool calls are forced to execute serially; the next command is only processed after the previous one's callback completes.
- **Exception Protection**: The queue includes deadlock protection in the `catch` block of `processNextCommand`, ensuring that even if a command throws an exception, it won't permanently block subsequent commands.
- **Observability**: Each request log displays `(Queue Length: N)` to help troubleshoot backlog issues.

### 2. IPC Timeout Protection (callSceneScriptWithTimeout)

- **Problem**: `Editor.Scene.callSceneScript` has no timeout mechanism. If the Scene panel is blocked, the callback never returns, causing both HTTP connections and the queue to pile up.
- **Fix**: Added a `callSceneScriptWithTimeout` unified wrapper function (default 15-second timeout), covering all 9 `callSceneScript` call points.
- **Timeout Log**: `[Timeout] callSceneScript "methodName" timed out after 15000ms`.

### 3. `batchExecute` Serialization

- **Problem**: The original implementation used `forEach` to dispatch all sub-operations in parallel, causing editor freezes when multiple `AssetDB` operations executed simultaneously.
- **Fix**: Changed to serial chain execution (`next()` recursive calls), ensuring each operation completes before the next one starts.

### 4. `refresh_editor` Path Parameter Optimization & Enhanced Warnings

- **Tool Schema Enhancement**: Added a red warning symbol (⚠️) and "Extremely Important" wording to the `manage_editor` tool description, explicitly requiring AI to specify a `path`.
- **AI Safety Rule #4**: Added a fourth rule to the global `globalPrecautions`, mandating that AI avoid refreshing global assets.
- **Result**: In production projects, refresh time dropped from **172 seconds** (default full refresh) to **19 seconds** (specified directory refresh).

### 5. Miscellaneous Fixes

- **Dead Code Cleanup**: Removed duplicate `res.writeHead / res.end` calls in the `/list-tools` route.
- **Document Update**: Added Chapter 9, "Concurrency Safety & Anti-freeze Mechanism," to `NOTES.md` to record the CommandQueue and IPC timeout mechanisms.

### 6. Scene & Prefab Tool Enhancements

- **New `open_prefab` Tool**: Resolved the issue of opening prefabs directly into edit mode. By using the correct IPC message `scene:enter-prefab-edit-mode` (combined with `Editor.Ipc.sendToAll`), AI can precisely control the prefab editing process instead of being limited to scene jumps.
- **Optimized Prefab Creation Stability (`create_node` + `prefab_management`)**:
    - Forced `Editor.assetdb.refresh` after creating physical directories to ensure immediate AssetDB synchronization.
    - Increased the safety delay between node renaming and prefab creation from 100ms to 300ms, eliminating race conditions where renaming wasn't completed before creation.
