# Modifiable Resource

[Power Type](../power_types.md)

An extension of the [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/), provides a variable with an assignable and modifiable minimum and maximum value.

Type ID: `origins-math:modifiable_resource`

!!! note
    This power type provides a variable like the [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/), whose minimum and maximum value can be modified through the [Modify Resource Minimum (Power Type)](./modify_resource_minimum.md) and [Modify Resource Maximum (Power Type)](./modify_resource_maximum.md), respectively.

!!! warning
	A bug in `1.20.1` prevents the `enforce_limits` and `retain_value` from being updated when using [`/reload`](https://minecraft.wiki/w/Commands/reload). To refresh the power, simply exit the world and rejoin. Sorry for any possible inconveniences.

### Fields

| Field			| Type | Default | Description
|---------------|------|---------|-------------
| `min`				|[Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/)		|			 | The minimum value of the resource.
| `max`				|[Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/)		|			 | The maximum value of the resource.
| `hud_render`		|[Hud Render](https://origins.readthedocs.io/en/latest/types/data_types/hud_render/)| _optional_ | Determines how the resource is visualized on the HUD.
| `start_value`		|[Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/)		| _optional_ | The value of the resource when the entity first receives the power. If not set, this will be set to the value of the `min` integer field.
| `enforce_limits`	|[Boolean](https://origins.readthedocs.io/en/latest/types/data_types/boolean/)		|   `true`	 | Whether or not the value of the resource should be bounded by the value of `min` and `max`. |
| `retain_value`	|[Boolean](https://origins.readthedocs.io/en/latest/types/data_types/boolean/)		|   `false`	 | Whether or not the previous value of the resource is retained. |
| `min_action`		|[Entity Action Type](../entity_action_types.md)									| _optional_ | If specified, this action will be executed on the entity whenever the minimum value is reached.
| `max_action`		|[Entity Action Type](../entity_action_types.md)									| _optional_ | If specified, this action will be executed on the entity whenever the maximum value is reached.

### Examples

```json
{
	"type": "origins:multiple",

	"resource": {
		"type": "origins-math:modifiable_resource",
		"hud_render": {
			"should_render": true,
			"sprite_location": "origins:textures/gui/resource_bar.png",
			"bar_index": 1
		},
		"min": 0,
		"max": 75
	},

	"change_maximum": {
		"type": "origins-math:modify_resource_maximum",
		"condition": {
			"type": "origins:sneaking"
		},
		"modifier": {
			"value": 65,
			"operation": "add_base_early"
		}
	}
}
```

This example will provide a variable with an initial minimum value of `0` and an initial maximum value of `75`. When the user is sneaking, the effective maximum value of the resource would be `140`. 

### More on `enforce_limits` and `retain_value`

The `enforce_limits` field indicates if the value of the resource should be within `max` and `min`, while `retain_value` indicates if the current value should truly be bounded by `min` and `max`.

To elaborate, `enforce_limits` will only affect the "resulting" current value of the resource, not it's internal value, depending on the value of `retain_value`. Therefore, a resource whose value is `75` (maximum) could have an internal value of `96` if `retain_value` is `true`. 

To clear things up, let's take the previous example, and say the value of the resource sub-power is currently `96` while the player is sneaking. If the player "unsneaks", the new value will be `75`, since the `enforce_limits` field is not specified, giving it a default value of `true`. Once the player sneaks again, the value will still be `75`, since the value was limited to `75` by the previous maximum value and `retain_value` is `false` since it isn't specified.

If we want the resource to have a value of `96` while the player is sneaking, but still have the limits enforced (so the value will be `75` is the player isn't sneaking), then the `retain_value` field is used.

If `retain_value` is true, the value of the resource is `75` when the player isn't sneaking, since `enforce_limits` is `true`, therefore, it's bounded by the `max` value. When the player is sneaking, the value of the resource is `96`, since it's not bounded by either the `max` or the `min` value.

If we want to have `96` as our real value despite the `max` value being lower than it, we can set `enforce_limits` to `false`. Now, the value of `resource` is always `96`, whether or not the player is sneaking.

For a quick recap, you can look below.

- Is `enforce_limits` set to `true`?
	- Is `retain_value` set to `true`?
		- The resource value will be bounded by `min` and `max`, but it's internal value will be unbound (if it isn't bound).
		- Otherwise, the resource value, and it's internal value is bounded by `min` and `max`.
	- Otherwise, the internal resource value is used, unbound by `min` and `max`.

!!! warning
	Do note that when you change the value of this resource, the new value will still be bound by the modified value of `min` and `max` even if `enforce_limits` is `false`. However, if the new value isn't bounded by `min` or `max` and `retain_value` is `true`, the new value is discarded, while the current value is retained.