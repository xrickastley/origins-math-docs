---
title: Resource-backed Float (Data Type)
---

# Resource-backed Float

[Data Type](../data_types.md)

A [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/) or [Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/) that links to a [Modifiable Resource (Power Type)](../power_types/modifiable_resource.md), [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/) or a power type that has a built-in cooldown (using remaining ticks as the value).

### Examples

```json
"entity_action": {
	"type": "origins-math:heal",
	"amount": 5.0
}
```
A [Heal (Entity Action Type)](https://origins.readthedocs.io/en/latest/types/entity_action_types/heal/) with an `amount` value of `5.0`, which is a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/).

```json
"entity_action": {
	"type": "origins-math:heal",
	"amount": "origins-math:healing_gauge"
}
```
A [Heal (Entity Action Type)](https://origins.readthedocs.io/en/latest/types/entity_action_types/heal/) with an `amount` value using the value of the `"origins-math:healing_gauge"` power.