---
title: Status Effect Linked Resource (Power Type)
---

# Status Effect Linked Resource

[Power Type](../power_types.md)

Provides a variable who's value is given by the provided value of the selected [Status Effect Property](../data_types/status_effect_property.md) from the specified [Status Effect](https://minecraft.wiki/w/Effect).

Type ID: `origins-math:status_effect_linked_resource`

### Fields
| Field    | Type | Default    | Description |
|----------|------|------------|-------------|
|`effect`  |[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)| | The ID of the Status Effect that this power will base it's value from.|
|`property`|[Status Effect Property](../data_types/status_effect_property.md)| | The specific property of the Status Effect that this power will take as it's value. |

### Examples
```json
{
	"type": "origins-math:status_effect_linked_resource",
	"effect": "minecraft:speed",
	"property": "AMPLIFIER"
}
```
This example will use the amplifier of the Speed effect on the entity as it's value. If the entity doesn't have the Speed effect, this would have a value of `-1`. 