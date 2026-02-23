# MCP Bridge Plugin

This is an MCP (Model Context Protocol) bridge plugin designed for Cocos Creator, used to connect external AI tools with the Cocos Creator editor, enabling automated operations on scenes, nodes, and other assets.

## Supported Versions

This plugin is designed for Cocos Creator 2.4.x versions. Due to the use of specific editor APIs, it may not be compatible with newer or older versions.

## Features

- **HTTP Service Interface**: Provides a standard HTTP interface for external tools to call Cocos Creator editor functions via the MCP protocol.
- **Scene Node Operations**: Get, create, and modify nodes in the scene.
- **Resource Management**: Create scenes and prefabs, open scenes or prefabs in edit mode.
- **Component Management**: Add, remove, and get entry for node components.
- **Script Management**: Create, delete, read, and write script files.
- **Batch Execution**: Execute multiple MCP tool operations in batch to improve efficiency.
- **Asset Management**: Create, delete, move, and get asset information.
- **Real-time Logs**: Detailed operation log recording and display.
- **Auto-start**: Supports automatic service startup when the editor opens.
- **Editor Management**: Get and set selected objects, refresh the editor.
- **GameObject Search**: Find nodes in the scene based on various conditions.
- **Material Management**: Create and manage material assets.
- **Texture Management**: Create and manage texture assets.
- **Menu Item Execution**: Execute Cocos Creator editor menu items.
- **Code Editing Enhancements**: Apply text editing operations to files.
- **Console Reading**: Read editor console output.
- **Script Validation**: Validate script syntax correctness.
- **Global Search**: Search text content within the project.
- **Undo/Redo**: Manage the editor's undo stack.
- **VFX Management**: Create and modify particle systems.
- **Tool Descriptions**: The test panel provides detailed descriptions and parameter explanations for all tools.

## Installation & Usage

### Installation

Copy this plugin into the `packages` directory of your Cocos Creator project.

### Startup

1. Open the Cocos Creator editor.
2. In the menu bar, select `MCP Bridge/Open Panel` to open the test panel.
3. Click the "Start" button in the panel to begin the service.
4. The service runs on port 3456 by default.

### Configuration Options

- **Port**: You can customize the HTTP service port (default: 3456).
- **Auto-start**: The service can be set to start automatically when the editor opens.
- **Multi-instance Support**: If the default port (3456) is occupied, the plugin will automatically try port+1 (e.g., 3457) until an available port is found.
- **Configuration Isolation**: Plugin configurations (auto-start, last used port) are stored in the project directory (`settings/mcp-bridge.json`), ensuring separate settings for different projects.

## Connecting to AI Editors

### Configuration in AI Editors (e.g., Cursor / VS Code)

If your AI editor provides a "Type: command" or "Stdio" option:

```
Command: node
Args: [Absolute path to Cocos Creator project]/packages/mcp-bridge/mcp-proxy.js
```

For example:

```
Args: C:/Projects/MyGame/packages/mcp-bridge/mcp-proxy.js
```

### Or add JSON Configuration:

```json
{
	"mcpServers": {
		"cocos-creator": {
			"command": "node",
			"args": ["[Absolute path to Cocos Creator project]/packages/mcp-bridge/mcp-proxy.js"]
		}
	}
}
```

Note: Replace the path above with the actual absolute path to the `mcp-proxy.js` file in your project.

## API Interfaces

The service provides the following MCP tool interfaces:

### 1. get_selected_node

- **Description**: Gets the currently selected node's UUID.
- **Parameters**: None.

### 2. set_node_name

- **Description**: Modifies the name of a specified node.
- **Parameters**:
    - `id`: The UUID of the node.
    - `newName`: The new name for the node.

### 3. save_scene

- **Description**: Saves modifications to the current scene.
- **Parameters**: None.

### 4. get_scene_hierarchy

- **Description**: Gets the full node tree structure of the current scene (including UUID, name, and hierarchy).
- **Parameters**: None.

### 5. update_node_transform

- **Description**: Modifies coordinates, scale, or color of a node.
- **Parameters**:
    - `id`: Node UUID.
    - `x`, `y`: Coordinates.
    - `width`, `height`: Node dimensions.
    - `scaleX`, `scaleY`: Scale values.
    - `color`: HEX color code (e.g., #FF0000).
- **Important**: You must call `get_scene_hierarchy` before execution to ensure the `id` is valid.

### 6. open_scene

- **Description**: Opens a specified scene file. This is an asynchronous and time-consuming operation. **Important**: For new or empty scenes, ensure basic nodes (Canvas/Camera) are initialized.
- **Parameters**:
    - `url`: Scene asset path, e.g., `db://assets/NewScene.fire`.

### 7. open_prefab

- **Description**: Opens a specified prefab file in edit mode.
- **Parameters**:
    - `url`: Prefab asset path, e.g., `db://assets/prefabs/Test.prefab`.

### 8. create_node

- **Description**: Creates a new node in the current scene.
- **Important**:
    1. If `parentId` is specified, verify its existence via `get_scene_hierarchy` first.
    2. **Preset Types**:
        - `empty`: Pure empty node.
        - `sprite`: Automatically adds a Sprite component with a default placeholder texture.
        - `button`: Adds Sprite and Button components.
        - `label`: Adds a Label component.
- **Parameters**:
    - `name`: Node name.
    - `parentId`: Parent node UUID (optional).
    - `type`: Node preset type (`empty`, `sprite`, `label`, `button`).

### 8. manage_components

- **Description**: Manages node components.
- **Best Practices**:
    1. **Verification**: Call `get_scene_hierarchy` to confirm the `nodeId` exists.
    2. Check for existing components via `get` before performing an `add`.
- **Parameters**:
    - `nodeId`: Node UUID.
    - `action`: Operation type (`add`, `remove`, `get`, `update`).
    - `componentType`: Component type, e.g., `cc.Sprite`.
    - `componentId`: Component ID (for `remove`/`update`).
    - `properties`: Component properties.
- **Intelligent Features**:
    1. Auto-resolves component types if a node UUID is passed for a property expecting a component.
    2. Handles async loading and serialization for asset-type properties (e.g., `cc.Prefab`).
    3. Supports UUID arrays for properties like `materials`.

### 9. manage_script

- **Description**: Manages script files (default: TypeScript).
- **Parameters**:
    - `action`: Operation type (`create`, `delete`, `read`, `write`).
    - `path`: Script path, e.g., `db://assets/scripts/NewScript.ts`.
    - `content`: Script content.
    - `name`: Script name.

### 10. batch_execute

- **Description**: Executes multiple operations in a single call.
- **Parameters**:
    - `operations`: List of operations (comprising `tool` and `params`).

### 11. manage_asset

- **Description**: Manages assets.
- **Parameters**:
    - `action`: `create`, `delete`, `move`, `get_info`.
    - `path`: Asset path.
    - `targetPath`: Target path (for `move`).
    - `content`: Asset content.

### 12. scene_management

- **Description**: Manages scenes.
- **Parameters**:
    - `action`: `create`, `delete`, `duplicate`, `get_info`.
    - `path`: Scene path.
    - `targetPath`: Target path (for `duplicate`).
    - `name`: Scene name.

### 13. prefab_management

- **Description**: Manages prefabs.
- **Parameters**:
    - `action`: `create`, `update`, `instantiate`, `get_info`.
    - `path`: Prefab path.
    - `nodeId`: Node ID (for `create`/`update`).
    - `parentId`: Parent node ID (for `instantiate`).

### 14. manage_editor

- **Description**: Manages editor states.
- **Parameters**:
    - `action`: `get_selection`, `set_selection`, `refresh_editor`.
    - `target`: `node` or `asset`.
    - `properties`: Object defining `nodes` or `assets` arrays.

### 15. find_gameobjects

- **Description**: Finds game objects based on conditions.
- **Parameters**:
    - `conditions`: Search parameters (`name`, `tag`, `component`, `active`).
    - `recursive`: Boolean (default: true).

### 16. manage_material

- **Description**: Manages material assets.
- **Parameters**:
    - `action`: `create`, `delete`, `update`, `get_info`.
    - `path`: Material path.
    - `properties`: Material properties (`shaderUuid`, `defines`, `uniforms`).

### 17. manage_shader

- **Description**: Manages shader (Effect) assets.
- **Parameters**:
    - `action`: `create`, `read`, `write`, `delete`, `get_info`.
    - `path`: Shader path.
    - `content`: Text content.

### 18. manage_texture

- **Description**: Manages texture assets.
- **Parameters**:
    - `action`: `create`, `delete`, `get_info`, `update`.
    - `path`: Texture path.
    - `properties`: Texture properties (`type`, `border`, `width`, `height`, etc.).

### 18. execute_menu_item

- **Description**: Executes editor menu items.
- **Parameters**:
    - `menuPath`: The path to the menu item.
    - Supports `delete-node:${UUID}` for direct node deletion.

### 19. apply_text_edits

- **Description**: Applies text edits to files.
- **Parameters**:
    - `filePath`: Target file path.
    - `edits`: List of edits (`insert`, `delete`, `replace`).

### 20. read_console

- **Description**: Reads the editor console output.
- **Parameters**:
    - `limit`: Output limit (optional).
    - `type`: Output type (`log`, `error`, `warn`).

### 21. validate_script

- **Description**: Validates script syntax.
- **Parameters**:
    - `filePath`: Script path.

### 22. search_project

- **Description**: Searches project files (supports regex, filename, and directory search).
- **Parameters**:
    - `query`: Keyword or regex string.
    - `useRegex`: Boolean (default: false).
    - `path`: Search scope.
    - `matchType`: `content`, `file_name`, or `dir_name`.

### 23. manage_undo

- **Description**: Manages undo/redo operations.
- **Parameters**:
    - `action`: `undo`, `redo`, `begin_group`, `end_group`, `cancel_group`.
    - `description`: Group description.

### 24. manage_vfx

- **Description**: Manages particle systems (VFX).
- **Parameters**:
    - `action`: `create`, `update`, `get_info`.
    - `nodeId`: Target node UUID.
    - `properties`: Particle parameters.

### 25. manage_animation

- **Description**: Manages node animation components.
- **Parameters**:
    - `action`: `get_list`, `get_info`, `play`, `stop`, `pause`, `resume`.
    - `nodeId`: Target node UUID.

### 26. get_sha

- **Description**: Gets the SHA-256 hash of a file.
- **Parameters**:
    - `path`: File path.

## Technical Implementation

### Architecture Design

The plugin uses the standard Cocos Creator extension architecture:

- **main.js**: Main entry point, starts the HTTP server and handles MCP requests.
- **scene-script.js**: Executes operations within the scene context.
- **panel/**: Provides the graphical user interface.

### HTTP Service

The plugin hosts an internal HTTP server with two main endpoints:

- `GET /list-tools`: Returns available MCP tool definitions.
- `POST /call-tool`: Executes a specific tool operation.

### Data Flow

1. External tool sends an MCP request to the HTTP interface.
2. `main.js` receives the request and parses parameters.
3. Request is forwarded to `scene-script.js` via `Editor.Scene.callSceneScript`.
4. `scene-script.js` executes the operation in the scene thread.
5. Results are returned back to the external tool.

## AI Operation Safety Rules (Subject Validation Rule)

To ensure stability, AI must follow these rules when using the plugin:

1.  **Certainty First**: All operations must be based on confirmed object existence.
2.  **Validation Process**:
    - **Node Validation**: Always use `get_scene_hierarchy` to confirm nodes.
    - **Component Validation**: Use `manage_components(get)` to verify components.
    - **Property Validation**: Confirm property names before updates.
3.  **No Assumptions**: Never attempt to modify non-existent objects or properties.

## License

GNU AFFERO GENERAL PUBLIC LICENSE
Version 3, 19 November 2007

Anyone is permitted to obtain, use, modify, and distribute this software, provided they comply with the following:

1. Distributed versions must release source code under the same license.
2. Services provided over a network must also provide source code to users.
3. Any derivative works must adhere to the same license terms.
