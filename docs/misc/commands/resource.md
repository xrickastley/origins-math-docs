# `/resource` (Overwrite)

The `/resource` command can be used to change (add/subtract), get, set, and do operations on resource. Resource operations can only do scoreboard objective to resource, not resource to resource.

!!! warning
	This command is originally an Origins/Apoli command, but has been modified by **Origins: Math** to add extra functionality to it. You can find the additions created by **Origins: Math** below. 

```mcfunction
resource get absolute <target> <power>
```

Fetch the absolute value of a (cooldown or resource) power from the specified target. This shows the value as a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/) instead of an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/).

* `<target>` being a target selector, can only select one entity at a time
	* (e.g: `@a[limit = 1]`, `@p`, `_xRickAstley`, `b1b981b6-4081-4abe-afd8-e79269c6a339`)
* `<power>` being the namespace and ID of a power
	* (e.g: `origins:arcane_skin` (`data/origins/powers/arcane_skin.json`))