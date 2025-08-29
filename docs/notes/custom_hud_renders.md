# Custom Hud Renders

### Introduction

When you start up Minecraft with logs enabled, you might've seen the following in the logs.

```
[Render thread/INFO] (origins-math/ResourceBackedInjector) Created custom HUD render fields ([origins-math:hud_render]) for origins-math:modifiable_resource
[Render thread/INFO] (origins-math/CustomHudRenderInjector) Cancelling creation of custom HUD Render field for origins-math:modify_attribute_like_resource (same SerializableData instances)
```

Like [Resource-backed Fields](./resource_backed_fields.md), this is what Origins: Math does behind-the-scenes for you to be able to use Resources in an [Hud Render](https://origins.readthedocs.io/en/latest/types/data_types/hud_render/)!

!!! warning
	You need to have the [Extended Compatibility through ASM](../experiments/extended_compatibility_through_asm.md) Alpha Experiment enabled to use this feature on **Minecraft 1.20.4**! Other versions do not need this Alpha Experiment enabled.

### Creation

For every [Power Type](../types/power_types.md) that gets added, Origins: Math adds a field that supports the custom [Origins Math Hud Render](../types/data_types/origins_math_hud_render.md), which allows you to use Resources directly into the `bar_index`, `icon_index` and `order` fields. 

You may have noticed that there are two versions of the message:

- "Created custom HUD render fields...", and  
- "Cancelling creation of custom HUD Render field..."

Here, "Cancelling creation" simply means that there is no Hud Render field for **Origins: Math** to create, as the power doesn't have one!

When an Hud Render field is created for a power, it will **always** be prepended by `origins-math:`. Like [Resource-backed Fields](./resource_backed_fields.md), this is done to avoid confusion when others read your code, as it would normally be unusual for a Resource to be the value for the Hud Render fields.

For instance, for the [Active Self](https://origins.readthedocs.io/en/latest/types/power_types/active_self) power, it has a `hud_render` field which contains the standard Origins [Hud Render](https://origins.readthedocs.io/en/latest/types/data_types/hud_render/). It's [Origins Math Hud Render](./custom_hud_renders.md) would be in the `origins-math:hud_render` field.

### Priority

It is important to note that the **Origins Math Hud Render** will take priority over the Hud Render in the corresponding field.

For instance, if you have an Hud Render in the `hud_render` field **and** an **Origins Math Hud Render** in the `origins-math:hud_render` field, the `origins-math:hud_render` field will be prioritized and effectively replace the `hud_render` field.

Likewise, if you have an Hud Render in the `hud_render` field **and** an **Origins Math Hud Render** in the `origins-math:another_hud_render` field, the `hud_render` and `origins-math:another_hud_render` fields do not change, as the `origins-math:another_hud_render` would attempt to replace the `another_hud_render` field.

This allows your Hud Render to effectively have a "fallback" option in case the **Origins: Math** mod doesn't exist.

### Example usage

For this section, say we have a "Status Effect Power" that has 8 "stacks", represented by icons and a specified duration, represented by the bar.

```json
"hud_render":{
    "sprite_location": "origins-math:custom_resource_bar.png",
    "bar_index": 0,
	"icon_index": 0
}
```

The example above creates a standard [Hud Render](https://origins.readthedocs.io/en/latest/types/data_types/hud_render/), with a custom `sprite_location`. If we want to show the different stacks, we would probably have something like this:

```json
{
	"type": "origins:multiple",

	// ...

	"icon_1": {
		"type": "origins:resource",
		"hud_render": {
			"sprite_location": "origins-math:custom_resource_bar.png",
			"bar_index": 0,
			"icon_index": 0,
			"condition": {
				"type": "origins:resource",
				"resource": "origins-math:custom_effect_stacks",
				"comparison": "==",
				"compare_to": 1
			}
		}
	},

	"icon_2": {
		"type": "origins:resource",
		"hud_render": {
			"sprite_location": "origins-math:custom_resource_bar.png",
			"bar_index": 0,
			"icon_index": 1,
			"condition": {
				"type": "origins:resource",
				"resource": "origins-math:custom_effect_stacks",
				"comparison": "==",
				"compare_to": 2
			}
		}
	},

	// ...

	"icon_8": {
		"type": "origins:resource",
		"hud_render": {
			"sprite_location": "origins-math:custom_resource_bar.png",
			"bar_index": 0,
			"icon_index": 7,
			"condition": {
				"type": "origins:resource",
				"resource": "origins-math:custom_effect_stacks",
				"comparison": "==",
				"compare_to": 8
			}
		}
	}
}
```

Or in 1.20.4, where [Hud Render](https://origins.readthedocs.io/en/latest/types/data_types/hud_render/) supports arrays.

```json
{
	"duration": {
		"type": "origins:resource",
		"hud_render": [
			{
				"sprite_location": "origins-math:custom_resource_bar.png",
				"bar_index": 0,
				"icon_index": 0,
				"condition": {
					"type": "origins:resource",
					"resource": "origins-math:custom_effect_stacks",
					"comparison": "==",
					"compare_to": 1
				}
			},
			{
				"sprite_location": "origins-math:custom_resource_bar.png",
				"bar_index": 0,
				"icon_index": 1,
				"condition": {
					"type": "origins:resource",
					"resource": "origins-math:custom_effect_stacks",
					"comparison": "==",
					"compare_to": 2
				}
			},

			// ...

			{
				"sprite_location": "origins-math:custom_resource_bar.png",
				"bar_index": 0,
				"icon_index": 7,
				"condition": {
					"type": "origins:resource",
					"resource": "origins-math:custom_effect_stacks",
					"comparison": "==",
					"compare_to": 8
				}
			}
		]
	}
}
```

This isn't even the full power, and it's already this long! You still have to account for the other icons and setting the duration to all these resources!

With an [Origins Math Hud Render](../types/data_types/origins_math_hud_render.md), doing this is way easier and **significantly** shorter!

```json
{
	"type": "origins:multiple",

	// ...

	"icon_index_shift": {
		"type": "origins-math:math_resource",
		"expression": "stacks - 1",
		"variables": {
			"stacks": "origins-math:custom_effect_stacks"
		}
	},

	"duration": {
		"type": "origins:resource",
		"origins-math:hud_render": {
			"sprite_location": "origins-math:custom_resource_bar.png",
			"bar_index": 0,
			"icon_index": "*:*_icon_index_shift",
		}
	}
}
```

You can even add a fallback icon in case they don't have Origins: Math so that at least *something* renders.