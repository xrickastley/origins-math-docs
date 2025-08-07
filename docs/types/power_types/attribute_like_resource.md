---
title: Attribute-like Resource (Power Type)
---

# Attribute-like Resource

[Power Type](../power_types.md)

An extension of the [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/), provides a variable whose changed values can be *globally* modified through the [Modify Attribute-like Resource (Power Type)](./modify_attribute_like_resource.md).

Upon **adding** a value to this resource, it is modified with all [Modify Attribute-like Resource](./modify_attribute_like_resource.md) powers on the entity targetting this resource, allowing it to act as an "attribute" with increasable stats. However, **setting** a value to this resource will **not** apply the specified modifiers.

Type ID: `origins-math:attribute_like_resource`

### Fields
| Field   | Type | Default    | Description |
|---------|------|------------|-------------|
|`min`|[Float](https://origins.readthedocs.io/en/latest/types/data_types/float/)| | The minimum value of the resource. |
|`max`|[Float](https://origins.readthedocs.io/en/latest/types/data_types/float/)| | The maximum value of the resource. |
|`start_value`|[Float](https://origins.readthedocs.io/en/latest/types/data_types/float/)| | The value of the resource when the entity first receives the power. If not set, this will be set to the value of the `min` float field. |

### Examples
```json
{
	"type": "origins:multiple",

	"energy_resource": {
		"type": "origins-math:attribute_like_resource",
		"min": 0,
		"max": 80
	},

	"sneak_modifier": {
		"type": "origins-math:modify_attribute_like_resource",
		"conditions": {
			"type": "origins:sneaking"
		},
		"resource": "*:*_energy_resource",
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
		"resource": "*:*_energy_resource",
		"modifier": {
			"operation": "multiply_base_multiplicative",
			"value": 0.25
		}
	}
}
```
This example creates a simple "energy system" through the `energy_resource` [Attribute-like Resource](#attribute-like-resource) sub-power, modified by both the `sneak_modifier` and the `rain_modifier` [Modify Attribute-like Resource](./modify_attribute_like_resource.md) sub-powers. 

When the player is sneaking **and** it is **not** raining at the entity's position, **all** received "energy" is multiplied by a factor of `1.5`. If it is **also** raining at the entity's position, all received "energy" is multiplied by a factor of `1.875` (1.25 × 1.5). If it is **only** raining at the entity's position, all received "energy" is multiplied by a factor of `1.25`.

!!! note
	In order for the modifiers to apply properly, you **must** use the resource changing methods provided by **Origins: Math** like [Change Resource (Entity Action Type)](../entity_action_types/change_resource.md) or the [`/resource change absolute`](../../misc/commands/resource.md) command.
