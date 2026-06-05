---
title: Variable Execute Command (Entity Action Type)
---

# Variable Execute Command

[Entity Action Type](../entity_action_types.md)

Executes a command with the entity as the source. (i.e. `@s` will select the entity itself).

This is a modified version of `origins:execute_command` that allows you to "inject" variables into the command.

The values for variables are calculated when the command is executed. This allows changes in variable values to be reflected in the executed command.

Type ID: `origins-math:variable_execute_command`

### Fields
| Field			| Type		| Default		| Description								|
|---------------|-----------|---------------|-------------------------------------------|
| `command`		|[Variable String § Command](../data_types/variable_string.md#command)|	| The command to execute on the entity. |
| `variables`	|[Variable Declaration](../data_types/variable_declaration.md)|*optional*| A variable declaration declaring variables used inside the command. 	|

### Examples
```json
"entity_action": {
	"type": "origins-math:variable_execute_command",
	"command": "/effect give @s minecraft:absorption 10 $:shield_amount",
	"variables": {
		"shield_amount": "origins-math:health_shield_multiplier"
	}
}
```
The example above will execute an `/effect` command that will apply the Absorption effect for 10 seconds with an amplifier value of `shield_amount`, who's value would be the value of the `origins-math:health_shield_multiplier` resource power.

For example, if the `origins-math:health_shield_multiplier` resource power has a value of `3`, then the `shield_amount` variable would also have a value of `3`. Therefore, our resulting final command would be `/effect give @s minecraft:absorption 10 3`, applying the Absorption IV effect.

