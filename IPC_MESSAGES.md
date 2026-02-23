# Cocos Creator 2.4.12 IPC Messages Documentation

This document records all IPC (Inter-Process Communication) messages defined in Cocos Creator version 2.4.12, including their purposes, parameters, and return values.

## Table of Contents

- [App IPC Messages](#app-ipc-messages)
- [Asset-DB IPC Messages](#asset-db-ipc-messages)
- [Dashboard IPC Messages](#dashboard-ipc-messages)
- [Scene IPC Messages](#scene-ipc-messages)
- [Editor IPC Messages](#editor-ipc-messages)
- [Selection IPC Messages](#selection-ipc-messages)
- [Scene-Animation IPC Messages](#scene-animation-ipc-messages)
- [Scene-Layout IPC Messages](#scene-layout-ipc-messages)
- [Metrics IPC Messages](#metrics-ipc-messages)
- [Package Template IPC Messages](#package-template-ipc-messages)
- [Additional Scene IPC Messages](#additional-scene-ipc-messages)
- [Broadcast Events](#broadcast-events)
- [Renderer Process Listeners](#renderer-process-listeners)

## App IPC Messages

### `app:explore-project`

- **Purpose**: Open the project folder.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:explore-app`

- **Purpose**: Open the application folder.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:build-project`

- **Purpose**: Build the project.
- **Parameters**: Object containing build options.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:query-cocos-templates`

- **Purpose**: Query Cocos templates.
- **Parameters**: None.
- **Return Value**: List of templates.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:query-android-apilevels`

- **Purpose**: Query Android API levels.
- **Parameters**: None.
- **Return Value**: List of API levels.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:query-android-instant-apilevels`

- **Purpose**: Query Android Instant API levels.
- **Parameters**: None.
- **Return Value**: List of API levels.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:compile-project`

- **Purpose**: Compile the project.
- **Parameters**: Object containing compilation options.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:open-cocos-console-log`

- **Purpose**: Open the Cocos console log.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:stop-compile`

- **Purpose**: Stop compilation.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:run-project`

- **Purpose**: Run the project.
- **Parameters**: Object containing platform information.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:save-keystore`

- **Purpose**: Save keystore.
- **Parameters**: Keystore information.
- **Return Value**: Error message or success confirmation.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:update-build-preview-path`

- **Purpose**: Update build preview path.
- **Parameters**: Preview path.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:update-android-instant-preview-path`

- **Purpose**: Update Android Instant preview path.
- **Parameters**: Preview path.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:play-on-device`

- **Purpose**: Play on device.
- **Parameters**: Platform information.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:reload-on-device`

- **Purpose**: Reload on device.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:query-plugin-scripts`

- **Purpose**: Query plugin scripts.
- **Parameters**: Build platform name.
- **Return Value**: List of plugin scripts.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `app:rebuild-editor-engine`

- **Purpose**: Rebuild editor engine.
- **Parameters**: Callback function.
- **Return Value**: Error message.
- **Type**: Listen by Main Process.
- **Status**: Available.

## Asset-DB IPC Messages

### `asset-db:explore`

- **Purpose**: Open the folder containing the asset.
- **Parameters**: Asset URL.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:exists`

- **Purpose**: Query if the asset exists.
- **Parameters**: Asset URL.
- **Return Value**: Boolean (existence).
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:query-path-by-url`

- **Purpose**: Query path by URL.
- **Parameters**: Asset URL.
- **Return Value**: Filesystem path.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:query-uuid-by-url`

- **Purpose**: Query UUID by URL.
- **Parameters**: Asset URL.
- **Return Value**: UUID string.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:query-path-by-uuid`

- **Purpose**: Query path by UUID.
- **Parameters**: UUID string.
- **Return Value**: Filesystem path.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:query-url-by-uuid`

- **Purpose**: Query URL by UUID.
- **Parameters**: UUID string.
- **Return Value**: Asset URL.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:query-info-by-uuid`

- **Purpose**: Query asset info by UUID.
- **Parameters**: UUID string.
- **Return Value**: Asset info object.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:query-meta-info-by-uuid`

- **Purpose**: Query meta information by UUID.
- **Parameters**: UUID string.
- **Return Value**: Meta information object.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:deep-query`

- **Purpose**: Deep query assets.
- **Parameters**: None.
- **Return Value**: Full Asset Database information.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:query-assets`

- **Purpose**: Query assets.
- **Parameters**: Query criteria, type.
- **Return Value**: List of assets.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:query-mounts`

- **Purpose**: Query mount points.
- **Parameters**: None.
- **Return Value**: List of mount points.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:import-assets`

- **Purpose**: Import assets.
- **Parameters**: Import path list, target path, refresh flag.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:create-asset`

- **Purpose**: Create asset.
- **Parameters**: URL, content.
- **Return Value**: Error message or creation result.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:move-asset`

- **Purpose**: Move asset.
- **Parameters**: Source URL, target URL, show error dialog flag.
- **Return Value**: Error message or move result.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:delete-assets`

- **Purpose**: Delete assets.
- **Parameters**: URL array.
- **Return Value**: Error message or deletion result.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:save-exists`

- **Purpose**: Save existing asset.
- **Parameters**: URL, content.
- **Return Value**: Error message or save result.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:create-or-save`

- **Purpose**: Create or save asset.
- **Parameters**: URL, content.
- **Return Value**: Error message or operation result.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:save-meta`

- **Purpose**: Save metadata.
- **Parameters**: UUID, metadata object.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:refresh`

- **Purpose**: Refresh assets.
- **Parameters**: Path (optional).
- **Return Value**: Error message or refresh result.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:attach-mountpath`

- **Purpose**: Mount path.
- **Parameters**: Mount path object.
- **Return Value**: Error message or mount result.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:unattach-mountpath`

- **Purpose**: Unmount path.
- **Parameters**: Mount path.
- **Return Value**: Error message or unmount result.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:query-watch-state`

- **Purpose**: Query watch state.
- **Parameters**: None.
- **Return Value**: None (sends state to main window).
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:asset-changed`

- **Purpose**: Asset change processing.
- **Parameters**: Change details.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:asset-uuid-changed`

- **Purpose**: Asset UUID change processing.
- **Parameters**: Change details.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:assets-moved`

- **Purpose**: Assets moved processing.
- **Parameters**: Move details array.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:assets-created`

- **Purpose**: Assets created processing.
- **Parameters**: Creation details array.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:assets-deleted`

- **Purpose**: Assets deleted processing.
- **Parameters**: Deletion details array.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:script-import-failed`

- **Purpose**: Script import failed processing.
- **Parameters**: Failure details.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `asset-db:meta-backup`

- **Purpose**: Metadata backup processing.
- **Parameters**: Backup details array.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

## Dashboard IPC Messages

### `app:query-recent`

- **Purpose**: Query recently opened projects.
- **Parameters**: None.
- **Return Value**: Recent projects list.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `app:query-templates`

- **Purpose**: Query project templates.
- **Parameters**: None.
- **Return Value**: Templates list.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `app:create-project`

- **Purpose**: Create a project.
- **Parameters**: Project config object.
- **Return Value**: Error message or creation result.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `app:open-project`

- **Purpose**: Open a project.
- **Parameters**: Project path, login required flag.
- **Return Value**: Error message or opening result.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `app:delete-project`

- **Purpose**: Delete a project.
- **Parameters**: Project path.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `app:close-project`

- **Purpose**: Close a project.
- **Parameters**: Project path.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `app:window-minimize`

- **Purpose**: Minimize window.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `app:window-close`

- **Purpose**: Close window.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `app:get-last-create`

- **Purpose**: Get last created project.
- **Parameters**: None.
- **Return Value**: Last created project info.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `app:open-manual-doc`

- **Purpose**: Open manual documentation.
- **Parameters**: Doc path.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `app:open-api-doc`

- **Purpose**: Open API documentation.
- **Parameters**: Doc path.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `app:query-last-create-path`

- **Purpose**: Query last creation path.
- **Parameters**: None.
- **Return Value**: Last creation path.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

## Scene IPC Messages

### `scene:is-ready`

- **Purpose**: Check if scene editor is ready.
- **Parameters**: None.
- **Return Value**: Readiness status.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:new-scene`

- **Purpose**: Create a new scene.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:saved`

- **Purpose**: Notification after scene saves.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `scene:play-on-device`

- **Purpose**: Play scene on device.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `scene:reload-on-device`

- **Purpose**: Reload scene on device.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `scene:preview-server-scene-stashed`

- **Purpose**: Preview server scene stashed.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:load-package-scene-script`

- **Purpose**: Load package scene script.
- **Parameters**: Script path, package name.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:unload-package-scene-script`

- **Purpose**: Unload package scene script.
- **Parameters**: Script path.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:soft-reload`

- **Purpose**: Soft reload scene.
- **Parameters**: Reload parameters.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:enter-prefab-edit-mode`

- **Purpose**: Enter prefab edit mode.
- **Parameters**: Prefab UUID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:stash-and-save`

- **Purpose**: Stash and save scene.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:print-simulator-log`

- **Purpose**: Print simulator log.
- **Parameters**: Log message, log type.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:generate-texture-packer-preview-files`

- **Purpose**: Generate Texture Packer preview files.
- **Parameters**: Asset path.
- **Return Value**: Error message or result.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:query-texture-packer-preview-files`

- **Purpose**: Query Texture Packer preview files.
- **Parameters**: Asset path.
- **Return Value**: Preview file information.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:export-particle-plist`

- **Purpose**: Export particle plist file.
- **Parameters**: Particle data.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:update-edit-mode`

- **Purpose**: Update edit mode.
- **Parameters**: Edit mode information.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:undo`

- **Purpose**: Undo operation.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:redo`

- **Purpose**: Redo operation.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:undo-record`

- **Purpose**: Record Undo operation.
- **Parameters**: Object ID, operation info.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:undo-commit`

- **Purpose**: Commit Undo operation.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:undo-cancel`

- **Purpose**: Cancel Undo operation.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-dirty-state`

- **Purpose**: Query scene dirty state.
- **Parameters**: None.
- **Return Value**: Dirty state info.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-group-list`

- **Purpose**: Query group list.
- **Parameters**: None.
- **Return Value**: Group list.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-hierarchy`

- **Purpose**: Query scene hierarchy structure.
- **Parameters**: None.
- **Return Value**: Scene UUID and node hierarchy.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-nodes-by-comp-name`

- **Purpose**: Query nodes by component name.
- **Parameters**: Component name.
- **Return Value**: Node UUID list.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-node`

- **Purpose**: Query node info.
- **Parameters**: Node UUID.
- **Return Value**: Node info.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-node-info`

- **Purpose**: Query detailed node info.
- **Parameters**: Node UUID, type.
- **Return Value**: Detailed node info.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-node-functions`

- **Purpose**: Query node functions.
- **Parameters**: Node UUID.
- **Return Value**: Node function list.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:choose-last-rigid-body`

- **Purpose**: Choose previous rigid body.
- **Parameters**: Current node UUID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:choose-next-rigid-body`

- **Purpose**: Choose next rigid body.
- **Parameters**: Current node UUID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:is-child-class-of`

- **Purpose**: Check if it is a child class.
- **Parameters**: Class ID, Parent class ID.
- **Return Value**: Boolean.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:has-copied-component`

- **Purpose**: Check if there's a copied component.
- **Parameters**: None.
- **Return Value**: Boolean.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-animation-hierarchy`

- **Purpose**: Query animation hierarchy.
- **Parameters**: Node UUID.
- **Return Value**: Animation hierarchy structure.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-animation-list`

- **Purpose**: Query animation list.
- **Parameters**: Node UUID.
- **Return Value**: Animation clip UUID list.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-animation-properties`

- **Purpose**: Query animation properties.
- **Parameters**: Node UUID.
- **Return Value**: Animation property list.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-animation-record`

- **Purpose**: Query animation record state.
- **Parameters**: None.
- **Return Value**: Animation record state info.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-animation-clip`

- **Purpose**: Query animation clip.
- **Parameters**: Animation clip UUID.
- **Return Value**: Animation clip serialized data.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-asset-info`

- **Purpose**: Query asset info.
- **Parameters**: Asset UUID.
- **Return Value**: Asset info.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:query-nodes-by-usedby-uuid`

- **Purpose**: Query nodes by used asset UUID.
- **Parameters**: Asset UUID.
- **Return Value**: List of node UUIDs using the asset.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:create-nodes-by-uuids`

- **Purpose**: Create nodes by UUIDs.
- **Parameters**: UUID list, position, parent node, options.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:create-node-by-classid`

- **Purpose**: Create node by Class ID.
- **Parameters**: Class ID, position, parent node, options.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:create-node-by-prefab`

- **Purpose**: Create node by prefab.
- **Parameters**: Prefab name, position, parent node, options.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:new-property`

- **Purpose**: Create new property.
- **Parameters**: Property information.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:reset-property`

- **Purpose**: Reset property.
- **Parameters**: Property information.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:set-property`

- **Purpose**: Set property.
- **Parameters**: Property information.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:add-component`

- **Purpose**: Add component.
- **Parameters**: Node UUID, component name.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:remove-component`

- **Purpose**: Remove component.
- **Parameters**: Node UUID, component ID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:reset-node`

- **Purpose**: Reset node.
- **Parameters**: Node UUID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:reset-all`

- **Purpose**: Reset all components.
- **Parameters**: Node UUID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:move-up-component`

- **Purpose**: Move component up.
- **Parameters**: Node UUID, component ID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:move-down-component`

- **Purpose**: Move component down.
- **Parameters**: Node UUID, component ID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:reset-component`

- **Purpose**: Reset component.
- **Parameters**: Node UUID, component ID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:copy-component`

- **Purpose**: Copy component.
- **Parameters**: Component ID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:paste-component`

- **Purpose**: Paste component.
- **Parameters**: Node UUID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:move-nodes`

- **Purpose**: Move nodes.
- **Parameters**: Node UUID list, position, parent node.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:delete-nodes`

- **Purpose**: Delete nodes.
- **Parameters**: Node UUID list.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:copy-nodes`

- **Purpose**: Copy nodes.
- **Parameters**: Node UUID list.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:paste-nodes`

- **Purpose**: Paste nodes.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:duplicate-nodes`

- **Purpose**: Duplicate nodes.
- **Parameters**: Node UUID list.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:create-prefab`

- **Purpose**: Create prefab.
- **Parameters**: Node UUID, prefab path.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:apply-prefab`

- **Purpose**: Apply prefab.
- **Parameters**: Node UUID, prefab path.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:revert-prefab`

- **Purpose**: Revert prefab.
- **Parameters**: Node UUID, prefab path.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:set-prefab-sync`

- **Purpose**: Set prefab synchronization.
- **Parameters**: Node UUID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:break-prefab-instance`

- **Purpose**: Break prefab instance association.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:link-prefab`

- **Purpose**: Link prefab.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:regenerate-polygon-points`

- **Purpose**: Regenerate polygon points.
- **Parameters**: Node UUID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:search-skeleton-animation-clips`

- **Purpose**: Search skeleton animation clips.
- **Parameters**: Node UUID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:change-node-lock`

- **Purpose**: Change node lock state.
- **Parameters**: Node UUID, lock state.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:copy-editor-camera-data-to-nodes`

- **Purpose**: Copy editor camera data to nodes.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `scene:set-group-sync`

- **Purpose**: Set group synchronization.
- **Parameters**: Node UUID, group name.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene:generate_attached_node`

- **Purpose**: Generate attached node.
- **Parameters**: Node UUID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

## Editor IPC Messages

### `editor:dragstart`

- **Purpose**: Start drag operation.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `editor:dragend`

- **Purpose**: End drag operation.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `editor:project-profile-updated`

- **Purpose**: Project profile updated.
- **Parameters**: Profile object.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

## Selection IPC Messages

### `selection:selected`

- **Purpose**: Select node.
- **Parameters**: Type, ID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `selection:unselected`

- **Purpose**: Deselect node.
- **Parameters**: Type, ID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `selection:activated`

- **Purpose**: Activate node.
- **Parameters**: Type, ID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `selection:deactivated`

- **Purpose**: Deactivate node.
- **Parameters**: Type, ID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `selection:hoverin`

- **Purpose**: Hover in on node.
- **Parameters**: Type, ID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `selection:hoverout`

- **Purpose**: Hover out of node.
- **Parameters**: Type, ID.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

## Scene-Animation IPC Messages

### `scene-animation:query-animation-time`

- **Purpose**: Query animation time.
- **Parameters**: Animation clip info.
- **Return Value**: Time information.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene-animation:animation-time-changed`

- **Purpose**: Animation time changed.
- **Parameters**: New time value.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene-animation:animation-clip-changed`

- **Purpose**: Animation clip changed.
- **Parameters**: Clip info.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene-animation:save-clip`

- **Purpose**: Save animation clip.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene-animation:set-animation-speed`

- **Purpose**: Set animation speed.
- **Parameters**: Speed value.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene-animation:change-animation-record`

- **Purpose**: Change animation record state.
- **Parameters**: Record state.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene-animation:mount-clip`

- **Purpose**: Mount animation clip.
- **Parameters**: Clip info, options.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene-animation:change-animation-state`

- **Purpose**: Change animation playback state (Play/Pause).
- **Parameters**: State.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `scene-animation:change-animation-current-clip`

- **Purpose**: Change current animation clip.
- **Parameters**: Clip info.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

## Scene-Layout IPC Messages

### `scene-layout:center-nodes`

- **Purpose**: Center camera on nodes.
- **Parameters**: Node list.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

## Metrics IPC Messages

### `metrics:track-event`

- **Purpose**: Track event.
- **Parameters**: Category, action, label, etc.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `editor:renderer-console-error`

- **Purpose**: Send renderer console error.
- **Parameters**: Error stack info.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

### `metrics:track-exception`

- **Purpose**: Track exception.
- **Parameters**: Exception info.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Available.

## Package Template IPC Messages

### `package-template:clicked`

- **Purpose**: Package template clicked.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `package-template:hello`

- **Purpose**: Package template greeting message.
- **Parameters**: Event object.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `package-template:open`

- **Purpose**: Open package template panel.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

### `package-template:say-hello`

- **Purpose**: Send greeting.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Listen by Main Process.
- **Status**: Unavailable (No Response).

## Additional Scene IPC Messages

### `asset-db:asset-changed`

- **Purpose**: Asset change processing.
- **Parameters**: Change details.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Available.

### `asset-db:assets-moved`

- **Purpose**: Assets moved processing.
- **Parameters**: Move details array.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Available.

### `asset-db:assets-created`

- **Purpose**: Assets created processing.
- **Parameters**: Creation details array.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Available.

### `asset-db:assets-deleted`

- **Purpose**: Assets deleted processing.
- **Parameters**: Deletion details array.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Available.

### `editor:ready`

- **Purpose**: Editor ready notification.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `editor:console-failed`

- **Purpose**: Console failure message.
- **Parameters**: Failure info.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `editor:console-warn`

- **Purpose**: Console warning message.
- **Parameters**: Warning info.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `editor:console-error`

- **Purpose**: Console error message.
- **Parameters**: Error info.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `editor:console-clear`

- **Purpose**: Clear console.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `compiler:state-changed`

- **Purpose**: Compiler state change.
- **Parameters**: State.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `preview-server:preview-port-changed`

- **Purpose**: Preview server port change.
- **Parameters**: None.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `preview-server:connects-changed`

- **Purpose**: Preview server connection count change.
- **Parameters**: Connection count.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `im-plugin:update-im-html`

- **Purpose**: Update IM plugin HTML.
- **Parameters**: HTML content.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `asset-db:state-changed`

- **Purpose**: Asset Database state change.
- **Parameters**: State.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `asset-db:watch-state-changed`

- **Purpose**: Asset Database watch state change.
- **Parameters**: State.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `editor:online-status-changed`

- **Purpose**: Editor online status change.
- **Parameters**: State ('online' or 'offline').
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `scene:node-component-updated`

- **Purpose**: Node component updated.
- **Parameters**: Object containing node, component, and property info.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `scene:node-component-added`

- **Purpose**: Node component added.
- **Parameters**: Object containing node and component info.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

### `scene:node-component-removed`

- **Purpose**: Node component removed.
- **Parameters**: Object containing node and component info.
- **Return Value**: None.
- **Type**: Broadcast Event.
- **Status**: Untested.

## Broadcast Events

Common events broadcasted to the renderer process (usually from the main process):

- `scene:saved`
- `scene:play-on-device`
- `scene:reload-on-device`
- `asset-db:asset-changed`
- `asset-db:assets-moved`
- `asset-db:assets-created`
- `asset-db:assets-deleted`
- `editor:ready`
- `editor:console-failed`
- `editor:console-warn`
- `editor:console-error`
- `editor:console-clear`
- `compiler:state-changed`
- `preview-server:preview-port-changed`
- `preview-server:connects-changed`
- `im-plugin:update-im-html`
- `asset-db:state-changed`
- `asset-db:watch-state-changed`
- `editor:online-status-changed`
- `scene:node-component-updated`
- `scene:node-component-added`
- `scene:node-component-removed`
- `package-template:hello`

## Renderer Process Listeners

Events actively listened to by the renderer process:

- `asset-db:assets-moved`
- `asset-db:assets-deleted`
- `asset-db:assets-created`
- `compiler:state-changed`
- `preview-server:preview-port-changed`
- `preview-server:connects-changed`
- `im-plugin:update-im-html`
- `asset-db:state-changed`
- `asset-db:watch-state-changed`
- `editor:console-failed`
- `editor:console-warn`
- `editor:console-error`
- `editor:console-clear`
