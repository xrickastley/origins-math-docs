# Changelog

Here, you can find the updated changelog for **Origins: Math**.

If a power, action or condition type doesn't exist for you, make sure that your version of Origins: Math has that type!

!!! tip
	If a type doesn't exist on this page, that means that it has existed since **Origins: Math 1.0.0**! This means that any version of **Origins: Math** should have that type.

## Origins: Math 1.6.1
### Fixes
- Modifiers for `origins-math:damage_dealt_linked_resource` and `origins-math:damage_taken_linked_resource` now apply properly.

## Origins: Math 1.6.0
### Additions
- Added new power types
	- `origins-math:modify_knockback`
- Added [Custom Hud Renders](./notes/custom_hud_renders.md).

### Changes
- Added new `modifier` and `modifiers` fields to `origins-math:damage_dealt_linked_resource` and `origins-math:damage_taken_linked_resource`
- Enabled conditions for the following powers:
	- `origins-math:damage_dealt_linked_resource`
	- `origins-math:damage_taken_linked_resource`
	- `origins-math:healing_linked_resource`
	- `origins-math:modify_resource_maximum`
	- `origins-math:modify_resource_minimum`

### Origins: Math - Alpha Experiments
- [Extended Compatibility through ASM](./experiments/extended_compatibility_through_asm.md)

<hr>

## Origins: Math 1.5.1
### Fixes
- Fixed crash when YACL isn't added.

### Origins: Math - Alpha Experiments
- [Extended Compatibility through ASM](./experiments/extended_compatibility_through_asm.md)

<hr>

## Origins: Math 1.5.0
### Additions
- Added new power types
	- `origins-math:attribute_like_resource`
	- `origins-math:living_entity_linked_resource`
	- `origins-math:modify_attribute_like_resource`
	- `origins-math:nbt_linked_resource`
- Added new entity action types
	- `origins-math:change_resource`
- Added new bi-entity action types
	- `origins-math:copy_resource_value`
- Added optional [YetAnotherConfigLib](https://modrinth.com/mod/yacl) dependency for the config menu.
- Added `/resource change absolute` command.

### Changes
- Allowed alpha experiments to be toggleable.

### Origins: Math - Alpha Experiments
- [Extended Compatibility through ASM](./experiments/extended_compatibility_through_asm.md)

<hr>

## Origins: Math 1.4.2-alpha
### Fixes
- Fixes `origins-math:status_effect_linked_resource`'s `AMPLIFIER` property from having a value of `0` even when the effect in question is not applied to the entity.

### Changes
- Origins: Math is now more strict when using Resource Backed fields on Resource powers that don't exist.

### Origins: Math - Alpha Experiments
- [Extended Compatibility through ASM](./experiments/extended_compatibility_through_asm.md)

<hr>

## Origins: Math 1.4.1-alpha
Adds a hotfix for the Alpha Experiment: [Extended Compatibility through ASM](./experiments/extended_compatibility_through_asm.md)

### Origins: Math - Alpha Experiments
- [Extended Compatibility through ASM](./experiments/extended_compatibility_through_asm.md)

<hr>

## Origins: Math 1.4.0-alpha
This Origins: Math version adds an alpha experiment: Extended Compatibility through ASM.

### Origins: Math - Alpha Experiments
- [Extended Compatibility through ASM](./experiments/extended_compatibility_through_asm.md)

<hr>

## Origins: Math 1.3.1
### Fixes
- Fixed /resource not working

<hr>

## Origins: Math 1.3.0
### Fixes
- Properly registered previously added Bi-entity action types

### Additions
- Added new power types
	- `origins-math:damage_dealt_linked_resource`
	- `origins-math:damage_taken_linked_resource`
	- `origins-math:healing_linked_resource`

<hr>

## Origins: Math 1.2.0
### Fixes
- Properly crashes when an invalid resource is provided ([#1](https://github.com/xrickastley/origins-math/issues/1))

### Additions
- Added a new Bi-entity Action Type
	- `origins-math:variable_execute_command`
- Added new [Resource Backed Injectors](./notes/resource_backed_fields.md)
	- Bi-entity Action
	- Bi-entity Condition
	- Item Action
	- Item Condition

<hr>

## Origins: Math 1.1.2
### Fixes
- Fixed `origins-math:modifiable_resource` executing it's `min_action`/`max_action` only when it's original maximum or minimum is reached.

<hr>

## Origins: Math 1.1.1
### Additions
- Added new power types
	- `origins-math:modify_simple`
- Added new entity conditions
	- `origins-math:relative_resource`
- Added new Attribute Modifier Operations
	- `origins-math:standard_multiply_base`
	- `origins-math:standard_multiply_total`
	- `origins-math:standard_divide_base`
	- `origins-math:standard_divide_total`
- Added a new command: `/modifier`

<hr>

## Origins: Math 1.1.0
### Additions
- Added new power types
	- `origins-math:current_biome_linked_resource`
	- `origins-math:modifiable_resource`
	- `origins-math:modify_resource_maximum`
	- `origins-math:modify_resource_minimum`
- Added new entity conditions
	- `origins-math:full_resource`
	- `origins-math:empty_resource`
- Added new properties to [Player Property](./types/data_types/player_property.md)
	- `EXP_LEVEL`
	- `EXP_POINTS`
	- `FALL_DISTANCE`
	- `TIME_OF_DAY`
	- `AGE`

### Fixes
- Fixed `origins-math:variable_change_resource` being unregistered.
- Prevented `ResourceBackedInjector` from registering duplicate factory IDs.

<hr>

## Origins: Math 1.0.1
### Additions
- Added `origins-math:variable_change_resource`.

### Changes
- Removed `@Debug(export = true)` mixin annotation used for debugging.
- Changed faulty sources and issues link inside `fabric.mod.json`