---
title: Variable Execute Command (Bi-entity Action Type)
---

# Variable Execute Command

[Bi-entity Action Type](../bientity_action_types.md)

Executes a command with an actor and target entity selector.

This is a modified version of `origins:execute_command` that allows you to "inject" variables into the command along with entities.

The values for variables are calculated when the command is executed. This allows for changes in variable values to be reflected in the executed command.

Type ID: `origins-math:variable_execute_command`

### Fields
| Field				| Type		| Default		| Description								|
|-------------------|-----------|---------------|-------------------------------------------|
| `command`			|[Variable String § Command](../data_types/variable_string.md#command)|	| The command to execute on the entity. |
| `actor_selector`	|[String](https://origins.readthedocs.io/en/latest/types/data_types/string/)|`%a`| The substring that refers to the actor entity. **All** instances of this string will be replaced with the actor entity's UUID. |
| `target_selector`	|[String](https://origins.readthedocs.io/en/latest/types/data_types/string/)|`%t`| The substring that refers to the target entity. **All** instances of this string will be replaced with the target entity's UUID. |
| `variables`		|[Variable Declaration](../data_types/variable_declaration.md)|*optional*| A variable declaration declaring variables used inside the command. 	|

### Examples
```json
"bientity_action": {
	"type": "origins-math:variable_execute_command",
	"command": "/damage @t $:dmgMultiplier minecraft:freeze by @a",
	"actor_selector": "@a",
	"target_selector": "@t",
	"variables": {
		"dmgMultiplier": "origins-math:damage_multiplier"
	}
}
```
The example above will deal Freeze DMG to the target, with it's amount being the value of the **actor's** `origins-math:damage_multiplier` resource.

For example, if the `origins-math:damage_multiplier` resource power has a value of `10`, then the `shield_amount` variable would also have a value of `10`. Therefore, our resulting final command would be `/damage <targetUUID> 10 minecraft:freeze by <actorUUID>`.