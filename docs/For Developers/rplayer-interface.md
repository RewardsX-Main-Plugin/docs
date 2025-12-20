# The RPlayer Interface Class

## `RPlayer` interface

`RPlayer` is a **platform-agnostic player abstraction** used by RewardsX.  
It allows addons and internal code to interact with a player **without caring** whether the server is running on **Spigot**, **BungeeCord**, **Velocity**, or another supported platform.

Instead of using platform-specific classes like `org.bukkit.entity.Player` or `com.velocitypowered.api.proxy.Player`, RewardsX exposes `RPlayer` so the same code can run on every instance.

---

### Identity methods

```java
boolean isPlayer();
Object getPlayer();
UUID getUniqueId();
String getPlayerName();
```

- **`isPlayer()`**
    Returns true if the sender is a real player. It can return false when the sender is the console or another non-player source.

- **`getPlayer()`**
    Returns the underlying platform player object as Object. 
    On Spigot/Paper: usually a Player. 
    On Velocity/Bungee: their respective player type. 
    Use this only when you really need platform-specific APIs.

- **`getUniqueId()`**
    Returns the player’s UUID. May be null if the sender is not a player (e.g. console).

- **`getPlayerName()`**
    Returns the display name of the sender (e.g. "Notch" or "CONSOLE"). Never null.

Full Documentation: [RewardsX Library](https://github.com/RewardX-Main/RewardsX-Library/blob/main/src/main/java/net/rewardsxdev/rewardsx/api/player/RPlayer.java).


## `RPlayerOffline` interface

`RPlayerOffline` is a **platform-agnostic abstraction for offline players**, built on top of `RPlayer`.  
It allows RewardsX and its addons to work with a player’s identity even when that player is **not currently online**, while still remaining compatible with multiple platforms (Spigot, Bungee, Velocity, etc.).

---

### Purpose

- Represents a **player profile** that may be offline.  
- Extends `RPlayer`, so it exposes the same identity-related methods.  
- Lets you handle UUIDs and names without depending on platform-specific classes like `OfflinePlayer`.

---

### Identity methods
```java
boolean isPlayer();
Object getPlayer();
UUID getUniqueId();
String getPlayerName();
```


- **`isPlayer()`**  
  Returns `true` if this represents a real player and not a console or other non-player source.  

- **`getPlayer()`**  
  Returns the underlying **platform-specific player object** as `Object`.  
  For offline players this can be an `OfflinePlayer` (on Bukkit-like platforms) or another platform-specific representation.

- **`getUniqueId()`**  
  Returns the player’s **UUID** on Mojang’s network.  
  May be `null` if the sender is not a player (for example, console).

- **`getPlayerName()`**  
  Returns the player’s **name** (for example `"Notch"` or `"CONSOLE"`).  
  This value is never `null`.

---

### When to use `RPlayerOffline`

Use `RPlayerOffline` when:

- You need to **read or store identity data** (UUID, name) for players who might be offline.  
- You work with **persistent rewards, stats, or queues** that should apply even if the player is not connected.  
- You want to keep your code **cross-platform**, avoiding direct use of Bukkit/Velocity/Bungee classes.

For live interactions (chat messages, titles, inventories) you typically use `RPlayer`.  
For identity and offline-safe logic, `RPlayerOffline` provides a unified interface while still fitting into the RewardsX abstraction layer.


Full Documentation: [RewardsX Library](https://github.com/RewardX-Main/RewardsX-Library/blob/main/src/main/java/net/rewardsxdev/rewardsx/api/player/RPlayerOffline.java).

# How To Obtain A RPlayer or RPlayerOffline ?
```java
RServer rserver = api.getRserver().getSpigotRserver() // getBungeeRserver - getVelocityRserver
RPlayer p = rserver.getPlayer(mcUuid); // getPlayer("Notch")
RPlayerOffline pOffline = rserver.getOfflinePlayer(mcUuid); // getOfflinePlayer("Notch")
```

## [`RServer` interface](../rserver-interface)

