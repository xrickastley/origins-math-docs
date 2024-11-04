# Relative Resource

[Entity Condition Type](../entity_condition_types.md)

Compares the value of a power that uses the [Modifiable Resource (Power Type)](./modifiable_resource.md), [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/) or a power type that has a built-in cooldown (using remaining ticks as the value) over it's current maximum value to the specified relativity.

| Field			| Type		| Default		| Description								|
|---------------|-----------|---------------|-------------------------------------------|
| `resource`	|[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)|	| The namespace and ID of the power that uses the [Modifiable Resource (Power Type)](../power_types/modifiable_resource.md), [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/) or has a built-in cooldown. |
| `comparison`		|[Comparison](https://origins.readthedocs.io/en/latest/types/data_types/comparison/)|	| Determines how the relativity value of `resource` should be compared to `relativity`. |
| `relativity`		|[Float](https://origins.readthedocs.io/en/latest/types/data_types/float/)|	| The relativity value at which the relativity of the resource will be compared to. |

### Examples
```json
"condition": {
    "type": "origins-math:relative_resource",
    "resource": "example:a_simple_resource",
	"comparison": ">=",
	"relativity": 0.50
}
```
This example will check if the player's `example:a_simple_resource` resource has a value of at least `50%`.