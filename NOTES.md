# Cocos Creator Plugin Development Notes

## 1. Asset & Prefab Management

### 1.1 Do Not Manually Intervene with `.meta` Files

- **Problem**: Manually creating or modifying files ending in `.meta` using `manage_asset` will cause editor errors (e.g., `Invalid assetpath, must not use .meta as suffix`).
- **Cause**: The Cocos Creator asset database automatically detects new assets and generates corresponding `.meta` files. External interference breaks asset index consistency.
- **Best Practice**: Always use high-level asset tools (like `prefab_management`) to handle assets, allowing the editor to manage `.meta` files itself.

### 1.2 Correct Prefab Creation Workflow

- **Recommended Tool**: Use the `create` operation of `prefab_management`.
- **Core Logic**: This tool synchronously handles node serialization, database path mapping, and asset refreshing, ensuring the atomic generation of the prefab and its accompanying `.meta` file.

---

## 2. Script Property (Inspector) Association

### 2.1 Difference Between Path and UUID

- **Problem**: In the Inspector panel, associating script properties (like `cc.Prefab` or `cc.Node`) via paths often results in a "Type Error".
- **Cause**: The editor Inspector expects engine-internal object references. While `db://` paths point to files, the editor cannot automatically convert string paths into class instances at the property assignment level.
- **Best Practice**:
    1.  Obtain the asset's **UUID** via `manage_asset -> get_info`.
    2.  When calling `manage_components -> update`, **prioritize using UUID** for assignment.
    3.  The UUID is the underlying unique identifier, ensuring the editor precisely recognizes the asset type and completes reference linking correctly.

---

## 3. Scene Synchronization & Editor State

### 3.1 Refresh & Compilation

- **Note**: After creating a new script, you must call `manage_editor -> refresh_editor` or wait a few seconds to trigger compilation.
- **Risk**: Attempting to add a script as a component to a node before compilation completes will result in errors like `Failed to add component: Cannot read property 'constructor' of null` or the script component not being found.

### 3.2 Node Deletion (IPC Protocol)

- **Correct Protocol**: Use `Editor.Ipc.sendToMain('scene:delete-nodes', uuid_or_array)`.
- **Note**: Do not use `scene:delete-selected`, as it may not accept parameters or might behave inconsistently across different editor versions.
- **Tip**: In `mcp-bridge`, calling `execute_menu_item("delete-node:NODE_UUID")` uses a direct scene script deletion, while `execute_menu_item("Delete")` goes through the main process's `scene:delete-nodes` and automatically picks up selected nodes.

---

## 4. MCP Bridge Source Patch Notes

### 4.1 Property Parsing Enhancement (Asset Serialization Fix)

- **Improvement**: Used `cc.AssetLibrary.loadAsset` in `applyProperties` within `scene-script.js` to solve "Type Error" issues for asset properties in the Inspector.
- **Principle**:
    - **Root Cause**: Assigning a UUID string directly to an asset property (e.g., `comp.prefab = "uuid"`) in the scene process causes the `.fire` file to save it as a string instead of an object. The Inspector expects a reference structure `{ "__uuid__": "..." }`.
    - **Fix Logic**:
        1.  **Real Loading**: Use `cc.AssetLibrary.loadAsset(uuid, callback)` to asynchronously load the actual asset instance.
        2.  **Instance Assignment**: Assign the loaded `asset` object to the component (`component[key] = asset`), ensuring correct serialization upon saving.
        3.  **UI Sync**: Send the `scene:set-property` IPC with the `{ uuid: value }` format to notify the Inspector UI to update.
    - **Node/Component**: For node or component references, find the instance and assign the instance object directly rather than the UUID string.

---

## 5. AI Operation Safety Rules (Subject Validation Rule)

### 5.1 Certainty First

- **Core Rule**: Any operation on nodes, components, or properties must be based on the **"confirmed existence of the subject"**.
- **Process**:
    1.  **Node Validation**: Call `get_scene_hierarchy` to confirm the `nodeId` before operation.
    2.  **Component Validation**: Call `manage_components(action: 'get')` to confirm the target component exists before `update` or `remove`.
    3.  **Property Validation**: Never guess property names. Confirm accurate names via script definitions or the existing property list returned by `get`.
- **Prohibited Behavior**: No blind assignments or deletions based on assumptions. If an object doesn't exist, report an error or attempt to rebuild; do not proceed with modification attempts.

---

## 6. Common Asset Keywords

- **Asset Recognition Heuristics**: When assigning via `manage_components`, the plugin treats properties containing these keywords as UUID assets:
  `prefab`, `sprite`, `texture`, `material`, `skeleton`, `spine`, `atlas`, `font`, `audio`, `data`
- **Advice**: If an asset fails to load, check if the property name includes these keywords or manually verify that the UUID doesn't belong to a node.

---

## 7. Search Tool (`search_project`) Recommendations

- **Performance**: Specify the `path` parameter to narrow the search scope (e.g., `db://assets/scripts`). Avoid full-project searches, especially in the `Library` directory.
- **Regex**: Ensure correct regex syntax when using `useRegex`.
- **Mode Selection**:
    - For logic code: Use `matchType: "content"`.
    - For asset files: Use `matchType: "file_name"` with `extensions` filter.
    - Before refactoring: Use `matchType: "dir_name"` to check for directory name conflicts.

---

## 8. Undo/Redo Guide

### 8.1 Transaction Grouping

- **Context**: When executing multiple changes (e.g., move and scale), it's best if a single "Undo" reverts all changes.
- **Best Practice**:
    1.  Call `manage_undo(action: 'begin_group', description: 'Operation Description')`.
    2.  Execute changes (e.g., `update_node_transform`).
    3.  Call `manage_undo(action: 'end_group')`.
- **Note**: `undo-record` must explicitly associate with a node ID during `begin_group`.

### 8.2 Property Modification Method

- **Rule**: Never use direct assignment like `node.x = 100` in `scene-script.js`.
- **Correct Way**: Use `Editor.Ipc.sendToPanel('scene', 'scene:set-property', ...)` to ensure the modification is captured by Cocos Creator's UndoManager.

---

## 9. Concurrency Safety & Anti-freeze Mechanism

### 9.1 Command Queue (CommandQueue)

- **Context**: Rapid-fire MCP requests can cause I/O and IPC channel conflicts, freezing the editor.
- **Solution**: Introduced an `enqueueCommand` mechanism to **serialize** all MCP tool calls. A request is only processed after the previous one completes.
- **Notes**:
    1.  Deadlock protection ensures that one failed command doesn't block the entire queue.
    2.  `batchExecute` also executes sub-operations serially.
    3.  Queue length is shown in logs (`REQ -> [toolName] (Queue Length: N)`).

### 9.2 `refresh_editor` Path Requirement

- **⚠️ Path Required**: Always specify a precise file or directory path in `properties.path` when calling `refresh_editor`. Avoid global refreshes in production projects.

### 9.3 IPC Timeout Protection (`callSceneScriptWithTimeout`)

- **Context**: `callSceneScript` can hang if the Scene panel is blocked.
- **Solution**: All calls now use `callSceneScriptWithTimeout` with a 15-second default timeout.
- **Log ID**: Timeouts are logged as `[Timeout] callSceneScript "methodName" timed out after 15000ms`.
