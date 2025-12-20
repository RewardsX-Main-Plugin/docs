# The RServer Interface Class

## `RServer` interface

`RServer` is a **platform-agnostic server abstraction** used by RewardsX.  
It wraps common server operations so your code works across **Spigot**, **BungeeCord**, **Velocity**, and other supported platforms without using their APIs directly.
```java
public interface RServer {
RPlayer getPlayerExact(String name);
RPlayer getPlayer(UUID uuid);
RPlayer getPlayer(String name);
RPlayerOffline getOfflinePlayer(String name);
RPlayerOffline getOfflinePlayer(UUID uuid);
void runSync(Runnable task);
void dispatchConsoleCommand(String cmd);
void registerEvents(Object... listeners);
}
```

---

### Player access methods
```java
RPlayer getPlayerExact(String name);
RPlayer getPlayer(UUID uuid);
RPlayer getPlayer(String name);
RPlayerOffline getOfflinePlayer(String name);
RPlayerOffline getOfflinePlayer(UUID uuid);
```


- **`getPlayerExact(String name)`**  
  Returns an **online** `RPlayer` whose name matches exactly the given string (implementation-dependent case-sensitivity).

- **`getPlayer(UUID uuid)`**  
  Returns an **online** `RPlayer` by UUID, or `null` if the player is not connected.

- **`getPlayer(String name)`**  
  Returns an **online** `RPlayer` by name (often allowing partial or case-insensitive matches, depending on platform rules).

- **`getOfflinePlayer(String name)` / `getOfflinePlayer(UUID uuid)`**  
  Return an `RPlayerOffline` wrapper for a player that may be **offline**.  
  Use these when you need identity data (UUID, name) or persistent logic that does not require the player to be online.

These methods let you work with players through `RPlayer`/`RPlayerOffline`, keeping your addon code portable and independent from Bukkit/Velocity/Bungee classes.

---

### Server utility methods
```java
void runSync(Runnable task);
void dispatchConsoleCommand(String cmd);
void registerEvents(Object... listeners);
```


- **`runSync(Runnable task)`**  
  Runs the given task on the **main server thread**.  
  Use this for operations that must be executed synchronously (for example, most Bukkit API calls).

- **`dispatchConsoleCommand(String cmd)`**  
  Executes a command as the **server console**.  
  The string `cmd` should usually be **without** a leading `/`; the implementation handles the dispatch details.

- **`registerEvents(Object... listeners)`**  
  Registers one or more **event listener** objects with the underlying platform’s event system.  
  This allows your addon to listen to events without directly using platform-specific registration APIs.

---

By using `RServer`, your RewardsX integrations can:

- Find online and offline players.  
- Safely run sync tasks.  
- Execute console commands.  
- Register event listeners.  

All while remaining **cross-platform** and decoupled from any specific server implementation.
