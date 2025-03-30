---
title: Damage Dealt Linked Resource (Power Type)
---

# Damage Dealt Linked Resource

[Power Type](../power_types.md)

Provides a variable who's value is given by the most recent amount of damage dealt to another entity.

The amount of damage recorded by this resource is the **final** amount of damage dealt, meaning that all possible modifiers such as Protection, Resistance, Modifiers, and etc. have all been accounted for.

Type ID: `origins-math:damage_dealt_linked_resource`

!!! warning 
	Damage values can be stored as a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/), but Resources have to be stored as an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).
	Therefore, you should be cautious when using this power as a resource, as the value will have it's decimal counterpart removed, effectively being [truncated](https://en.wikipedia.org/wiki/Truncation), when doing so.

	If you are using this resource as a variable to either the [Math Resource (Power Type)](../power_types/math_resource.md) or the [Variable Change Resource (Entity Action Type)](../entity_action_types/variable_change_resource.md), then it's [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/) counterpart is used instead.

### Fields
| Field  			| Type | Default    | Description |
|-------------------|------|------------|-------------|
|`duration`			|[Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/)| |The amount of ticks the damage amount is stored for. |
|`damage_condition`	|[Damage Condition Type](https://origins.readthedocs.io/en/latest/types/data_types/integer/)|*optional*| If specified, the amount of damage dealt is only set if the damage being dealt fulfills the condition. 	|
|`bientity_action`	|[Bi-entity Action Type](../bientity_action_types.md)|*optional*| If specified, the specified actions will be executed once the amount of damage dealt has been set, with the **'actor'** being the entity **dealing** the damage and the **'target'** being the entity that **receives** the damage.|

### Examples
```json
{
	"type": "origins-math:damage_dealt_linked_resource",
	"duration": 5,
	"damage_condition": {
		"type": "origins:in_tag",
		"tag": "minecraft:is_fire"
	}
}
```
This example will record the most recent amount of Fire damage dealt for `5` ticks. After `5` ticks have passed, the value is discarded, making this resource have a value of `0`.