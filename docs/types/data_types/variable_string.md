---
title: Variable Declaration (Data Type)
---

# Variable String

[Data Type](../data_types.md)

A [String](https://origins.readthedocs.io/en/latest/types/data_types/string/) with special support for adding variables.

There are two ways to indicate a variable inside a variable string: prepending it with `$:` or the more preferred way: by following the JavaScript [String interpolation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#string_interpolation) syntax, enclosing it in `${}`.

For the former, it will consider **every character** after it as part of the variable **so long as** it follows the variable naming rule: the valid characters to name a variable are **letters**, **digits** and the **underscore** character (`_`), however, you can **not** have a number as the **first** character in the variable.

For example: `$:myVariable` where `myVariable` is the variable, `"$:variableInQuotes"` where `variableInQuotes` is the variable, and `$:i2`, where `i2` is the variable.

Well, what if you want to output something like seconds, where it'd come with a format? For instance, take the command below.

```mcfunction
title @s actionbar { "text": "Ability on Cooldown! $:secondss", "color": "red" }
```

The intention is to tell the user that their ability is on cooldown, providing the seconds. You'd assign "seconds" as the variable, and it'd output, for example: "Ability on Cooldown! 30s". 

However, Origins: Math wouldn't read it that way. Instead, it reads the variable as `secondss` and returns an error since there is no variable named `secondss`. A workaround would be to split the text into different parts, like so:

```mcfunction
title @s actionbar [{ "text": "Ability on Cooldown! ", "color": "red" }, { "text": "$:seconds", "color": "red" }, { "text": "s", "color": "red" }]
```
However, it's quite tedious, especially if you have a lot of these. This is where the second way to define variables comes in.

For the latter, it will consider **every character inside** the brackets as part of the variable, regardless of variable naming rules. However, this will **not** prevent errors from trying to do so; you will still get them. This is the more preferred method as the intended variable name can be easily determined, both by you and other people reading your power code. In addition, this is the only syntax where [specifying a number format](#specifying-a-number-format) can be done.

For example: `${myVariable}` where `myVariable` is the variable, `"${variableInQuotes}"` where `variableInQuotes` is the variable, and `${i2}`, where `i2` is the variable.

For our example command, we can easily change it to this format and get the result we want.
```mcfunction
title @s actionbar { "text": "Ability on Cooldown! ${seconds}s", "color": "red" }
```
Here, Origins: Math will only see `seconds` inside the brackets, and will search for it as a variable instead of `secondss`.

### Specifying a number format

Sometimes, we want to use more precise values, like for teleporting, or for adding velocity. This is easily achieved within Origins: Math with [Resource-backed fields](../../notes/resource_backed_fields.md), but is quite hard to do so outside of Origins: Math, like within commands, or NBT. However, since Origins: Math v1.7.0, this is now possible!

Previously, both [Variable Execute Command (Entity Type)](../entity_action_types/variable_execute_command.md) and [Variable Execute Command (Bi-entity Type)](../bientity_action_types/variable_execute_command.md) worked the same way: when you declare a variable, its value is first [truncated](https://en.wikipedia.org/wiki/Truncation), and is then added to the command string in place of the variable, taking advantage of the fact that Minecraft's command system can accept an [Integer](https://origins.readthedocs.io/en/latest/types/data_types/integer/) as a [Float](https://origins.readthedocs.io/en/latest/types/data_types/float/).

However, this proved to be a hindrance in times when you wanted to take advantage of the fact that Origins: Math will store floating-point values, as you were unable to use them in commands. Now, you can **explicitly** specify when the floating-point value is used.

As of now, there are two format specifiers: `%d` for decimal integers, and `%.<p>f` for floating-point numbers, where `<p>` is the precision, or in other words, the amount of numbers after the decimal point. Variables are implicitly specified as integers, unless the floating-point number format specifier is used. These are added after the variable, after a colon (`:`), like so: `${myVariable:%.2f}`.

Say we want to add some milliseconds to the command display above. We can easily do so by using the floating-point specifier.

```mcfunction
title @s actionbar { "text": "Ability on Cooldown! ${seconds:%.2f}s", "color": "red" }
```

This will display the amount of seconds before the ability can be used again, with two decimal places of precision.

## Command

[Data Type](../data_types.md)

A [Variable String](#variable-string) that specifies a valid command string.

### Examples

```mcfunction
damage @s $:stored_damage minecraft:player_attack
```

This example will damage the user by the amount given by the `stored_damage` variable. If the `stored_damage` variable is internally represented as a floating-point number, it is [truncated](https://en.wikipedia.org/wiki/Truncation).

```mcfunction
title @s actionbar { "text": "Ability on Cooldown! ${seconds:%.2f}s", "color": "red" }
```

This example will display the amount of seconds given by the `seconds` variable before the ability can be used again, with two decimal places of precision as provided by the [floating-point format specifier](#specifying-a-number-format).

## NBT Path

[Data Type](../data_types.md)

A [Variable String](#variable-string) used to specify one or more particular elements from an [NBT Data Tree](https://minecraft.wiki/w/NBT_format), as used by the [`/data`](https://minecraft.wiki/w/Commands/data) command.

### Examples

```
foo.bar[${n}]
```

This example specifies the n-th element of the "bar" tag under the "foo" tag, provided by the variable `n`. If the `stored_damage` variable is internally represented as a floating-point number, it is [truncated](https://en.wikipedia.org/wiki/Truncation).