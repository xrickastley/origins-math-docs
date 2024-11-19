---
title: Player Linked Resource (Power Type)
---

# Player Linked Resource

[Power Type](../power_types.md)

Provides a variable who's value is given by the player's value for the provided property.

Type ID: `origins-math:player_linked_resource`

### Fields
| Field   | Type | Default    | Description |
|---------|------|------------|-------------|
|`property`|[Player Property](../data_types/player_property.md)| | The specific property of the player that this power will take as it's value. |

### Examples
```json
{
	"type": "origins-math:player_linked_resource",
	"property": "FOOD_LEVEL"
}
```
This example will use the player's food level (hunger) as it's value.
