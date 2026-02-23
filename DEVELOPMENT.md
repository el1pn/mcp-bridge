# MCP Bridge Plugin Development Process Documentation

This document records the complete development process of the MCP Bridge plugin, including core architecture design, functional implementation, testing, and debugging stages.

## 0. Project Development Rules

> [!IMPORTANT]
> All contributors must strictly adhere to the following rules:

1.  **Language & Communication**: All comments, documentation, plans, tasks, and commits must use **English**. AI responses to the user should use **Vietnamese** (as per global user rules).
2.  **Technology Stack**: New scripts must use **TypeScript** (`.ts`). Creating new `.js` files is prohibited (except for build scripts or test configurations).
3.  **Documentation**: All modified or created scripts must include detailed JSDoc format comments.
4.  **Architecture**: Introducing new architectural patterns or heavy external libraries is strictly prohibited. Existing Cocos Creator managers and utility classes must be reused.
5.  **Isolation Principle**: Maintain strict separation of responsibilities between `main.js` (Main Process) and `scene-script.js` (Renderer Process). Even in the main process, interactions with scene scripts should be done via IPC.
6.  **Subject Validation Rule**: Before any "write" operation on nodes, components, or properties, the AI must first verify the existence and type correctness of the subject. Operations based on assumptions are strictly prohibited.

## 1. Project Initialization

### 1.1 Directory Structure

```
mcp-bridge/
├── main.js              # Plugin main entry
├── scene-script.js      # Scene script
├── mcp-proxy.js         # MCP proxy
├── README.md            # Project description
├── DEVELOPMENT.md       # Development process documentation
├── package.json         # Plugin configuration
└── panel/               # Panel directory
    ├── index.html       # Panel interface
    └── index.js         # Panel logic
```

### 1.2 Plugin Configuration

Configure plugin information in `package.json`:

```json
{
	"name": "mcp-bridge",
	"version": "1.0.0",
	"description": "MCP Bridge for Cocos Creator",
	"main": "main.js",
	"panel": {
		"main": "panel/index.html",
		"type": "dockable",
		"title": "MCP Bridge",
		"width": 800,
		"height": 600
	},
	"contributions": {
		"menu": [
			{
				"path": "Packages/MCP Bridge",
				"label": "Open Test Panel",
				"message": "open-test-panel"
			}
		]
	}
}
```

## 2. Core Architecture Design

### 2.1 System Architecture

```
┌────────────────────┐     HTTP     ┌────────────────────┐     IPC     ┌────────────────────┐
│   External AI Tool  │  ──────────> │ main.js (HTTP Srv) │  ─────────> │  scene-script.js   │
│  (Cursor/VS Code)  │     <──────── │ (MCP Processing)   │     <──────── │ (Scene Operations) │
└────────────────────┘     JSON     └────────────────────┘     JSON     └────────────────────┘
```

### 2.2 Core Modules

1. **HTTP Service Module**: Handles external requests and parses MCP protocol.
2. **MCP Tool Module**: Implements various operation tools.
3. **Scene Operation Module**: Executes scene-related operations.
4. **Asset Management Module**: Handles scripts and asset files.
5. **Panel Interface Module**: Provides a user interaction interface.

## 3. Functional Module Implementation

### 3.1 HTTP Service Implementation

Implement the HTTP service in `main.js`:

```javascript
startServer(port) {
    try {
        const http = require('http');
        mcpServer = http.createServer((req, res) => {
            // Handle requests...
        });
        mcpServer.listen(port, () => {
            addLog("success", `MCP Server running at http://127.0.0.1:${port}`);
        });
    } catch (e) {
        addLog("error", `Failed to start server: ${e.message}`);
    }
}
```

## 4. Development History & Milestones

### 2026-02-10: Deep Fix & Standardization of Undo System

- **Issue Analysis**: Fixed `TypeError: Cannot read property '_name' of null`. This error was caused by mixing direct node property modification (bypassing the Undo system) with grouped transactions.
- **Refactoring Key**: Replaced all direct assignments in `update-node-transform` with `scene:set-property` IPC calls, ensuring all transform modifications are monitored by the Undo system.
- **Flaw Correction**: Fixed an issue where `manage_undo` passed incorrect parameters during `begin_group`, leading to "Unknown object to record".
- **Documentation Sync**: Updated `README.md`, `DEVELOPMENT.md`, and `NOTES.md`.

### 2026-02-13: New `open_prefab` Feature & Message Protocol Correction

- **Requirement Implementation**: Added the `open_prefab` tool, allowing AI to open prefabs directly into edit mode.
- **Protocol Correction**: Through testing, it was confirmed that using the `scene:enter-prefab-edit-mode` message in conjunction with `Editor.Ipc.sendToAll` is the best solution for entering prefab edit mode, resolving the issue where `scene:open-by-uuid` failed to trigger the edit state.
- **Documentation Completion**: Synchronized all technical documents to ensure full coverage and detailed JSDoc comments.

### 3.2 MCP Tool Registration

Register tools in the `/list-tools` interface:

```javascript
const tools = [
	{
		name: "get_selected_node",
		description: "Get the currently selected node",
		parameters: [],
	},
	// Other tools...
];
```

### 3.3 Scene Operation Implementation

Implement scene-related operations in `scene-script.js`:

```javascript
const sceneScript = {
	"create-node"(params, callback) {
		// Create node logic...
	},
	"set-property"(params, callback) {
		// Set property logic...
	},
	// Other operations...
};
```

### 3.4 Script Management Implementation

Implement script management in `main.js`:

```javascript
manageScript(args, callback) {
    const { action, path, content } = args;
    switch (action) {
        case "create":
            // Ensure parent directory exists
            const fs = require('fs');
            const pathModule = require('path');
            const absolutePath = Editor.assetdb.urlToFspath(path);
            const dirPath = pathModule.dirname(absolutePath);
            if (!fs.existsSync(dirPath)) {
                fs.mkdirSync(dirPath, { recursive: true });
            }
            // Create TypeScript script
            Editor.assetdb.create(path, content || `const { ccclass, property } = cc._decorator;

@ccclass
export default class NewScript extends cc.Component {
    // LIFE-CYCLE CALLBACKS:

    onLoad () {}

    start () {}

    update (dt) {}
}`, (err) => {
                callback(err, err ? null : `Script created at ${path}`);
            });
            break;
        // Other operations...
    }
}
```

### 3.5 Batch Execution Implementation

Implement batch execution in `main.js`:

```javascript
batchExecute(args, callback) {
    const { operations } = args;
    const results = [];
    let completed = 0;

    if (!operations || operations.length === 0) {
        return callback("No operations provided");
    }

    operations.forEach((operation, index) => {
        this.handleMcpCall(operation.tool, operation.params, (err, result) => {
            results[index] = { tool: operation.tool, error: err, result: result };
            completed++;

            if (completed === operations.length) {
                callback(null, results);
            }
        });
    });
}
```

### 3.6 Asset Management Implementation

Implement asset management in `main.js`:

```javascript
manageAsset(args, callback) {
    const { action, path, targetPath, content } = args;

    switch (action) {
        case "create":
            // Ensure parent directory exists
            const fs = require('fs');
            const pathModule = require('path');
            const absolutePath = Editor.assetdb.urlToFspath(path);
            const dirPath = pathModule.dirname(absolutePath);
            if (!fs.existsSync(dirPath)) {
                fs.mkdirSync(dirPath, { recursive: true });
            }
            Editor.assetdb.create(path, content || '', (err) => {
                callback(err, err ? null : `Asset created at ${path}`);
            });
            break;
        // Other operations...
    }
}
```

### 3.7 Panel Interface Implementation

Implement the tabbed interface in `panel/index.html`:

```html
<div class="mcp-container">
	<!-- Tabs -->
	<div class="tabs">
		<ui-button id="tabMain" class="tab-button active">Main</ui-button>
		<ui-button id="tabTest" class="tab-button">Tool Test</ui-button>
	</div>

	<!-- Main Panel Content -->
	<div id="panelMain" class="tab-content active">
		<!-- Content... -->
	</div>

	<!-- Test Panel Content -->
	<div id="panelTest" class="tab-content">
		<div class="test-container">
			<div class="test-layout">
				<!-- Left Tool List -->
				<div class="left-panel">
					<!-- Tool list... -->
				</div>

				<!-- Right Input/Output -->
				<div class="right-panel">
					<!-- Input/output areas... -->
				</div>
			</div>
		</div>
	</div>
</div>
```

## 4. Testing & Debugging

### 4.1 Local Testing

1. **Start Service**: Click the "Start" button in the panel.
2. **Test Tools**: Test various tools in the "Tool Test" tab.
3. **View Logs**: Check operation logs in the main panel.

### 4.2 Common Errors & Fixes

#### 4.2.1 Panel Loading Error

**Error**: `Panel info not found for panel mcp-bridge`

**Solution**:

- Check the panel configuration in `package.json`.
- Ensure the `panel` field is correctly configured and remove conflicting `panels` fields.

#### 4.2.2 Asset Creation Error

**Error**: `Parent path ... is not exists`

**Solution**:

- Add a directory check and creation logic before creating assets.
- Use `fs.mkdirSync(dirPath, { recursive: true })` to recursively create directories.

#### 4.2.3 Script Syntax Error

**Error**: `SyntaxError: Invalid or unexpected token`

**Solution**:

- Use template literals (backticks) for multi-line strings.
- Avoid variable name conflicts.

### 4.3 Performance Optimization

1. **Batch Execution**: Use the `batch_execute` tool to reduce HTTP requests.
2. **Async Operations**: Use callback functions to handle async operations and avoid blocking the main thread.
3. **Error Handling**: Enhance error handling mechanisms to improve plugin stability.

## 5. Documentation Writing

### 5.1 README.md

- Project Introduction
- Features
- Installation & Usage
- API Documentation
- Technical Implementation

### 5.2 API Documentation

Detailed API documentation for each MCP tool, including:

- Tool Name
- Functional Description
- Parameter Description
- Return Value Format
- Usage Example

### 5.3 Development Documentation

- Project Architecture
- Development Process
- Code Standards
- Contribution Guidelines

## 6. Deployment & Usage

### 6.1 Deployment

1. **Local Deployment**: Copy the plugin to the `packages` directory of the Cocos Creator project.
2. **Remote Deployment**: Manage plugin code through a version control system.

### 6.2 Usage Workflow

1. **Start Service**:
    - Open Cocos Creator editor.
    - Select `Packages/MCP Bridge/Open Test Panel`.
    - Click "Start" to begin the service.

2. **Connect AI Editor**:
    - Configure the MCP proxy in the AI editor.
    - Use `node [project path]/packages/mcp-bridge/mcp-proxy.js` as the command.

3. **Execute Operations**:
    - Send MCP requests through the AI editor.
    - Or test tools directly in the test panel.

### 6.3 Configuration Options

- **Port Settings**: Default 3456, customizable.
- **Auto-start**: Supports automatic service startup when the editor opens.

## 7. Functional Extension

### 7.1 Adding New Tools

1. **Register Tool in `main.js`**:
    - Add tool definition in `/list-tools` response.
    - Add handling logic in the `handleMcpCall` function.

2. **Add Example in Panel**:
    - Add tool example parameters in `panel/index.js`.
    - Update the tool list.

3. **Update Documentation**:
    - Add tool documentation in `README.md`.
    - Update the feature list.

### 7.2 Integrating New APIs

1. **Understand Cocos Creator API**:
    - Check Cocos Creator Editor API documentation.
    - Understand Scene Script API.

2. **Implementation**:
    - Add corresponding functions in `main.js` or `scene-script.js`.
    - Handle async operations and errors.

3. **Verification**:
    - Write test cases.
    - Verify functional correctness.

## 8. Version Management

### 8.1 Version Control

- Use Git for version control.
- Follow semantic versioning standards.

### 8.2 Release Process

1. **Code Review**: Check code quality and functional completeness.
2. **Testing**: Ensure all functions work as expected.
3. **Doc Update**: Update README and related documents.
4. **Release**: Tag the version and publish.

## 9. Technology Stack

- **JavaScript**: Primary development language.
- **Node.js**: HTTP service and file operations.
- **Cocos Creator API**: Editor function integration.
- **HTML/CSS**: Panel interface.
- **MCP Protocol**: Communication with AI tools.

## 10. Best Practices

1. **Code Organization**:
    - Modular design, separation of responsibilities.
    - Use callback functions effectively for async operations.

2. **Error Handling**:
    - Comprehensive error catching and handling.
    - Detailed error logging.

3. **User Experience**:
    - Intuitive panel interface.
    - Real-time operation feedback.
    - Detailed logging info.

4. **Security**:
    - Validate input parameters.
    - Prevent path traversal attacks.
    - Restrict service access scope.

## 11. Roadmap

### 11.1 Phase 3 Development (Completed)

| Task | Status | Description |
| ---- | ------ | ----------- |
| Editor Management Tools | ✅ Done | Implement `manage_editor` for editor state control. |
| GameObject Search Tools | ✅ Done | Implement `find_gameobjects` to find scene nodes. |
| Material & Texture Tools | ✅ Done | Implement `manage_material` and `manage_texture`. |
| Menu Item Execution | ✅ Done | Implement `execute_menu_item`. |
| Code Editing Enhancements | ✅ Done | Implement `apply_text_edits`. |
| Console Reading | ✅ Done | Implement `read_console`. |
| Script Validation | ✅ Done | Implement `validate_script`. |

### 11.2 Phase 4 Development (Completed)

| Task | Status | Description |
| ---- | ------ | ----------- |
| Testing Functions | ✅ Done | Implement `run_tests.js` for automated test cases. |
| Error Handling Enhancements | ✅ Done | Improve error logging for HTTP and tools. |

### 11.3 Gap Filling Phase (Completed)

| Task | Status | Description |
| ---- | ------ | ----------- |
| VFX Management | ✅ Done | Implement `manage_vfx` for particle system management. |
| File Hashing | ✅ Done | Implement `get_sha` for SHA-256 calculation. |
| Animation Management | ✅ Done | Implement `manage_animation` for animation control. |

### 11.4 Phase 6: Reliability & Experience Optimization (Completed)

| Task | Status | Description |
| ---- | ------ | ----------- |
| IPC Tool Enhancements | ✅ Done | Fix IpcManager return value parsing, optimize panel. |
| Script Reliability Fixes | ✅ Done | Resolve compilation sequence issues (doc guidance + refresh). |
| Component Parsing Fixes | ✅ Done | Fix UUID conversion for component properties. |

### 11.5 Phase 7 Development (Completed)

| Task | Status | Description |
| ---- | ------ | ----------- |
| Plugin Release | ✅ Done | Prepare for release to Cocos Store. |
| Documentation Completion | ✅ Done | Finish API docs and "Getting Started" tutorial. |
| UI Polishing | ✅ Done | Optimize panel UI experience. |
| Internationalization | ✅ Done | Add multi-language support (i18n). |
| Tool Expansion | ✅ Done | Add more advanced tools. |

## 12. Unity-MCP Comparison

Cocos-MCP has implemented almost all core features found in Unity-MCP.

| Category | Unity-MCP Feature | Cocos-MCP Status | Notes |
| -------- | ----------------- | ---------------- | ----- |
| Editor Mgmt | manage_editor | ✅ Implemented | |
| GameObject Mgmt | find_gameobjects | ✅ Implemented | |
| Material Mgmt | manage_material | ✅ Implemented | |
| Texture Mgmt | manage_texture | ✅ Implemented | |
| Code Editing | apply_text_edits | ✅ Implemented | |
| Global Search | search_project | ✅ Implemented | Enhanced version. |
| Console | read_console | ✅ Implemented | |
| Menu Execution| execute_menu_item | ✅ Implemented | Recommended delete-node:UUID. |
| Script Validation| validate_script | ✅ Implemented | |
| Undo/Redo | undo/redo | ✅ Implemented | |
| VFX Management | manage_vfx | ✅ Implemented | |
| Git Integration | get_sha | ✅ Implemented | |
| Animation Mgmt | manage_animation | ✅ Implemented | |
| ScriptableObject| manage_so | ❌ N/A | Use AssetDB instead. |

## 13. Risk Assessment

| Risk | Impact | Mitigation |
| ---- | ------ | ---------- |
| Editor API Changes | Plugin failure | Regularly check Cocos updates & adapt. |
| Performance Issues | Slow response | Optimize `batch_execute`, reduce IPC. |
| Security | Unauthorized access| (Planned) IP Whitelist / Token Auth. |
| Compatibility | Version mismatch | Test 2.4.x, provide compatibility layer. |

## 14. Conclusion

The Cocos-MCP development plan has successfully completed multiple iterations. The plugin now features a full suite of core functions and reliability improvements. Future focus will shift towards **release preparation**, **documentation development**, and **user experience optimization**.
