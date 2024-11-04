# Modify Simple

[Power Type](../power_types.md)

A power that can store [Attribute Modifiers](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/#attribute_modifier). This is normally used as a debugging tool for the `/modifier` command. Other than that, it doesn't have any other use.

Type ID: `origins-math:modify_simple`

### Fields

| Field         | Type | Default | Description
|---------------|------|---------|-------------
| `modifier`	|[Attribute Modifier](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) | _optional_ | If specified, this modifier will be applied to the resource's minimum value.|
| `modifiers`	|[Array](https://origins.readthedocs.io/en/latest/types/data_types/array/) of [Attribute Modifiers](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/) | _optional_ | If specified, these modifiers will be applied to the resource's minimum value.|

### Examples

```json
{
	"type": "origins-math:modify_simple",
	"modifier": {
		"value": -10,
		"operation": "add_base_early"
	}
}
```

This example will create a modifier that subtracts `10` from it's value. You can test the modifier using the `/modifier apply` command. 