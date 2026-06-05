---
title: For-range (Meta Action Type)
---

# For-range

[Meta Action Type](../meta_action_types.md)

Executes the provided action for a specified amount of times.

Type ID: `origins-math:for_range`

!!! warning
	The provided action is executed for the specified amount of times on the **same tick**!

### Fields
| Field		| Type		| Default		| Description								|
|-----------|-----------|---------------|-------------------------------------------|
| `range`	|[Resource-backed Integer*](../data_types/resource-backed_integer.md) <br> [Integer*](https://origins.readthedocs.io/en/latest/types/data_types/integer/)|	| The amount of times the provided action will be executed. |
| `action`	|[Action Type](../action_types.md) |  | The action to execute for `range` times in the same tick. |

- A [Resource-backed Integer*](../data_types/resource-backed_integer.md) is accepted for action types of [Bi-entity Action Types](./bientity_action_types.md), [Entity Action Types](./entity_action_types.md) and [Item Action Types](https://origins.readthedocs.io/en/latest/types/item_action_types/), otherwise, only an [Integer*](https://origins.readthedocs.io/en/latest/types/data_types/integer/) is accepted.

### Examples

```json
"entity_action": {
	"type": "origins-math:for_range",
	"range": 5,
	"action": {
		"type": "origins:and",
		"actions": [
			{
				"type": "origins:change_resource",
				"resource": "origins-math:fireball_delay",
				"change": 5
			},
			{
				"type": "origins:delay",
				"ticks": "origins-math:fireball_delay",
				"action": {
					"type": "origins:fire_projectile",
    				"entity_type": "minecraft:fireball",
    				"divergence": 0.25,
    				"count": 1
				}
			}
		]
	}
}
```

This example will fire 5 fireballs at where the entity is facing, each delayed by 5 ticks.

Since the action inside a [For-range (Meta Action Type)](#for-range) is executed on the same tick, we assign a resource: `origins-math:fireball_delay` to hold the delay value, and increment it by 5 every time the action is executed. As a result, we get a delay of 5, 10, 15, 20 and 25, all 5 ticks apart from each other.