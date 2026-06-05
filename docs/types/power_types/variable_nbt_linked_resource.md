---
title: Variable Nbt-linked Resource (Power Type)
---

# Variable Nbt-linked Resource

[Power Type](../power_types.md)

Provides a variable who's value is given by the entity's value for the provided NBT path.

Type ID: `origins-math:variable_nbt_linked_resource`

!!! warning
	The provided NBT path **must** only be a **single** element that corresponds to a numerical value. If a non-numerical value is used, an error is logged and `0` is taken as the value instead.
	
### Fields
| Field			| Type | Default    | Description |
|---------------|------|------------|-------------|
|`path`			|[Variable String § NBT Path](../data_types/variable_string.md#nbt-path)| | The specific NBT path that this power will take as it's value. |
| `variables`	|[Variable Declaration](../data_types/variable_declaration.md)|*optional*| A variable declaration declaring variables used inside the command. 	|

### Examples

```json
{
	"type": "origins-math:variable_nbt_linked_resource",
	"path": "foo.bar.nums[${index}]",
	"variables": {
		"index": "origins-math:current_array_index"
	}
}
```

This example will use the the n-th element of the `foo.bar.nums` NBT array, provided by the variable `index`, as it's value.