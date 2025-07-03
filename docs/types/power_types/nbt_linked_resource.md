---
title: Nbt-linked Resource (Power Type)
---

# Nbt-linked Resource

[Power Type](../power_types.md)

Provides a variable who's value is given by the entity's value for the provided NBT path.

Type ID: `origins-math:nbt_linked_resource`

!!! warning
	The provided NBT path **must** only be a **single** element that corresponds to a numerical value. If a non-numerical value is used, an error is logged and `0` is taken as the value instead.
	
### Fields
| Field   | Type | Default    | Description |
|---------|------|------------|-------------|
|`path`|[NBT Path](../data_types/nbt_path.md)| | The specific NBT path that this power will take as it's value. |

### Examples

```json
{
	"type": "origins-math:nbt_linked_resource",
	"path": "foo.num"
}
```

This example will use the `foo.num` NBT element as it's value.

```json
{
	"type": "origins-math:nbt_linked_resource",
	"path": "foo.bar.nums[0]"
}
```


This example will use the first element of the `foo.bar.nums` NBT array as it's value.