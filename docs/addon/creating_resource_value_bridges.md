# Creating Resource value Bridges

!!! warning 
	This page leans more on the development of an Addon (Mod) for Origins: Math. Click another page if you are only using this mod as a dependency for datapacking.

In Origins: Math, a "resource bridge" is used in **bridging** a Minecraft-based value to a Resource. This allows for datapackers to use Minecraft values directly in their resources without the need for a big [If-Else List](https://origins.readthedocs.io/en/latest/types/meta_action_types/if_else_list/) chain comparing various values from conditions and linking them to their Resource counterpart.

Currently, Resource powers are handled through Apoli's `VariableIntPower` class. Origins: Math extends upon that with both the `LinkedVariableIntPower` and `SuppliedLinkedVariableIntPower` classes, both of which makes bridging Minecraft-based values to Resources easier.

## Single-value bridging

Say that we want a single Minecraft value to be used as a Resource bridge, such as the score of the entity holding the power for a specific objective. We can do this through the `LinkedVariableIntPower` class.

A `LinkedVariableIntPower` is an abstract extension of the `VariableIntPower` class, whose value is obtained either through `supplyValue()` or the `supplyDoubleValue()` method.

This power's value cannot be changed, as it only serves as an bridge between the Minecraft-based value and Resource power types. Attempting to do so will only change it back to it's supplied value given by either the `supplyValue()` or `supplyDoubleValue()` method.

When extending on a `LinkedVariableIntPower`, you must provide an implementation for both the `supplyValue()` and `supplyDoubleValue()` abstract methods.

The `supplyValue()` method is used when the value of the Resource needs to be obtained, which is used by Origins/Apoli. Since Resources are by default: integers, the `supplyValue()` method must return an `int`.

On the other hand, the `supplyDoubleValue()` method is used when a more-precise value of the Resource needs to be obtained. This is more commonly used in the `VariableSerializer`, which handles serializing declared variables in both the [Math Resource (Power Type)](../types/power_types/math_resource.md) and the [Variable Execute Command (Entity Action Type)](../types/entity_action_types/variable_execute_command.md), as well as the `/resource get absolute` subcommand.

We can retrieve the value of the entity for the specified scoreboard objective, then return it through `supplyValue()` since it's already an `int` type. You can see this done by [Scoreboard Linked Resource](../types/power_types/scoreboard_linked_resource.md) in it's [source code](https://github.com/xrickastley/origins-math/blob/1.20.2/src/main/java/io/github/xrickastley/originsmath/powers/ScoreboardLinkedResourcePower.java#L25).

In Origins: Math `v1.4.2`, the [**Value Providers API**](./value_providers_api.md) uses the `supplyDoubleValue()` method of the `LinkedVariableIntPower` class to retrieve it's values.

## Multi-value bridging

Say we have a bunch of properties of a `PlayerEntity` that we would want to get, such as their hunger, saturation, relative health, breathing, how long they've been on fire, etc. We could use an `if-else` to check for each property the power would want, but that would be harder to read the more properties you want to have. This is where the `SuppliedLinkedVariableIntPower` comes into play.

A `SuppliedLinkedVariableIntPower` is an abstract extension of the `LinkedVariableIntPower` class. What makes it different is it's capability to easily supply various properties from a single object.

The type `T` would be the object that the given `Supplier` would be supplying for the `Consumer` to consume. For our example, since we want the properties of a player, `T` would be a `PlayerEntity`.

For our wanted player values, we need to declare an `enum` that implements the `InstanceValueSupplier<T>` interface. This allows the enum values to be used in supplying the value for a specific property.

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

Here, we created an enum whose values supply a function that takes in `T` (which is a `PlayerEntity`) to return a `Number`, the superclass for the wrapper classes containing the primitive numerical types, allowing us to return any sort of number under the `Number` class. The `PlayerProperty` enum also provides implementations for the `InstanceValueSupplier<T>` interface, which are `supplyValue()` and `supplyAsNumber()`.

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
The neat part about using a `PlayerProperty` enum allows for different enum values to return different Player values, while also taking advantage of easily creating a `SerializableDataType` out of our enum through the `SerializableDataType.enumValue` method, allowing us to easily get a `PlayerProperty` from the `SerializableData$Instance`, removing the need for a big `if-else` chain to check the provided `String` value. This also allows Calio, the serialization library, to prematurely throw an error upon trying to read powers with incorrect enum values.

You may have noticed that in the constructor, the chosen `PlayerProperty` and a `Supplier<T>` is also provided. This is actually how a `SuppliedLinkedVariableIntPower` gets it's value: the `Supplier<T>` is run, giving a value of `T`, which is then passed to the `InstanceValueSupplier<T>#supplyValue()` or the `InstanceValueSupplier<T>#supplyAsNumber()` value, giving the value of the Resource.

In this example, `null` may be returned if the `LivingEntity` isn't a `PlayerEntity`. When `null` is returned by the `Supplier<T>`, the resource will automatically have a value of `0` when it is queried.