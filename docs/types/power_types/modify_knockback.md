---
title: Modify Knockback (Power Type)
---

# Modify Knockback

[Power Type](../power_types.md)

Modifies the knockback received in a specified axis.

Type ID: `origins-math:modify_attribute_like_resource`

### Fields
| Field   | Type | Default    | Description |
|---------|------|------------|-------------|
| `x` | [Attribute Modifier](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) or [Array](https://origins.readthedocs.io/en/latest/types/data_types/array/) of [Attribute Modifiers](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) | *optional* | The modifiers to apply to the knockback on the x-axis. |
| `y` | [Attribute Modifier](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) or [Array](https://origins.readthedocs.io/en/latest/types/data_types/array/) of [Attribute Modifiers](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) | *optional* | The modifiers to apply to the knockback on the y-axis. |
| `z` | [Attribute Modifier](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) or [Array](https://origins.readthedocs.io/en/latest/types/data_types/array/) of [Attribute Modifiers](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) | *optional* | The modifiers to apply to the knockback on the z-axis. |


### Examples
```json
{
	"type": "origins-math:modify_knockback",
	"y": {
		"operation": "set_total",
		"value": 0
	}
}
```
This example removes knockback on the y-axis. 

```json
{
	"type": "origins-math:modify_knockback",
	"x": {
		"operation": "multiply_total_multiplicative",
		"value": -0.5
	},
	"y": {
		"operation": "set_total",
		"value": 0
	},
	"z": {
		"operation": "multiply_total_multiplicative",
		"value": -0.5
	}
}
```
This example nullifies knockback on the y-axis while reducing knockback on the x and z axes by 50%.