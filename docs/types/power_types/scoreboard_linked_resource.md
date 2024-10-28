# Scoreboard Linked Resource

[Power Type](../power_types.md)

Provides a variable who's value is given by the player's score for the provided scoreboard objective.

Type ID: `origins-math:scoreboard_linked_resource`

### Fields
| Field      | Type | Default    | Description |
|------------|------|------------|-------------|
|`objective` |[String](https://origins.readthedocs.io/en/latest/types/data_types/string/)| | The name of the scoreobard objective to retrieve the score of this entity from. |

### Examples
```json
{
	"type": "origins-math:scoreboard_linked_resource",
	"objective": "my_objective"
}
```
This example will use the player's score in the `my_objective` scoreboard objective as it's value. 