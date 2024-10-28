# Attribute Linked Resource

[Power Type](../power_types.md)

Provides a variable who's value is given by the provided value of the selected [Attribute Value](../data_types/attribute_value.md) from the specified [Attribute](https://minecraft.wiki/w/Attribute).

Type ID: `origins-math:attribute_linked_resource`

!!! warning 
	Attribute values are normally stored as a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/), but Resources have to be stored as an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).
	Therefore, you should be cautious when using this power as a resource, as the value will have it's decimal counterpart removed, effectively being [truncated](https://en.wikipedia.org/wiki/Truncation), when doing so.

	If you are using this resource as a variable to either the [Math Resource (Power Type)](../power_types/math_resource.md) or the [Variable Change Resource (Entity Action Type)](../entity_action_types/variable_change_resource.md), then it's [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/) counterpart is used instead.

### Fields
| Field   | Type | Default    | Description |
|---------|------|------------|-------------|
|`attribute`|[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)| | The ID of the attribute that this power will base it's value from.|
|`value`    |[Attribute Value](../data_types/attribute_value.md)| `"TOTAL"` | The specific value of `"attribute"` this power will take. |

### Examples
```json
{
	"type": "origins-math:attribute_linked_resource",
	"attribute": "minecraft:generic.attack_speed",
	"value": "TOTAL"
}
```
This example will use the player's current attack speed as it's value.

```json
{
	"type": "origins-math:attribute_linked_resource",
	"attribute": "minecraft:generic.attack_speed",
	"value": "BASE"
}
```
This example will use the player's base attack speed as it's value, which is `4`.