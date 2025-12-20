# RewardsX API

## `API` interface

The `API` interface is the **main entry point** for interacting with RewardsX as a developer.  
It exposes core systems such as the database, jobs, GUI, configs, platform adapter, and a few utility methods (like resource copying and logging banners).
```java
public interface API {
String JOBS_LIST_PATH = "/Jobs/list";
String MENU_GUI_PATH = "/Menus";
String JOBS_PATH = "/Jobs";
String ADDONS_PATH = "/Addons";
String TRADE_PATH = "/Trade";
String STREAK_PATH = "/Streak";
String MESSAGES_PATH = "/Lang";
String cmd = "rewardsx";
String cmdAlias = "rx";

void loadLibrary();
void initDb();
Database getRemoteDatabase();
void copyResource(Class<?> var1, String var2, File var3);
void logAddOnBanner(String var1, boolean var2);
RewardsGUI getGUI();
JobSystem getJobManager();
Configs getConfigs();
TradeManager getTradeManager();
void displayCustomerDetails(RPlayer var1);
RServer getRserver();
PlatformAdapter getInstance();
}

```


### Core responsibilities

- **Constants**  
  Paths like `JOBS_PATH`, `ADDONS_PATH`, `MESSAGES_PATH` point to internal RewardsX folders (jobs, menus, addons, streak, trade, language, etc.), and `cmd` / `cmdAlias` define the base command (`/rewardsx` / `/rx`).

- **Lifecycle & library/database**  
  - `loadLibrary()` – load any required external libraries (e.g. for DB, platform integration).  
  - `initDb()` – initialize the database connection used by RewardsX.  
  - `Database getRemoteDatabase()` – obtain the shared database instance.

- **Resource and logging utilities**  
  - `copyResource(Class<?> clazz, String resourcePath, File destination)` – copy a resource from the JAR to a file.  
  - `logAddOnBanner(String name, boolean enabled)` – log an add-on banner with a flag.  
  - `displayCustomerDetails(RPlayer player)` – show customer/license details to a player (for example in a GUI or chat).

- **Subsystem accessors**  
  - `RewardsGUI getGUI()` – access the GUI system for rewards menus.  
  - `JobSystem getJobManager()` – access the Jobs reward system.  
  - `Configs getConfigs()` – access the configuration managers (see [`Configs`](#nested-configs-interface) below).  
  - `TradeManager getTradeManager()` – access the Trade reward system.  
  - `RServer getRserver()` – access the cross-platform server abstraction.  
  - `PlatformAdapter getInstance()` – get the active platform adapter (Spigot/Bungee/Velocity).

---

## `copyResources` utility (static method)
```java
static void copyResources(Class<?> clazz, String resourcePath, File destination) {
        try (InputStream in = clazz.getResourceAsStream("/" + resourcePath)) {
            if (in == null) {
                System.out.println("❌ Resource not found: " + resourcePath);
                return;
            }

            if (!destination.getParentFile().exists()) {
                destination.getParentFile().mkdirs();
            }

            Files.copy(in, destination.toPath(), StandardCopyOption.REPLACE_EXISTING);
            System.out.println("✅ Copied " + resourcePath + " to " + destination.getAbsolutePath());
        } catch (IOException e) {
            System.out.println("❌ Error copying " + resourcePath + ": " + e.getMessage());
            e.fillInStackTrace();
        }
    }

```


This static helper:

- Loads a resource from inside the JAR (`clazz.getResourceAsStream("/" + resourcePath)`).
- Ensures the destination folder exists (`destination.getParentFile().mkdirs()`).
- Copies the stream to `destination` using `Files.copy(..., REPLACE_EXISTING)`.
- Logs success (`✅ Copied ...`) or failure (`❌ Resource not found` / error message).
- Properly closes the input stream and handles exceptions.

Use it to **export default configs/Menus/Jobs** from your addon or RewardsX core into the file system.

---

## Logging helpers (default methods)
```java
default void logAddOnBanner(String name) {
        PlatformAdapter adapter = getInstance();
        adapter.info("╔════════════════════════════════════════════════════════════════════════╗");
        adapter.info("║                                                                        ║");
        adapter.info(String.format ("            Enabled add-on %-40s        ", name));
        adapter.info("║                                                                        ║");
        adapter.info("╚════════════════════════════════════════════════════════════════════════╝");
    }


default void printRewardsX() {
        PlatformAdapter adapter = getInstance();
        String[] art = new String[] {
                "██████╗ ███████╗██╗    ██╗ █████╗ ██████╗ ██████╗ ███████╗██╗  ██╗",
                "██╔══██╗██╔════╝██║    ██║██╔══██╗██╔══██╗██╔══██╗██╔════╝╚██╗██╔╝",
                "██████╔╝█████╗  ██║ █╗ ██║███████║██████╔╝██║  ██║███████╗ ╚███╔╝ ",
                "██╔══██╗██╔══╝  ██║███╗██║██╔══██║██╔══██╗██║  ██║╚════██║ ██╔██╗ ",
                "██║  ██║███████╗╚███╔███╔╝██║  ██║██║  ██║██████╔╝███████║██╔╝ ██╗",
                "╚═╝  ╚═╝╚══════╝ ╚══╝╚══╝ ╚═╝  ╚═╝╚═╝  ╚═╝╚═════╝ ╚══════╝╚═╝  ╚═╝"
        };

        adapter.info("");
   
        for (String line : art) {
            adapter.info(line);
        }
        adapter.info("");
    }
```

- `logAddOnBanner(String name)`  
  Prints a nice ASCII banner in console when an addon is enabled, using the `PlatformAdapter` logger.

- `printRewardsX()`  
  Prints RewardsX ASCII art using the `PlatformAdapter` logger, useful on startup.

Both methods use `getInstance()` to get the `PlatformAdapter`, which abstracts the logging for Spigot/Bungee/Velocity.

---

## Nested `RServer` interface

```java
public interface RServer {
SpigotServer getSpigotRserver();
BungeeServer getBungeeRserver();
VelocityServer getVelocityRserver();
}
```

- Provides **platform-specific server wrappers**:
  - `SpigotServer`, `BungeeServer`, `VelocityServer`.
- Allows advanced integrations to access platform-specific features while still going through the RewardsX API layer.

Use this when you need deeper integration with one platform, but still want to obtain the objects via the API.

---

## Nested `Configs` interface
```java
public interface Configs {
ConfigManager getSpigotConfig();
net.rewardsxdev.rewardsx.api.bungee.configuration.ConfigManager getBungeeConfig();
net.rewardsxdev.rewardsx.api.velocity.configuration.ConfigManager getVelocityConfig();

ConfigManager getRewardsConfig();
ConfigManager getJobsSettings();
MenuConfig getMenusGUI(String var1);
ConfigManager getTradeRewards();
ConfigManager getJobsYml();
YamlConfiguration getJobsYml(String var1);
}
```

This interface centralizes access to **RewardsX configuration files** across all supported platforms:

- **Global platform configs**
  - `getSpigotConfig()` – Spigot-specific config manager.  
  - `getBungeeConfig()` – Bungee-specific config manager.  
  - `getVelocityConfig()` – Velocity-specific config manager.

- **RewardsX core configs**
  - `getRewardsConfig()` – main RewardsX configuration.  
  - `getJobsSettings()` – settings for the Jobs system.  
  - `getTradeRewards()` – configuration for trade-related rewards.  
  - `getJobsYml()` / `getJobsYml(String name)` – access to jobs YAML files, either default or by name.  
  - `getMenusGUI(String key)` – returns a `MenuConfig` for a specific GUI/menu definition.

As an addon developer, you typically:

- Use `getConfigs()` from the `API` instance.  
- Call specific getters (e.g. `getRewardsConfig()`, `getJobsSettings()`) to read or modify RewardsX configuration and menus in a **standardized** way.

---

## Summary

- `API` is the **central hub** for all RewardsX systems (DB, jobs, trade, GUI, configs, server, platform).  
- It provides **utility methods** like `copyResources`, `logAddOnBanner`, and `printRewardsX` to simplify common tasks.  
- Nested interfaces `RServer` and `Configs` structure access to **server-specific** and **configuration** components, keeping your integrations clean and cross-platform.


Full Documentation: **[RewardsX Library](https://github.com/RewardX-Main/RewardsX-Library/blob/main/src/main/java/net/rewardsxdev/rewardsx/api/API.java)**