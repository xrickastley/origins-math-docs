# Creating Resource Bridges

!!! warning 
	This topic leans more on the development of an Addon (Mod) for Origins: Math. Click another page if you are only using this mod as a dependency for datapacking.

In Origins: Math, a "resource bridge" is used in **bridging** a Minecraft-based value to a Resource. This allows for datapackers to use Minecraft values directly in their resources without the need for a big [If-Else List](https://origins.readthedocs.io/en/latest/types/meta_action_types/if_else_list/) chain comparing various values from conditions and linking them to their Resource counterpart.

Currently, Resource powers are handled through Apoli's `VariableIntPower` class. Origins: Math extends upon that, creating a `LinkedVariableIntPower` and a `SuppliedLinkedVariableIntPower`, both of which makes bridging Minecraft-based values to Resources easier.

## The LinkedVariableIntPower class

A `LinkedVariableIntPower` is an abstract extension of the `VariableIntPower` class, where the value of this power is obtained either through the `supplyValue()` or the `supplyDoubleValue()` method.

This power's value cannot be changed, as it only serves as an bridge between the Minecraft-based value and Resource power types. Attempting to do so will only change it back to it's supplied value given by either the `supplyValue()` or the `supplyDoubleValue()` method

When extending on a `LinkedVariableIntPower`, you must provide an implementation for both the `supplyValue()` and the `supplyDoubleValue()` abstract methods.

The `supplyValue()` method is used when the value of the Resource needs to be obtained. Since Resources are by default; integers, the `supplyValue()` method must return an `int`.

On the other hand, the `supplyDoubleValue()` method is used when a more-precise value of the Resource needs to be obtained. This is more commonly used in the `VariableSerializer`, which handles serializing declared variables in both the [Math Resource (Power Type)](../types/power_types/math_resource.md) and the [Variable Execute Command (Entity Action Type)](../types/entity_action_types/variable_execute_command.md), as well as the `/resource get absolute` subcommand.

## SuppliedLinkedVariableIntPower

A `SuppliedLinkedVariableIntPower` is an abstract extension of the `LinkedVariableIntPower` class. What makes it different is it's capability to easily supply various properties of a given object.

The type `T` would be the object that the given `Supplier` would be supplying for the `Consumer` to consume. It'll make more sense when I give an example, so hang on tight in there!

Say we have a bunch of properties of a `PlayerEntity` that we would want to get, such as their hunger, saturation, relative health, breathing, how long they've been on fire, etc. We could use an `if-else` to check for each property the power would want, but that would be harder to read the more properties you want to have. This is where the `SuppliedLinkedVariableIntPower` comes into play.

First of all, we need to declare an `enum` that implements the `InstanceValueSupplier<T>` interface. This allows the enum values to be used in supplying the value for a specific property.

For this example, we will be referring to the `PlayerLinkedResourcePower` class.
```java
public class PlayerLinkedResourcePower extends SuppliedLinkedVariableIntPower<PlayerEntity> {
	// ...

	private static enum PlayerProperty implements InstanceValueSupplier<PlayerEntity> {
		FOOD_LEVEL      (player -> player.getHungerManager().getFoodLevel()),
		SATURATION      (player -> player.getHungerManager().getSaturationLevel()),
		HEALTH          (player -> player.getHealth()),
		RELATIVE_HEALTH (player -> player.getHealth() / player.getMaxHealth()),
		ABSORPTION      (player -> player.getAbsorptionAmount()),
		BREATHING       (player -> player.getAir()),
		FIRE_TICKS      (player -> player.getFireTicks()),
		FROZEN_TICKS    (player -> player.getFrozenTicks()),
		FREEZING_SCALE  (player -> player.getFreezingScale()),
		EXP_SCORE       (player -> player.getScore()),
		SLEEP_TIMER     (player -> player.getSleepTimer()),
		STUCK_ARROWS    (player -> player.getStuckArrowCount()),
		X               (player -> player.getX()),
		Y               (player -> player.getY()),
		Z               (player -> player.getZ()),
		VELOCITY_X      (player -> player.getVelocity().getX()),
		VELOCITY_Y      (player -> player.getVelocity().getY()),
		VELOCITY_Z      (player -> player.getVelocity().getZ()),
		PITCH           (player -> player.getPitch()),
		YAW             (player -> player.getYaw()),
		ROLL            (player -> player.getRoll());

		private final Function<PlayerEntity, Number> supplier;

		PlayerProperty(Function<PlayerEntity, Number> supplier) {
			this.supplier = supplier;
		}

		public int supplyValue(PlayerEntity player) {
			return supplier
				.apply(player)
				.intValue();
		}

		public Number supplyAsNumber(PlayerEntity player) {
			return supplier.apply(player);
		}
	}
}
``` 

Here, we created an enum whose values supply a function that takes in `T` (which is a `PlayerEntity`) to return a `Number` (this is the superclass containing all numerical types, so an implicit conversion or widening type casting is made). The `PlayerProperty` enum also provides implementations for the `InstanceValueSupplier<T>` interface, which are `supplyValue()` and `supplyAsNumber()`.

We can take advantage of Calio's `SerializableDataType.enumValue()` method to easily create a `SerializableDataType` out of our newly created `PlayerProperty` enumeration.
```java
private static final SerializableDataType<PlayerProperty> PLAYER_PROPERTY = SerializableDataType.enumValue(PlayerProperty.class);
```

Finally, we create the factory and constructor for the `PlayerLinkedResourcePower` class.
```java
public class PlayerLinkedResourcePower extends SuppliedLinkedVariableIntPower<PlayerEntity> {
	private PlayerLinkedResourcePower(PowerType<?> type, LivingEntity entity, PlayerProperty property) {
		super(type, entity, property, () -> entity instanceof final PlayerEntity player ? player : null);
	}

	public static PowerFactory<?> createFactory() {
		return new PowerFactory<>(
			OriginsMath.identifier("player_linked_resource"),
			new SerializableData()
				.add("property", PlayerLinkedResourcePower.PLAYER_PROPERTY),
			data -> (powerType, livingEntity) -> new PlayerLinkedResourcePower(powerType, livingEntity, data.get("property"))
		);
	}

	// ...
}
```
The neat part of creating a `SerializableDataType` out of our `PlayerProperty` enum allows for us to easily get a `PlayerProperty` from the `SerializableData$Instance`, removing the need to check our enum for the provided `String` value.

You may have noticed that in the constructor, the chosen `PlayerProperty` and a `Supplier<T>` is also provided. This is actually how a `SuppliedLinkedVariableIntPower` gets it's value: the `Supplier<T>` is run, giving a value of `T`, which is then passed to the `InstanceValueSupplier<T>#supplyValue()` or the `InstanceValueSupplier<T>#supplyAsNumber()` value, giving the value of the Resource.

In this example, `null` may be returned if the `LivingEntity` isn't a `PlayerEntity`. When `null` is returned by the `Supplier<T>`, the resource will automatically have a value of `0` when it is queried.