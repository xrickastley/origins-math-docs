# Resource-backed fields

### This is too long! Is there a summary?

Basically, Resource-backed fields are a core feature of **Origins: Math** that allows you to use resources in specified Action and Condition types.

To avoid naming conflicts and to improve readability (it would be very confusing for someone else to see a Resource ID in a normally-`int`/`float` field!), the "Origins: Math" version of an Action/Condition Type would be `origins-math:<type>` for standard types in Apoli/Origins and `origins-math:<modid>/<type>` for types added in by other Origins mods. You **must** use the "Origins: Math" version as using the original version will result in your power file being invalid!

Resources are evaluated based on the entity executing the action, which we will call the "*evaluator*". For Entity Action/Condition Types, the *evaluator* is the entity the action is performed on. For Bi-entity Action/Condition Types, the *evaluator* is the **actor** entity, and for Item Action/Condition Types, the *evaluator* is the **holder** of the `ItemStack`. Do note that if: the *evaluator* does **not exist** (possible in Item Action/Condition Types) or if the *evaluator* does **not have the requested resource**, a value of `0` is supplied instead.

When debugging why some resources don't work, it is **imperative** that you know "**Which entity is the action/condition being evaluated on?**". For more information, you may refer to the ["Invalid" resource values](#invalid-resource-values) section, as this caveat is explained in great detail and should be useful in debugging this issue.

### Introduction

When you start up Minecraft with logs enabled, you might've seen the following in the logs.

```
[Render Thread/INFO] (origins-math/ResourceBackedInjector) Creating ResourceBacked factory (origins-math:status_effect) for apoli:status_effect
[Render Thread/INFO] (origins-math/ResourceBackedInjector) Cancelling creation of ResourceBacked factory for apoli:submerged_in (same SerializableData instances)
```

This is what Origins: Math does behind-the-scenes for you to be able to use Resources in [Entity Action Types](../types/entity_action_types.md) or [Entity Condition Types](../types/entity_condition_types.md) directly!

### Creation

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

### Example usage

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

While this isn't a bad solution to the problem, it's quite large and can be hard to maintain as you go on. Additionally, if our solution is already very large for a power that just *heals* based on the value of a Resource, how much longer would it be if our power is more complex?

This is where the ResourceBacked version for `origins:heal` comes in. Instead of a big [If-Else List](https://origins.readthedocs.io/en/latest/types/meta_action_types/if_else_list/) chain, you can simply use the Resource as the value for the healing directly!

```json
{
	"type": "origins-math:heal",
	"amount": "example:healing_gauge"
}
```

The example above will heal the entity using the value of the `example:healing_gauge` Resource, removing the need for a big [If-Else List](https://origins.readthedocs.io/en/latest/types/meta_action_types/if_else_list/) chain.

### Bi-entity and Item contexts

In **Origins: Math v1.2.0**, Resource-Backed fields were added to both the Bi-entity and Item Actions and Conditions. This new addition highlights a limitation in **Origins: Math**, specifically, ["Invalid" resource values](#invalid-resource-values).

For entities, the Resource is calculated based on the entity the action is being executed on. What about Bi-entities, or Items?

In a Bi-entity context (Bi-entity Actions and Conditions), the Resource is calculated based on the **actor** of the Bi-entity context. This means that you can use [Invert (Bi-entity Action Type)](https://origins.readthedocs.io/en/latest/types/bientity_action_types/invert/) to invert the entity that the Resource will be calculated from, as that action type inverts the actor and target.

In an Item context (Item Actions and Conditions), the Resource is calculated based on the **holder** of the `ItemStack`, obtained through Apoli's `EntityLinkedItemStack` addition, the same addition being used for [Holder Action (Item Action Type)](https://origins.readthedocs.io/en/latest/types/item_action_types/holder_action/).

### Caveats and Limitations

While this is a great system, there is a certain limitations to this feature that **Origins: Math** has no current workaround for.

#### "Invalid" resource values

Ever wondered why the reason why you can only do this with a specific action or condition type? Well, it's because of the way Origins, or more specifically, Apoli, resolves these kinds of objects.

When an action or condition is evaluated, the value, related to the action or condition (an `Entity` for Entity Actions/Conditions, a `Pair<Entity, Entity>` for Bi-entities, etc.), being evaluated on, is passed to the action or condition before it is evaluated on them. **Origins: Math** takes advantage of this system by using the value being passed as the resource target and getting an `Entity` from it, meaning that if a resource is supplied as a "variable" in an Entity Action Type or Entity Conditition Type, the value of that resource for the entity got from the value will be used.

To further clarify the explanation, here is an example:

```json
{
	"type": "origins:multiple",

	// ...

	"resource": {
		"type": "origins:resource",
		"min": 1,
		"max": 10
	},

	"handler": {
		"type": "origins:active_self",
		// ...
		"entity_action": {
			"type": "origins:area_of_effect",
			"radius": 8,
			"shape": "sphere",
			"bientity_action": {
				"type": "origins:target_action",
				"action": {
					"type": "origins:damage",
					"damage_type": "minecraft:player_attack",
					"amount": "*:*_resource"
				}
			}
		}
	}
}
```

In the example above, instead of the actor's (the power holder) `resource` subpower being used as the value for the damage, the target's (entities in the area of effect) `resource` subpower is being used as the value for the damage instead, and since they don't have the requested resource power, `0` is being used instead.

Why is the target's `resource` subpower being used? This is because they are the the "entity being evaluated upon", which we will call the "*evaluator*". The damage is being dealt to them, meaning that they are the entity being evaluated upon, which is the same entity **Origins: Math** uses for resolving resource values, resulting in an unexpected `0`.

Since **Origins: Math v1.2.0**, we can use Resource-backed fields in Bi-entity and Item contexts! For the example above, we can use the [Damage (Bi-entity Action Type)](https://origins.readthedocs.io/en/latest/types/bientity_action_types/damage/) to deal damage to the target while using Resource values from the actor.

```json
{
	"type": "origins:multiple",

	// ...

	"resource": {
		"type": "origins:resource",
		"min": 1,
		"max": 10
	},

	"handler": {
		"type": "origins:active_self",
		// ...
		"entity_action": {
			"type": "origins:area_of_effect",
			"radius": 8,
			"shape": "sphere",
			"bientity_action": {
				"type": "origins:damage",
				"damage_type": "minecraft:player_attack",
				"amount": "*:*_resource"
			}
		}
	}
}
```

Here, the **actor** serves as the *evaluator*, which results in the actor's `resource` subpower being used as the amount of damage dealt to the target, since Resource-backed fields in a **Bi-entity** context are **always** executed on the actor.