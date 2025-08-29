---
title: Origins Math Hud Render (Data Type)
---

# Origins Math Hud Render

[Data Type](../data_types.md)

An [Object](https://origins.readthedocs.io/en/latest/types/data_types/object) or [Array](https://origins.readthedocs.io/en/latest/types/data_types/array.md) of [Objects](https://origins.readthedocs.io/en/latest/types/data_types/object) <span style="color: darkred">**(1.20.4 only!)**</span> used to define how a resource or cooldown bar should be rendered.

!!!	note
	If the specified HUD render is an array of objects, then the HUD render will choose the first object that is allowed to be rendered (its `should_render` field set to `true`) and its condition fulfilled (or if its `condition` field is absent) from top to bottom. The `order` value of the very first object will also be inherited by the following objects that do not have the `order` field specified.

	You may only use an array in the <span style="color: darkred">**1.20.4 alpha version**</span> of Origins.

!!!	caution
	The entity condition specified in the `condition` field is only evaluated on the <span style="color:goldenrod"><b>client-side</b></span>, therefore, using entity condition types that only work on the server-side will not work.

###	Fields

| Field  | Type | Default | Description
| -------|------|---------|-------------
| `should_render` | [Boolean](https://origins.readthedocs.io/en/latest/types/data_types/boolean) | `true` | Whether the bar should be visible or not.
| `sprite_location` | [Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier) | `"origins:textures/gui/resource_bar.png"` | The path to the file in the assets which contains what the bar looks like. See the [List of sprites](https://origins.readthedocs.io/en/latest/misc/extras/sprites) for a list of files included by default in the mod.
| `bar_index` | [Resource-backed Integer](./resource-backed_integer.md) | `0` | The indexed position of the bar on the sprite to use. Please note that indexes start at `0`.
| `icon_index` <br> <span style="color: darkred">**(1.20.2+)**</span> | [Resource-backed Integer](./resource-backed_integer.md) | `0` | The indexed position of the icon on the sprite to use. Please note that indexes start at `0`.
| `condition` | [Entity Condition Type](../entity_condition_types.md) | _optional_ | If set (and `should_render` is true), the bar will only display when the entity with the power fulfills this condition.
| `inverted` | [Boolean](https://origins.readthedocs.io/en/latest/types/data_types/boolean) | `false` | If set to true, inverts the way the hud render process (it'll look like its value is being decreased).
| `order` <br> <span style="color: darkred">**(1.20.4)**</span> | [Resource-backed Integer](./resource-backed_integer.md) | *optional* | If specified, this determines the position of the HUD render when being rendered. The higher the `order` value is, the higher it is on the rendered HUD render stack.

### Examples

```json
"origins-math:hud_render": {
	"bar_index": 4,
	"condition": {
		"type": "origins:power_active",
		"power": "origins:phantomize"
	}
}
```

This definition shows the resource/cooldown as the Elytrian bar (white and with a wings icon), but only while the player is phantomized.
<br>

```json
"origins-math:hud_render": {
    "sprite_location": "origins:textures/gui/community/spiderkolo/resource_bar_03.png",
    "bar_index": 5
}
```

This definition shows the resource/cooldown as a white bar with a bone icon.
<br>

```json
"origins-math:hud_render": [
	{
		"sprite_location": "origins:textures/gui/community/spiderkolo/resource_bar_03.png",
		"bar_index": 0,
		"icon_index": "origins-math:level"
	}
]
```
This definition will show the resource/cooldown as a light blue bar that changes its icon dynamically based on the value of the `origins-math:level` resource.