---
title: Modify Resource Minimum (Power Type)
---

# Modify Resource Minimum

[Power Type](../power_types.md)

Modifies the minimum value of a resource that uses the [Modifiable Resource (Power Type)](./modifiable_resource.md).

Type ID: `origins-math:modify_resource_minimum`

!!! note
	When modifying the minimum value of a [Modifiable Resource (Power Type)](./modifiable_resource.md), the resulting minimum value will have it's decimal counterpart removed, effectively being [truncated](https://en.wikipedia.org/wiki/Truncation). This is because the minimum value of a resource must be as an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).

### Fields

| Field         | Type | Default | Description
|---------------|------|---------|-------------
| `resource`	|[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)|	| The namespace and ID of the power that uses the [Modifiable Resource (Power Type)](./modifiable_resource.md) |
| `modifier`	|[Attribute Modifier](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) | _optional_ | If specified, this modifier will be applied to the resource's minimum value.|
| `modifiers`	|[Array](https://origins.readthedocs.io/en/latest/types/data_types/array/) of [Attribute Modifiers](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) | _optional_ | If specified, these modifiers will be applied to the resource's minimum value.|

### Examples

```json
{
	"type": "origins-math:modify_resource_minimum",
	"condition": {
		"type": "origins:sneaking"
	},
	"modifier": {
		"value": -10,
		"operation": "add_base_early"
	}
}
```

This example will provide a variable with an initial minimum value of `0` and an initial maximum value of `75`. When the user is sneaking, the effective minimum value of the resource would be `-10`. 