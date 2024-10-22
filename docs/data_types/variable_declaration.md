# Variable Declaration

[Data Type](../types/data_types.md)

An [Object](https://origins.readthedocs.io/en/latest/types/data_types/object/) representing a key-value pair of used names and their associated variables.

```jsonc
"variables": {
	"hunger": "origins-math:hunger_linked_resource",
	"health": "origins-math:health_linked_resource",
	"amplifier": "origins-math:my_resource"
}
```

This represents three variables; `hunger`, which represents the value of the `origins-math:hunger_linked_resource` resource, `health`, which represents the value of the `origins-math:health_linked_resource` resource, and `amplifier`, which represents the value of the `origins-math:my_resource` resource.

All variables here can be used in expressions and inside the `command` string in the [Variable Execute Command (Entity Action Type)](../entity_action_types/variable_execute_command.md)