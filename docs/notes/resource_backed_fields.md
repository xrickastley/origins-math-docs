# Resource-backed fields

When you start up Minecraft with logs enabled, you might've seen the following in the logs.

```
[Render Thread/INFO] (origins-math/ResourceBackedInjector) Creating ResourceBacked factory (origins-math:status_effect) for apoli:status_effect
[Render Thread/INFO] (origins-math/ResourceBackedInjector) Cancelling creation of ResourceBacked factory for apoli:submerged_in (same SerializableData instances)
```

This is what Origins: Math does behind-the-scenes for you to be able to use Resources in [Entity Action Types](../types/entity_action_types.md) or [Entity Condition Types](../types/entity_condition_types.md) directly!

For every [Entity Action Type](../types/entity_action_types.md) or [Entity Condition Type](../types/entity_condition_types.md) that gets added, Origins: Math can create a modified version of it that allows you to use Resources directly into [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/) or [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/) values. 

You may have noticed that there are two versions of the message:

- "Creating ResourceBacked factory...", and  
- "Cancelling creation of ResourceBacked factory..."

To prevent redundant types, a ResourceBacked factory is only created if the type in question takes an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/) or a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/). This is because Origins: Math allows for you to use Resource values in place of numbers, therefore, it would be pointless if a ResourceBacked factory is created for a type that doesn't even take a number.

When a ResourceBacked factory is created for a type, it will always be under the `origins-math` namespace. This is done to avoid confusion when others read your code, as it would normally be unusual for a Resource to be the value for a numerical field.

However, this little caveat may cause an issue when Origin addons add power types with the same path: ex: `origins:change_resource` and `example-mod:change_resource`. Luckily, Origins: Math uses a specific naming convention for creating ResourceBacked factories that directly prevent this issue.

- If the type is from Apoli or Origins, the resulting ResourceBacked factory will be `origins-math:<type>`
	- ex. `origins:delay` to `origins-math:delay`
- If the type is not from Apoli or Origins, the resulting ResourceBacked factory will be `origins-math:<modid>/<type>`
	- ex. `eggolib:change_health` to `origins-math:eggolib/change_health`

A ResourceBacked factory's [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/) and [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/) fields can take the value of Resources as it's value. For example, let's take a look at `origins:heal` and it's ResourceBacked version: `origins-math:heal`.

```json
{
	"type": "origins:heal",
	"amount": 4
}
```

The example above will heal the entity for `2` hearts. Normally, this alone can suffice for a lot of Origins, but what if you'd want to heal based on the value of a Resource? Well, you'd end up doing something like this:

```json
{
	"type": "origins:if_else_list",
	"actions": [
		{
			"condition": {
				"type": "origins:resource",
				"resource": "example:healing_gauge",
				"comparison": "==",
				"compare_to": 1
			},
			"action": {
				"type": "origins:heal",
				"amount": 1
			}
		},
		{
			"condition": {
				"type": "origins:resource",
				"resource": "example:healing_gauge",
				"comparison": "==",
				"compare_to": 2
			},
			"action": {
				"type": "origins:heal",
				"amount": 2
			}
		},
		// and so on and so forth...
	]
}
```

While this isn't a bad solution to the problem, it's quite large and can be hard to maintain as you go on. Additionally, our solution would be very large for a power that just *heals* based on the value of a Resource, how longer would it be if our power is more complex?

This is where the ResourceBacked version for `origins:heal` comes in. Instead of a big [If-Else List](https://origins.readthedocs.io/en/latest/types/meta_action_types/if_else_list/) chain, you can simply use the Resource as the value for the healing directly!

```json
{
	"type": "origins-math:heal",
	"amount": "example:healing_gauge"
}
```

The example above will heal the entity using the value of the `example:healing_gauge` Resource, removing the need for a big [If-Else List](https://origins.readthedocs.io/en/latest/types/meta_action_types/if_else_list/) chain.