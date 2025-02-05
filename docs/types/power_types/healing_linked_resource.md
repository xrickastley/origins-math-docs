---
title: Healing Linked Resource (Power Type)
---

# Healing Linked Resource

[Power Type](../power_types.md)

Provides a variable who's value is given by the most recent amount of healing received.

Type ID: `origins-math:healing_linked_resource`

!!! warning 
	Healing values can be stored as a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/), but Resources have to be stored as an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).
	Therefore, you should be cautious when using this power as a resource, as the value will have it's decimal counterpart removed, effectively being [truncated](https://en.wikipedia.org/wiki/Truncation), when doing so.

	If you are using this resource as a variable to either the [Math Resource (Power Type)](../power_types/math_resource.md) or the [Variable Change Resource (Entity Action Type)](../entity_action_types/variable_change_resource.md), then it's [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/) counterpart is used instead.

### Fields
| Field  			| Type | Default    | Description |
|-------------------|------|------------|-------------|
|`duration`			|[Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/)| |The amount of ticks the healing amount is stored.|
|`entity_action`	|[Entity Action Type](../entity_action_types.md)|*optional*| If specified, the specified actions will be executed once the amount of healing received has been set.|

### Examples
```json
{
	"type": "origins-math:healing_linked_resource",
	"duration": 10
}
```
This example will record the most recent amount of healing received for `10` ticks.