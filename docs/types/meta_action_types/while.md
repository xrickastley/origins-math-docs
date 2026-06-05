---
title: While (Meta Action Type)
---

# While

[Meta Action Type](../meta_action_types.md)

Executes the provided action while the specified condition is true.

Type ID: `origins-math:while`

!!! warning
	The provided action is executed for the specified amount of times on the **same tick**!

	As a result, the maximum amount of times an action can be executed inside a single [While (Meta Action Type)](#while) is 100,000. Once this is reached, the loop is prematurely terminated.
	
### Fields
| Field			| Type									| Default | Description									|
|---------------|---------------------------------------|---------|---------------------------------------------|
| `condition`	|[Condition Type](../condition_types.md)|		  | The condition that must be fulfilled for `action` to be executed. |
| `action`		|[Action Type](../action_types.md) 		|		  | The action to execute while `condition` is true. |

### Examples
```json
"entity_action": {
	"type": "origins-math:while",
	"condition": {
		"type": "origins:saturation_level",
		"comparison": ">",
		"compare_to": 0
	},
	"action": {
		"type": "origins:and",
		"actions": [
			{
				"type": "origins:exhaust",
				"amount": 4.0
			},
			{
				"type": "origins:change_resource",
				"resource": "origins-math:teleport_delay",
				"change": 30
			},
			{
				"type": "origins:delay",
				"ticks": "origins-math:teleport_delay",
				"action": {
    				"type": "origins:random_teleport",
    				"area_width": 8,
    				"area_height": 8,
    				"landing_offset": {
    				    "x": 0.5,
    				    "z": 0.5
    				}
				}
			}
		]
	}
}
```

This example will use all the player's saturation to teleport randomly a certain amount of times within a 17x17x17 area for an interval of 1.5 seconds each, where the amount of random teleports is directly proportional to the player's saturation when the action is executed.