# Current Biome Linked Resource

[Power Type](../power_types.md)

Provides a variable who's value is given by the current biome's value for the provided property.

Type ID: `origins-math:current_biome_linked_resource`

### Fields
| Field   | Type | Default    | Description |
|---------|------|------------|-------------|
|`property`|[Biome Property](../data_types/biome_property.md)| | The specific property of the current biome that this power will take as it's value. |

### Examples
```json
{
	"type": "origins-math:current_biome_linked_resource",
	"property": "TEMPERATURE"
}
```
This example will use the power holders' current biome's temperature as it's value.
