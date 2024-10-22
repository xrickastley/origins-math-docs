# Variable Execute Command

[Entity Action Type](../types/entity_action_types.md)

Executes a command with the entity as the source. (i.e. `@s` will select the entity itself).

This is a modified version of `origins:execute_command` that allows you to "inject" variables into the command.

To signify a variable inside the `command` string, the variable name must either be prepended with `$:` or follow the JavaScript [String interpolation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#string_interpolation) syntax. You may use both methods to signify variables in a single command string.

The values for variables are calculated when the command is executed. This allows for changes in variable values to be reflected in the executed command.

Type ID: `origins-math:variable_execute_command`

!!! warning 
	Some Resource values in this mod can be internally stored as a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/), but normal Origin Resource powers have to be stored as an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).
	
	In this [Entity Action Type](../types/entity_action_types.md), floating-point values are [floored](https://en.wikipedia.org/wiki/Floor_and_ceiling_functions) while being used as a value in the `command` string. This is because Minecraft's command system can accept an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/) as a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/), but not the other way around.

### Fields
| Field			| Type		| Default		| Description								|
|---------------|-----------|---------------|-------------------------------------------|
| `command`		|[String](https://origins.readthedocs.io/en/latest/types/data_types/string/)|	| The command to execute on the entity. |
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

