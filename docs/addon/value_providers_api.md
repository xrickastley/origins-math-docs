# Value Providers API

!!! warning 
	This page leans more on the development of an Addon (Mod) for Origins: Math. Click another page if you are only using this mod as a dependency for datapacking.

Since Origins: Math `v1.4.2-alpha`, resource values are now queried through the **Value Providers API**. If your mod also contains a "more-precise" value being held by a `Power`, and you want Origins: Math to recognize it, the Value Providers API is now the proper way to do so.

## Registering a Value Provider

To register a Value Provider, you can use the [`ValueProviders.registerProvider`](https://github.com/xrickastley/origins-math/blob/1.20.2/src/main/java/io/github/xrickastley/originsmath/util/ValueProviders.java#L35) method.

This method accepts a `Power` class and a `ValueProvider<T>` instance, which handles the various providers for each "value", such as the Power's value, maximum value and minimum value!

```java
ValueProviders.registerProvider(
	MyCustomPower.class,
	new ValueProvider<>(MyCustomPower::getCustomValue, MyCustomPower::getCustomMax, MyCustomPower::getCustomMin)
);
```

## Registering a Value Modifier

Despite this API's name, it also holds Value Modifiers! As their name suggests, Value Modifiers are objects that allow you to **modify** the value of a `Power` class.

These Value Modifiers are used in place of "changing resources", such as the [Variable Change Resource (Entity Action Type)](../types/entity_action_types/variable_change_resource.md).

To register a Value Modifier, you can use the [`ValueProviders.registerModifier`](https://github.com/xrickastley/origins-math/blob/1.20.2/src/main/java/io/github/xrickastley/originsmath/util/ValueProviders.java#L43) method.

This method accepts a `Power` class and a `ValueModifier<T>` instance, which handles the various modifiers for each "value", such as adding and setting a value!

```java
ValueProviders.registerModifier(
	MyCustomPower.class,
	new ValueModifier<>(MyCustomPower::setCustomValue, MyCustomPower::addCustomValue)
);
```

## Value Provider and Modifier Priority

For a specific `Power` instance, the priority of `ValueProvider` A and `ValueProvider` B depend on the `Power`'s class inheritance, where a more "recently" inherited class, i.e. a class deeper in the inheritance chain, will have higher priority.

For instance, say that a `Power` instance is an instance of `MyCustomPower`, where the `MyCustomPower` class follows the given inheritance chain:

```
MyCustomPower < MyAbstractCustomPower < VariableIntPower < Power
```

If a `ValueProvider` for both `MyAbstractCustomPower` and `VariableIntPower` exist, the `ValueProvider` for `MyAbstractCustomPower` takes the higher priority, as it is more deeper in the inheritance chain than `VariableIntPower`.

However, if no `ValueProvider` exists for both `MyCustomPower` and `MyAbstractCustomPower`, then the value provider for `VariableIntPower` will be used instead, given that a `ValueProvider` for it exists and is registered.

If no `ValueProvider` exists for all classes in the inhertiance chain of the `Power` instance, an `IllegalArgumentException` is thrown instead of returning a `null` value.

The rules for the priority of a `ValueModifier` follow the same rules as those governing the priority of a `ValueProvider`, discussed in this section.