---
title: Living Entity Linked Resource (Power Type)
---

# Living Entity Linked Resource

[Power Type](../power_types.md)

Provides a variable who's value is given by a living entity's value for the provided property.

Type ID: `origins-math:living_entity_linked_resource`

### Fields
| Field   | Type | Default    | Description |
|---------|------|------------|-------------|
|`property`|[Living Entity Property](../data_types/living_entity_property.md)| | The specific property of the entity that this power will take as it's value. |

### Examples
```json
{
	"type": "origins-math:living_entity_linked_resource",
	"property": "breathing"
}
```
This example will use the entity's breath ("air bubbles") as it's value.