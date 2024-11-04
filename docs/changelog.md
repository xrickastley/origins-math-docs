# Changelog

Here, you can find the updated changelog for **Origins: Math**.

If a power, action or condition type doesn't exist for you, make sure that your version of Origins: Math has that type!

!!! tip
	If a type doesn't exist on this page, that means that it has existed since **Origins: Math 1.0.0**! This means that any version of **Origins: Math** should have that type.

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
- Added new properties to [Player Property](https://origins-math.readthedocs.io/en/latest/types/data_types/player_property/)
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