---
title: Change Resource (Entity Action Type)
---

# Change Resource

[Entity Action Type](../entity_action_types.md)

Changes the value of a power that either uses the [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/) or it's extensions, or has a built-in cooldown.

This is a modified version of `origins:change_resource` that accepts floating-point numbers and works for specific **Origins: Math** contexts such as [Attribute-like Resource](../power_types/attribute_like_resource.md).

Type ID: `origins-math:change_resource`

!!! warning 
	Some Resource values in this mod can be internally stored as a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/), but normal Origin Resource powers have to be stored as an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).
	
	In this [Entity Action Type](../entity_action_types.md), floating-point values is preserved as much as possible. However, for standard Origin Resource powers, the value is most likely to have it's decimal counterpart removed, effectively being [truncated](https://en.wikipedia.org/wiki/Truncation) since it will be added to/set as the value of the actor's resource.

### Fields
| Field			| Type		| Default		| Description								|
|---------------|-----------|---------------|-------------------------------------------|
| `resource`	|[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)|	| The namespace and ID of the power that uses the [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/) or it's extensions, or has a built-in cooldown. |
| `change`      |[Resource-backed Float](../data_types/resource-backed_float.md) |  | This value will be added to the resource (won't go below `min` or above `max` of the [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/)). |
| `operation`	|[String](https://origins.readthedocs.io/en/latest/types/data_types/string/)|`"add"`| Determines if the action should add or set the value of the resource. Accepts `"add"` or `"set"`.	|

### Examples
```json
"entity_action": {
    "type": "origins:change_resource",
    "resource": "namespace:example",
    "change": 1
}
```

This example will add 1 to the `namespace:example` (`data/namespace/powers/example.json`) power that uses the [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/).