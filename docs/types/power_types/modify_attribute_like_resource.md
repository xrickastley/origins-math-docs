---
title: Modify Attribute-like Resource (Power Type)
---

# Modify Attribute-like Resource

[Power Type](../power_types.md)

Creates an global entity-linked modifier for a resource that uses the [Attribute-like Resource (Power Type)](./attribute_like_resource.md)

Type ID: `origins-math:modify_attribute_like_resource`

### Fields
| Field   | Type | Default    | Description |
|---------|------|------------|-------------|
| `resource`	|[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)|	| The namespace and ID of the power that uses the [Attribute-like Resource (Power Type)](./attribute_like_resource.md) |
| `modifier`	|[Attribute Modifier](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) | _optional_ | If specified, this modifier will be applied to the resource' added values.|
| `modifiers`	|[Array](https://origins.readthedocs.io/en/latest/types/data_types/array/) of [Attribute Modifiers](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) | _optional_ | If specified, these modifiers will be applied to the resource's added values.|

### Examples
```json
{
	"type": "origins:multiple",

	"resource": {
		"type": "origins-math:attribute_like_resource",
		"min": 0,
		"max": 80
	},

	"sneak_modifier": {
		"type": "origins-math:modify_attribute_like_resource",
		"conditions": {
			"type": "origins:sneaking"
		},
		"resource": "*:*_resource",
		"modifier": {
			"operation": "multiply_base_multiplicative",
			"value": 0.5
		}
	},

	"rain_modifier": {
		"type": "origins-math:modify_attribute_like_resource",
		"conditions": {
			"type": "origins:is_rain"
		},
		"resource": "*:*_resource",
		"modifier": {
			"operation": "multiply_base_multiplicative",
			"value": 0.25
		}
	}
}
```
This example creates an `resource` [Attribute-like Resource](./attribute_like_resource.md) sub-power that is modified by both the `sneak_modifier` and the `rain_modifier` [Modify Attribute-like Resource](./modify_attribute_like_resource.md) sub-powers. 

When the player is sneaking **and** it is **not** raining at the entity's position, **all** added values are multiplied by `1.5`. If it is **also** raining at the entity's position, all added values are multiplied by `1.875` (1.25 × 1.5). If it is **only** raining at the entity's position, all added values are multiplied by `1.25`.

!!! note
	In order for the modifiers to apply properly, you **must** use the resource changing methods provided by **Origins: Math** like [Change Resource (Entity Action Type)](../entity_action_types/change_resource.md) or the [`/resource change absolute`](../../misc/commands/resource.md) command.