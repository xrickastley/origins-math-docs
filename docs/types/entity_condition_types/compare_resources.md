---
title: Compare Resources (Entity Condition Type)
---

# Compare Resources

[Entity Condition Type](../entity_condition_types.md)

Compares the value of a power that uses the [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/) or a power type that has a built-in cooldown (using remaining ticks as the value) with another power.

Type ID: `origins-math:compare_resources`

### Fields
| Field			| Type		| Default		| Description								|
|---------------|-----------|---------------|-------------------------------------------|
| `left_resource`	|[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)|	| The namespace and ID of the power that uses the [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/) or has a built-in cooldown. |
| `comparison`		|[Comparison](https://origins.readthedocs.io/en/latest/types/data_types/comparison/)|	| Determines how the value of `left_resource` should be compared to `right_resource`. |
| `right_resource`	|[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)|	| The namespace and ID of the power that uses the [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/) or has a built-in cooldown. |

### Examples
```json
"condition": {
    "type": "origins-math:compare_resources",
    "left_resource": "example:a_simple_resource",
    "comparison": ">=",
    "right_resource": "example:another_simple_resource"
}
```
This example will check if the player's `example:a_simple_resource` resource has a greater or equal value to  `example:another_simple_resource` resource.