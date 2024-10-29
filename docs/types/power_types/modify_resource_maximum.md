# Modify Resource Maximum

[Power Type](../power_types.md)

Modifies the maximum value of a resource that uses the [Modifiable Resource (Power Type)](./modifiable_resource.md).

Type ID: `origins-math:modify_resource_maximum`

!!! note
    When modifying the maximum value of a [Modifiable Resource (Power Type)](./modifiable_resource.md), the resulting maximum value will have it's decimal counterpart removed, effectively being [truncated](https://en.wikipedia.org/wiki/Truncation). This is because the maximum value of a resource must be as an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).

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
	"type": "origins-math:modify_resource_maximum",
	"condition": {
		"type": "origins:sneaking"
	},
	"modifier": {
		"value": 65,
		"operation": "add_base_early"
	}
}
```

This example will provide a variable with an initial minimum value of `0` and an initial maximum value of `75`. When the user is sneaking, the effective maximum value of the resource would be `140`. 