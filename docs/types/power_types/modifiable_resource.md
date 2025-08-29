---
title: Modifiable Resource (Power Type)
---

# Modifiable Resource

[Power Type](../power_types.md)

!!! attention
	The [Modifiable Resource (Power Type)](./modifiable_resource.md) is an experimental feature, and is subject to change.

	Experimental features are brand new features added to **Origins: Math**, aimed at testing current implementations of highly-requested resource-related features for standard Origins.

	It is highly suggested that you read the [Experimental Feature: Modifiable Resource](#experimental-feature-modifiable-resource) section to get a grasp on how this Power Type works.

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
| **(EXPERIMENTAL)** `min_action`		|[Entity Action Type](../entity_action_types.md)									| _optional_ | If specified, this action will be executed on the entity whenever the minimum value is reached.
| **(EXPERIMENTAL)** `max_action`		|[Entity Action Type](../entity_action_types.md)									| _optional_ | If specified, this action will be executed on the entity whenever the maximum value is reached.

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

### Experimental Feature: Modifiable Resource

To better explain the caveats and features of this new Power Type, an in-depth explanation of how it works (specifically, `enforce_limits` and `retain_value`) can be seen below.

If you would like to convey your opinion and thoughts on the matter, you may

- Ping @_xridge on the official [Origins Discord server](https://discord.gg/4mTMHu3) in the #addon-talk channel.
- Alternatively, you may DM me directly. (make sure you're in the [Origins Discord server](https://discord.gg/4mTMHu3) though!).

!!! tip
	If you're a developer and would like to suggest your own implementation, you may create a Pull Request on the [Origins: Math GitHub page](https://github.com/xrickastley/origins-math).

#### Internal values, `enforce_limits` and `retain_value`

The `enforce_limits` field indicates if the value of the resource should be within `max` and `min`, while `retain_value` indicates if the current value should truly be bounded by `min` and `max`.

To elaborate, `enforce_limits` will only affect the "resulting" current value of the resource, not its internal value, depending on the value of `retain_value`. Therefore, a resource whose value is `75` (maximum) could have an internal value of `96` if `retain_value` is `true`. 

Let's take the previous example, and say the value of the resource sub-power is currently `96` while the player is sneaking. If the player "unsneaks", the new value will be `75`, since the `enforce_limits` field is not specified, giving it a default value of `true`. Once the player sneaks again, the value will still be `75`, since the value was limited to `75` by the previous maximum value and `retain_value` is `false` since it isn't specified.

If we want the resource to have a value of `96` while the player is sneaking, but still have the limits enforced (so the value will be `75` is the player isn't sneaking), then the `retain_value` field is used.

If `retain_value` is `true`, the value of the resource is `75` when the player isn't sneaking, since `enforce_limits` is `true`, therefore, it's bounded by the `max` value. When the player is sneaking, the value of the resource is `96`, since it's not bounded by either the `max` or the `min` value and its value is retained.

If we want to have `96` as our real value despite the `max` bound being lower than it, we can set `enforce_limits` to `false`. Now, the value of `resource` is always `96`, whether or not the player is sneaking.

For a quick recap, you can look below.

- Is `enforce_limits` set to `true`?
	- **Yes:** Is `retain_value` set to `true`?
		- **Yes:** The obtained resource value will be bounded by `min` and `max`, but its internal value will be unbound (if it isn't bound).
		- **No:** The resource value and its internal value is bounded by `min` and `max`.
	- **No:** The internal resource value is used, unbound by `min` and `max`.

#### Value locks and `retain_value`

Now that we have cleared up how `enforce_limits` and `retain_value` work in obtaining a value of a resource, now we will discuss how they work in setting the value of a resource.

Take note that even if `enforce_limits` is `false`, it will **not** allow setting a value greater than the current maximum or lower than the current minimum. `enforce_limits` is only used when the maximum or minimum bounds change.

Now, onto `retain_value`. `retain_value` will allow for the resources' internal value to be kept. However, this causes a complication when a resource value is being set.

With `retain_value` active, a "lock" is placed if it's current internal value isn't bound by the current minimum and maximum. This lock will "lock" the current internal value and will only allow edits to the value if the new value is bound by the current minimum and maximum.

This means that, with `retain_value` active, you can only set the value if

- a. The internal current value is bound by the current minimum and maximum (This will accept any new value)
- b. The new value is bound by the current minimum and maximum (This will accept any internal current value)

#### Internal resource values, `min_action` and `max_action`

Now that a resources' maximum and minimum value can be modified and it's internal value can be different than it's shown value, a problem arises in the execution of the `min_action` and `max_action` fields. Since `enforce_limits` and `retain_value` can modify a resources' current value and how it's changed, the `min_action` and `max_action` fields may behave differently than expected.

For starters, we need to go back to how `retain_value` works, and how resource values are set with the field active.

Again, if `retain_value` is `true`, you can only set the value if

- a. The internal current value is bound by the current minimum and maximum (This will accept any new value), or
- b. The new value is bound by the current minimum and maximum (This will accept any internal current value)

Now, the `min_action` and `max_action` actions are only executed if the **internal** current value has been succeessfully changed and it is equal to minimum or maximum, respectively.

Take note that when the maximum or minimum values are modified and `enforce_limits` is `true`, the `min_action` or `max_action` actions will **not** be executed when the resource is bounded by `enforce_limits`.

Lets take our previous example and say that the value of the resource sub-power is `139`, both `enforce_limits` and `retain_value` are `true` and the player is shifting. Currently, our minimum and maximum values are `1` and `140`, respectively. When we add `1` to the resource, it will have a value of `140`, executing all actions under `max_action`.

When the player isn't shifting, the value of the resource is `75`, and when the player is shifting, the value of the resource is `140`. Even though it's current value is clearly hitting the maximum values, the `max_action` will **not** be executed. It will only be executed if the resources' internal value reaches the current maximum again. Do note that setting the resource to `140` (while shifting) will still not execute `max_action`, since it's value effectively stayed the same. 

Now, if the player isn't shifting, and the value of the resource is set to `75`, `max_action` is executed since it's internal value changes to `75`. (`75` is bound by minimum and maximum, therefore it is set)