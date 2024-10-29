# Modify Resource Maximum

[Power Type](../power_types.md)

Modifies the maximum value of a resource that uses the [Modifiable Resource (Power Type)](./modifiable_resource.md).

Type ID: `origins-math:modify_resource_maximum`

!!! note
	When modifying the maximum value of a [Modifiable Resource (Power Type)](./modifiable_resource.md), the resulting maximum value will have it's decimal counterpart removed, effectively being [truncated](https://en.wikipedia.org/wiki/Truncation). This is because the maximum value of a resource must be as an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).

### Fields

| Field         | Type | Default | Description
|---------------|------|---------|-------------
| `resource`	|[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)|	| The namespace and ID of the power that uses the [Modifiable Resource (Power Type)](./modifiable_resource.md) |
| `modifier`	|[Attribute Modifier](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) | _optional_ | If specified, this modifier will be applied to the resource's maximum value.|
| `modifiers`	|[Array](https://origins.readthedocs.io/en/latest/types/data_types/array/) of [Attribute Modifiers](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) | _optional_ | If specified, these modifiers will be applied to the resource's maximum value.|

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