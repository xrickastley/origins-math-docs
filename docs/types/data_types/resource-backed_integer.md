---
title: Resource-backed Integer (Data Type)
---

# Resource-backed Integer

[Data Type](../data_types.md)

An [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/) or [Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/) that links to a [Modifiable Resource (Power Type)](../power_types/modifiable_resource.md), [Resource (Power Type)](https://origins.readthedocs.io/en/latest/types/power_types/resource/) or a power type that has a built-in cooldown (using remaining ticks as the value).

### Examples

```json
"entity_action": {
	"type": "origins-math:set_on_fire",
	"duration": 6
}
```
A [Set On Fire (Entity Action Type)](https://origins.readthedocs.io/en/latest/types/entity_action_types/set_on_fire/) with a `duration` value of `6`, which is an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).

```json
"entity_action": {
	"type": "origins-math:set_on_fire",
	"duration": "origins-math:fire_ticks"
}
```
A [Set On Fire (Entity Action Type)](https://origins.readthedocs.io/en/latest/types/entity_action_types/set_on_fire/) with a `duration` value using the value of the `"origins-math:fire_ticks"` power.