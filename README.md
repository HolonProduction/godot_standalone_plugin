# :package: Standalone Plugin
[![Godot: 4.0.beta5](https://img.shields.io/static/v1?label=Godot&message=4.0.beta5&color=grey&style=flat-square&logo=godotengine&logoColor=white&labelColor=478cbf)](https://godotengine.org)

This package makes it possible to develop a godot plugin and a corresponding standalone app using the same codebase.

---

## :seedling: Setup

1. Add your plugin to the `res://addons` folder or create a new plugin.
2. Copy `res://standalone_plugin/editor.gd` into your plugins directory. (And rename it if you want.)
3. Modify the main script of your plugin to inherit the copied file from step 2.
4. You are ready to go. :tada:
> Step 2 and 3 can also be done automatically by calling `Project>Tools>Make Plugin Standalone`

> If you use any of the helper options inside of the tools menu it is highly recommended to restart the edior afterwards. The build in script editor may not update the changed scripts otherwise. (No way to do it by code :/)
---
## :rocket: Features
Not every method of `EditorPlugin` can be reproduced without Godot internals like the import or export systems. Therefore not all methods of `EditorPlugin` will have an effect when running standalone. This won't throw an error though, in case your plugin has more functionality in Godot but should still run standalone.

Calling unsuported methods won't throw an error but when it comes to return values there can be problems.
You should add `null` checks for all return values (e.g. `get_script_create_dialog` will return `null` on runtime). Same goes for everything returned from `EditorInterface`

### Check if running standalone
If your plugin relies on godot internal systems adapting to standalone may require to disable some features. You can easily test whether you are running in the editor by calling `Engine.is_editor_hint`. This allows you to enable certain features only when running standalone or as plugin. For example the standalone app may require additional steps to select a file to edit.

### Supported methods of `EditorPlugin`
| Method | Standalone | Editor | May Return `null`  |
|---|:---:|:---:|:---:|
| `_apply_changes` _virtual_ | :x: | :heavy_check_mark: |   |
| `_build` _virtual_ | :x: | :heavy_check_mark: |   |
| `_clear` _virtual_ | :x: | :heavy_check_mark: |   |
| `_disable_plugin` _virtual_ | :heavy_check_mark: | :heavy_check_mark: |   |
| `_edit` _virtual_ | :x:  | :heavy_check_mark: |   |
| `_enable_plugin` _virtual_ | :heavy_check_mark: | :heavy_check_mark: |   |
| `_forward_3d_draw_over_viewport` _virtual_ | :x: | :heavy_check_mark: |   |
| `_forward_3d_force_draw_over_viewport` _virtual_ | :x: | :heavy_check_mark: |   |
| `_forward_3d_gui_input` _virtual_ | :x: | :heavy_check_mark: |   |
| `_forward_canvas_draw_over_viewport` _virtual_ | :x: | :heavy_check_mark: |   |
| `_forward_canvas_force_draw_over_viewport` _virtual_ | :x: | :heavy_check_mark: |   |
| `_forward_canvas_gui_input` _virtual_ | :x: | :heavy_check_mark: |   |
| `_get_breakpoints` _virtual_ | :x: | :heavy_check_mark: |   |
| `_get_plugin_icon` _virtual_ | :heavy_check_mark: | :heavy_check_mark: |   |
| `_get_plugin_name` _virtual_ | :heavy_check_mark: | :heavy_check_mark: |   |
| `_get_state` _virtual_ | :x: | :heavy_check_mark: |   |
| `_get_window_layout` _virtual_ | :heavy_check_mark: | :heavy_check_mark: |   |
| `_handles` _virtual_ | :x: | :heavy_check_mark: |   |
| `_has_main_screen` _virtual_ | :heavy_check_mark: | :heavy_check_mark: |   |
| `_make_visible` _virtual_ | :heavy_check_mark: | :heavy_check_mark: |   |
| `_save_external_data` _virtual_ | :heavy_check_mark: only when exiting | :heavy_check_mark: |   |
| `_set_state` _virtual_ | :x: | :heavy_check_mark: |   |
| `_set_window_layout` _virtual_ | :heavy_check_mark: | :heavy_check_mark: |   |
| `add_autoload_singelton` | :large_orange_diamond: not at runtime but when registered in editor plugin it is still in the export | :heavy_check_mark: |   |
| `add_control_to_bottom_panel` | :heavy_check_mark: | :heavy_check_mark: |   |
| `add_control_to_container` | :large_orange_diamond: Only `CONTAINER_TOOLBAR`. Also adds new containers that are not supported in editor: `CONTAINER_LAUNCH_PAD` | :heavy_check_mark: |   |
| `add_control_to_dock` | :heavy_check_mark: | :heavy_check_mark: |   |
| `add_custom_type` | :x: | :heavy_check_mark: |   |
| `add_debugger_plugin` | :x: | :heavy_check_mark: |   |
| `add_export_plugin` | :x: | :heavy_check_mark: |   |
| `add_import_plugin` | :x: | :heavy_check_mark: |   |
| `add_inspector_plugin` | :x: | :heavy_check_mark: |   |
| `add_scene_format_importer_plugin` | :x: | :heavy_check_mark: |   |
| `add_scene_post_import_plugin` | :x: | :heavy_check_mark: |   |
| `add_spacial_gizmo_plugin` | :x: | :heavy_check_mark: |   |
| `add_tool_menu_item` | :large_orange_diamond: use the standalone exclusive menu api | :heavy_check_mark: |   |
| `add_tool_submenu_item` | :large_orange_diamond: use the standalone exclusive menu api | :heavy_check_mark: |   |
| `add_translation_parser_plugin` | :x: | :heavy_check_mark: |   |
| `add_undo_redo_inspector_hook_callback` | :x: | :heavy_check_mark: |   |
| `get_editor_interface` | :large_orange_diamond: returns an standalone interface | :heavy_check_mark: | No  |
| `get_export_as_menu` | :x: | :heavy_check_mark: | Yes |
| `get_script_create_dialog` | :x: | :heavy_check_mark: | Yes |
| `get_undo_redo` | :heavy_check_mark: | :heavy_check_mark: | No |
| `hide_bottom_panel` | :heavy_check_mark: | :heavy_check_mark: |   |
| `make_bottom_panel_item_visible` | :heavy_check_mark: | :heavy_check_mark: |   |
| `queue_save_layout` | :heavy_check_mark: | :heavy_check_mark: |   |
| `remove_autoload_singelton` | :x: | :heavy_check_mark: |   |
| `remove_control_from_bottom_panel` | :heavy_check_mark: | :heavy_check_mark: |   |
| `remove_control_from_container` | :large_orange_diamond: | :heavy_check_mark: |   |
| `remove_control_from_docks` | :heavy_check_mark: | :heavy_check_mark: |   |
| `remove_custom_type` | :x: | :heavy_check_mark: |   |
| `remove_debugger_plugin` | :x: | :heavy_check_mark: |   |
| `remove_export_plugin` | :x: | :heavy_check_mark: |   |
| `remove_import_plugin` | :x: | :heavy_check_mark: |   |
| `remove_inspector_plugin` | :x: | :heavy_check_mark: |   |
| `remove_scene_format_importer_plugin` | :x: | :heavy_check_mark: |   |
| `remove_scene_post_import_plugin` | :x: | :heavy_check_mark: |   |
| `remove_spatial_gizmo_plugin` | :x: | :heavy_check_mark: |   |
| `remove_tool_menu_item` | :x: | :heavy_check_mark: |   |
| `remove_translation_parser_plugin` | :x: | :heavy_check_mark: |   |
| `remove_undo_redo_inspector_hook_callback` | :x: | :heavy_check_mark: |   |
| `set_force_draw_over_forwarding_enabled` | :x: | :heavy_check_mark: |   |
| `update_overlays` | :x: | :heavy_check_mark: |   |
| `add_menu_popup` | :heavy_check_mark::construction: | :x: |   |
| `remove_menu_popup` | :heavy_check_mark::construction: | :x: |   |

### Supported methods of `EditorInterface`
| Method | Standalone | Editor | May Return `null`  |
|---|:---:|:---:|:---:|
| `edit_node` | :x: | :heavy_check_mark: |   |
| `edit_resource` | :x: | :heavy_check_mark: |   |
| `edit_script` | :x: | :heavy_check_mark: |   |
| `get_base_control` | :heavy_check_mark: | :heavy_check_mark: | No |
| `get_command_palette` | :x: | :heavy_check_mark: | Yes |
| `get_current_path` | :large_orange_diamond: will always return `user://` | :heavy_check_mark: | No |
| `get_edited_scene_root` | :x: | :heavy_check_mark: | Yes |
| `get_editor_main_screen` | :heavy_check_mark: | :heavy_check_mark: | No
| `get_editor_paths` | :heavy_check_mark: the paths are diffrent | :heavy_check_mark: | No |
| `get_editor_scale` | :heavy_check_mark: will always return `1.0` | :heavy_check_mark: | No |
| `get_editor_settings` | :x: | :heavy_check_mark: | Yes |
| `get_file_system_dock` | :x: | :heavy_check_mark: | Yes |
| `get_inspector` | :x: | :heavy_check_mark: | Yes |
| `get_open_scenes` | :large_orange_diamond: will always be empty | :heavy_check_mark: | No |
| `get_playing_scene` | :large_orange_diamond: will always be empty | :heavy_check_mark: | No |
| `get_resource_filesystem` | :x: | :heavy_check_mark: | Yes |
| `get_resource_previewer` | :x: | :heavy_check_mark: | Yes |
| `get_script_editor` | :x: | :heavy_check_mark: | Yes |
| `get_selected_path` | :large_orange_diamond: will always return `user://` | :heavy_check_mark: | No |
| `get_selection` | :x: | :heavy_check_mark: | Yes |
| `inspect_object` | :x: | :heavy_check_mark: |   |
| `is_playing_scene` | :large_orange_diamond: will always be `false` | :heavy_check_mark: | No |
| `is_plugin_enabled` | :heavy_check_mark: | :heavy_check_mark: | No |
| `make_mesh_previews` | :x: will return `PlaceholderTexture2D` | :heavy_check_mark: | No |
| `open_scene_from_path` | :x: | :heavy_check_mark: |   |
| `play_current_scene` | :x: | :heavy_check_mark: |   |
| `play_custom_scene` | :x: | :heavy_check_mark: |   |
| `play_main_scene` | :x: | :heavy_check_mark: |   |
| `reload_scene_from_path` | :x: | :heavy_check_mark: |   |
| `restart_editor` | :x: | :heavy_check_mark: |   |
| `save_scene` | :x: will always return `OK` | :heavy_check_mark: | No |
| `save_scene_as` | :x: | :heavy_check_mark: |   |
| `select_file` | :x: | :heavy_check_mark: |   |
| `set_main_screen_editor` | :heavy_check_mark: | :heavy_check_mark: |   |
| `set_plugin_enabled` | :heavy_check_mark: | :heavy_check_mark: |   |
| `stop_playing_scene` | :x: | :heavy_check_mark: |   |


---

## :wrench: For the curious minded (and for me in one month)

### :shrug: _How on earth does this piece of witchcraft work?_

The script you copied before starts with the following line of code.
```GDScript
#@standalone
```

Before building your project the package will loop over all activated plugins. It will check for each whether the main script inherits a script which starts with this very line. 
In this way a list of plugins that should run standalone is obtained. This list is then stored in your ProjectSettings (the key is `editor_plugins/standalone`).
When running your app the runtime compontents will read the list from the settings and there you have it.

### :raising_hand: _But wait. The script does still inherit `EditorPlugin` or does it?_

The script in your plugins directory does. This is the reason why your plugin can still be used in the godot editor. But this package does another usefull thing on build time. With the list of standalone plugins it also has a list of all scripts, that inherit `EditorPlugin`. Luckily Godot has the powerfull feature to load resource packs on runtime (read more on it [here](https://docs.godotengine.org/en/latest/classes/class_projectsettings.html#class-projectsettings-method-load-resource-pack)). This resource packs can override files at certain paths.  
While building a resource pack is created, that overrides all the scripts inheriting `EditorPlugin`. They get replaced with a version that hooks the methods into the standalone UI.

### :ok_person: _I see, it is that simple!_

Of course one still needs an `ExportPlugin` that ensures that the resource pack and all the plugins are included in the export. But you must have thought of that yourself, haven't you?

### :bow: _Sure..._
