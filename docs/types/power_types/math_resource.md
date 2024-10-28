# Math Resource

[Power Type](../power_types.md)

Provides a variable who's value is given by the value of the provided [Expression](../data_types/expression.md).

Type ID: `origins-math:math_resource`

!!! warning 
	Expression values can be stored as a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/), but Resources have to be stored as an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).
	Therefore, you should be cautious when using this power as a resource, as the value will have it's decimal counterpart removed, effectively being [truncated](https://en.wikipedia.org/wiki/Truncation), when doing so.

	If you are using this resource as a variable to either the [Math Resource (Power Type)](../power_types/math_resource.md) or the [Variable Change Resource (Entity Action Type)](../entity_action_types/variable_change_resource.md), then it's [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/) counterpart is used instead.

### Fields
| Field   | Type | Default    | Description |
|---------|------|------------|-------------|
|`expression`|[Expression](../data_types/expression.md)| |The mathematical expression that gives the value of this resource.|
|`variables`|[Variable Declaration](../data_types/variable_declaration.md)|*optional*| A variable declaration declaring variables used in the expression. 	|

### Examples
```json
{
	"type": "origins-math:math_resource",
	"expression": "9 + 10"
}
```
This example will use the resulting value of `9 + 10` as it's value, which is `19`.

```json
{
	"type": "origins-math:math_resource",
	"expression": "2(abs(amplifier))",
	"variables": {
		"amplifier": "origins-math:my_awesome_resource"
	}
}
```
This example will use the resulting value of `2(abs(amplifier))` as it's value, with the value of `amplifier` being the value of the `origins-math:my_awesome_resource` resource.