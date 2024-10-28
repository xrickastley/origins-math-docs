# Variable Change Resource

[Entity Action Type](../entity_action_types.md)

Changes the value of a power that either uses the Resource power type, or has a built-in cooldown.

This is a modified version of `origins:change_resource` that allows you to use an expression with variables as the change value.

The expression is calculated when this [Entity Action Type](../entity_action_types.md) is executed. This allows for changes in variable values to be reflected in the expression.

Type ID: `origins-math:variable_change_resource`

!!! warning 
	Some Resource values in this mod can be internally stored as a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/), but normal Origin Resource powers have to be stored as an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).
	
	In this [Entity Action Type](../entity_action_types.md), the resulting value of `expression` will have it's decimal counterpart removed, effectively being [truncated](https://en.wikipedia.org/wiki/Truncation) since it will be added to/set as the value of the target resource. However, all Resource values that are used in `expression` have their original values unchanged.

### Fields
| Field			| Type		| Default		| Description								|
|---------------|-----------|---------------|-------------------------------------------|
| `resource`	|[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)|	| The namespace and ID of the power that uses the [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/) or has a built-in cooldown. |
| `expression`	|[Expression](../data_types/expression.md)| |The mathematical expression that gives the value that will be added to/set as the value (won't go below `min` or above `max` of the [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/)).|
| `variables`	|[Variable Declaration](../data_types/variable_declaration.md)|*optional*| A variable declaration declaring variables used inside the command. 	|
| `operation`	|[String](https://origins.readthedocs.io/en/latest/types/data_types/string/)|`"add"`| Determines if the action should add or set the value of the resource. Accepts `"add"` or `"set"`.	|

### Examples
```json
"entity_action": {
	"type": "origins-math:variable_change_resource",
	"resource": "origins-math:my_resource",
	"expression": "sqrt(my_variable / 2)",
	"variables": {
		"my_variable": "origins-math:my_other_resource"
	}
}
```
The example above will add the truncated value of the expression `"sqrt(my_variable / 2)"` to the `origins-math:my_resource` Resource, with `my_variable` corresponding to the `origins-math:my_other_resource` Resource.