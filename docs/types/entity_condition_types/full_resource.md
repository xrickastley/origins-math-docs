# Full Resource

[Entity Condition Type](../entity_condition_types.md)

Checks if the value of a power that uses the [Modifiable Resource (Power Type)](../power_types/modifiable_resource.md), [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/) or a power type that has a built-in cooldown (using remaining ticks as the value) is equal or greater than it's maximum value.

| Field			| Type		| Default		| Description								|
|---------------|-----------|---------------|-------------------------------------------|
| `resource`	|[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)|	| The namespace and ID of the power that uses the  [Modifiable Resource (Power Type)](../power_types/modifiable_resource.md), [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/) or has a built-in cooldown. |

### Examples
```json
"condition": {
    "type": "origins-math:full_resource",
    "resource": "example:a_simple_resource"
}
```
This example will check if the player's `example:a_simple_resource` resource has a value equal to it's maximum value.