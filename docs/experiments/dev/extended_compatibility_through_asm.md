# Experiment: Extended Compatibility through ASM (Developer)

### Affected Versions: 
- Origins: Math `>=1.4.0`

<hr>

This **Origins: Math** experiment introduces "Extended Compatibility through ASM".

This alpha experiment aims to extend the compatibility of **Origins: Math** with specific action and condition types from Origins/Apoli and more action and condition types added by other mods by taking advantage of ASM and the Java Bytecode.

## Introduction

In preceeding versions, **Origins: Math**'s [Resource-backed Fields](../../notes/resource_backed_fields.md) relies on a single premise to work properly: when taking an `int`, `float` or `double`, or their respective object wrappers/reference types (`Integer`, `Float`, or `Double`), they must **always** be queried through their corresponding `get` versions in the `SerializableData$Instance`: `getInt`, `getFloat`, and `getDouble`, respectively.

This looked to **always** be the case. However, with hindsight, there are **rare** instances where it would be queried with `get()`, as seen [here](https://github.com/apace100/apoli/blob/1.20/src/main/java/io/github/apace100/apoli/power/factory/action/bientity/DamageAction.java#L30), in the code for [Damage (Bi-entity Action Type)](https://origins.readthedocs.io/en/latest/types/bientity_action_types/damage/):
```java
public class DamageAction {
    public static void action(SerializableData.Instance data, Pair<Entity, Entity> entities) {
		// ...

        Float damageAmount = data.get("amount");
	}
}
```
Since using `get()` returns the `ResourceBacked` instance itself, this results in a `ClassCastException`, since the `ResourceBacked` class isn't an extension of the `Float` class (and `Float` can't be extended, due to it being `final`). 

Thanks to @younglife4712 from the Origins Discord Server who initially caught this issue and brought to our attention in [this message](https://discord.com/channels/734127708488859831/1372182632325972111/1374131353314136255), we were able to revoke the assumption that "Number" types were always queried from their `get` version in the `SerializableData$Instance`.

An easy fix would be to hardcode every single action/condition type that uses this "method" to instead use the specialized `get` version in the `SerializableData$Instance`. However, this method would prove to be tedious and ruins the "dynamicness" of the mod that we originally saw.

Thus, we looked for other options, eventually resulting in using the Java Bytecode to dynamically precast the `ResourceBacked` instance into the wanted `Number` type before it is returned from `SerializableData$Instance`.

## Theorem

We first concluded that if we can "dynamically determine" what the wanted `Number` type is before returning, we can "precast" the `ResourceBacked` instance through it's `intValue()`, `floatValue()` and `doubleValue()` methods and return that instead.

Second, we discovered you can retrieve the current stack trace of the current thread through `Thread.currentThread().getStackTrace()`, which returns a `StackTraceElement[]`. In each `StackTraceElement`, it contains the **class name**, **method name** and the **line number** of the executing method.

Third, thanks to Mixin, we know that the Java Bytecode has various instruction points, which is actually what Mixin uses to find an injection point!

With this information, we concluded that if there is some way to "extract" the calling method's instruction and retrieve the type being casted into so that we can "precast" the `ResourceBacked` instance, we can keep the "dynamicness" of **Origins: Math** while extending it's compatibility with other action and condition types further.

## Methodology

First, we "go back" in the `StackTraceElement` array by a shifted amount. If you've looked at the code, this is set as `4`, with the "trace" being:  
`someClass.method > SerializableData$Instance.get > SerializableData$Instance.originsmath$injectResourceLinkToGet > SerializableData$Instance.originsmath$getCallingContext`,  
all of which is `4` in total.

This allows us to get the class method calling `SerializableData$Instance.get`. We exclude the `SerializableData$Instance` itself, since we've already handled that through the other Mixin methods.

We create a `ClassReader` for the target class and a `ClassNode` that the `ClassReader` accepts in order for the specified `ClassNode` to be populated with the data for the target class.

Once done, we iterate over it's methods, then, for each method, see if it has the same name as the method name retrieved from the `StackTraceElement`. If it does, we check it's `LineNumberNode`s if the specified number in the `StackTraceElement` is in the method, to account for *method overloading*.

If we successfully find the `LineNumberNode`, we can proceed with precasting. Otherwise, the "find" operation is considered a failure, and the original `ResourceBacked` instance is returned.

In precasting, we take the found `LineNumberNode` and iterate over the instructions in it to find a call to `SerializableData$Instance.get`. Once found, we immediately check the next instruction to see if it's a `TypeInsnNode` with an Opcode of `CHECKCAST`. If it's not, then we can safely return the `ResourceBacked` instance, as it doesn't look to be casted into an invalid type. If it is, then we can conclude that this is the "line" retrieving the value.

Finally, using the `desc` of the `TypeInsnNode`, we can retrieve what it's trying to be casted into, and return the precasted value accordingly.

You can find the code for this experimental feature [here](https://github.com/xrickastley/origins-math/blob/1.20.2/src/main/java/io/github/xrickastley/originsmath/mixins/SerializableDataInstanceMixin.java#L58).