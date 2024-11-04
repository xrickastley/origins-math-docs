# `/modifier` 

The `/modifier` command can be used to apply a modifier on a given base value.

```mcfunction
modifier apply <target> <power> <base>
```

Fetch a power that uses [Attribute Modifiers](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/#attribute_modifier), applies the value of `<base>` to it, and shows the resulting value as a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/).

* `<target>` being a target selector, can only select one entity at a time
	* (e.g: `@a[limit = 1]`, `@p`, `_xRickAstley`, `b1b981b6-4081-4abe-afd8-e79269c6a339`)
* `<power>` being the namespace and ID of a power that uses [Attribute Modifiers](https://origins.readthedocs.io/en/latest/types/data_types/attribute_modifier/#attribute_modifier)
	* (e.g: `origins:damage_modifier` (`data/origins/powers/damage_modifier.json`))
* `<base>`
	* (e.g: `6`, `9`, `4.20`, `7`)
