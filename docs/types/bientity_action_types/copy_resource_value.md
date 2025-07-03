---
title: Copy Resource Value (Bi-entity Action Type)
---

# Copy Resource Value

[Bi-entity Action Type](../bientity_action_types.md)

Copies the target's resource power value onto the specified actor's resource power.

Type ID: `origins-math:copy_resource_value`

!!! warning 
	Some Resource values in this mod can be internally stored as a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/), but normal Origin Resource powers have to be stored as an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).
	
	In this [Bi-entity Action Type](../bientity_action_types.md), floating-point values is preserved as much as possible. However, for standard Origin Resource powers, the value is most likely to have it's decimal counterpart removed, effectively being [truncated](https://en.wikipedia.org/wiki/Truncation) since it will be added to/set as the value of the actor's resource.

### Fields
| Field			| Type		| Default		| Description								|
|---------------|-----------|---------------|-------------------------------------------|
| `target_resource`	|[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)|		| The target's resource power with the value to be copied. |
| `actor_resource`	|[Identifier](https://origins.readthedocs.io/en/latest/types/data_types/identifier/)|		| The actor's resource power the target's resource value is copied over to. |
| `operation`		|[String](https://origins.readthedocs.io/en/latest/types/data_types/string/)		|`"add"`| Determines if the action should add or set the value of the resource. Accepts `"add"` or `"set"`.	|

### Examples
```json
"bientity_action": {
	"type": "origins-math:copy_resource_value",
	"actor_resource": "*:*_copy_target",
	"target_resource": "custom:health_resource",
	"operaton": "set"
}
```
The example above will copy and set the target's `custom:health_resource` power value onto the actor's `copy_target` power.

```json
"bientity_action": {
	"type": "origins-math:copy_resource_value",
	"actor_resource": "*:*_copy_target",
	"target_resource": "custom:health_resource",
	"operaton": "add"
}
```
The example above will copy and add the target's `custom:health_resource` power value onto the actor's `copy_target` power.