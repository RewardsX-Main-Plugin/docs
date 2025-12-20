# Register A Listener

## How To Register A New Listener?

Simplify create your Listener Class and then put:

**Spigot Listener Register**
```java
RewardsXAPI.getRserver().getSpigotRserver().registerEvents(new YourListenerClass());
```

**BungeeCord Listener Register**
```java
RewardsXAPI.getRserver().getBungeeRserver().registerEvents(new YourListenerClass());
```

**Velocity Listener Register**
```java
RewardsXAPI.getRserver().getVelocityRserver().registerEvents(new YourListenerClass());
```

That's It!