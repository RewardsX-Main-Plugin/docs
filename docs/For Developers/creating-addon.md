# Creating An Addon

## 🧩 Creating Addons (Conceptual)

While concrete APIs depend on the current RewardsX version, the common pattern for an addon is:

1. **Register** yourself as a RewardsX addon (e.g. via a specific API or config).
2. **Listen** to the events or hooks exposed by RewardsX (e.g. when a feature is triggered).
3. Optionally **register new features** or extend existing ones.

### Addon data folder (`data`)
Conceptual Java example (pseudo-code, not final API):

In this example:

```java
public final class YourAddonClass implements RewardsXAddon {
    
    private static Path data;

    @Override
    public String getAddonName() {
        return "YourAddonName";
    }

    @Override
    public String getVersion() {
        return "1.0.0";
    }

    @Override
    public void onEnable(AddonContext ctx) {
        try {
            data = ctx.getMainDataFolder(); // <---- THIS IS IMPORTANT FOR THE ADDON TO WORK
            // .. Rest of your code ..
        } catch (Exception e) {
            getLogger().severe("[YourAddonPlugin] Error during onEnable:");
            e.printStackTrace();
        }
    }
}
```


The `data` field is a `Path` that points to the **main data folder of your addon inside the RewardsX plugin structure**.

Typically this will resolve to something like:

- `plugins/RewardsX/Addons/YourAddonName/`

This directory is where your addon should:

- Create and read its configuration files (for example: `config.yml`, `features.yml`).
- Store any local data (JSON/YAML files, cache, logs, etc.). (Or you can use RewardsX local data to store)
- Create subdirectories if needed (for example: `data/`, `storage/`, `lang/`).  (Or you can use RewardsX subdirectories)


Using `ctx.getMainDataFolder()` is **required** so that your addon always uses the correct RewardsX addon folder and works reliably across different environments and operating systems.


Example usage:
```java
Path configPath = data.resolve("config.yml");
Path featuresPath = data.resolve("features.yml");

// Ensure the folder exists
Files.createDirectories(data);

if (Files.notExists(configPath)) {
// Copy a default config from your JAR resources or create a new one
}
```

Another Example Usage With Custom Config Manager:
```java
public static API RewardsXAPI;

Class<?> mainClass = Class.forName("net.rewardsxdev.rewardsx.spigot.RewardsX_Spigot");
    Method getAPIMethod = mainClass.getMethod("getApi");
    Object apiInstance = getAPIMethod.invoke(null); // static method

    if (apiInstance instanceof API) {
        RewardsXAPI = (API) apiInstance;
    }
    
PlatformAdapter adapter = RewardsXAPI.getInstance();
Plugin plugin = (Plugin) adapter.pluginInstance();
ConfigManager config = new MainConfig(plugin, "config", data.toString());
ConfigManager featuresConfig = new FeaturesConfig(plugin, "features", data.toString());
```

In this example:

- `PlatformAdapter` is an abstraction used by RewardsX to **detect and adapt to the current server platform/instance** (for example Spigot, Paper, or other supported implementations). 
- Calling `RewardsXAPI.getInstance()` returns the active `PlatformAdapter`, which knows how to provide platform-specific objects, such as the main plugin instance.
- `adapter.pluginInstance()` returns the underlying **plugin instance** that RewardsX is running on, which is then cast to `Plugin` so it can be passed into your custom `ConfigManager` implementations.

The custom config managers:

- `MainConfig` handles the main `config.yml` file for your addon.  
- `FeaturesConfig` handles the `features.yml` (or similar) where you define reward features and rules.  

Both use `data.toString()` (the addon data folder path) so that configuration files are created and loaded inside the correct RewardsX addon directory.

PlatformAdapter Interface Class:
```java
public interface PlatformAdapter {
    Type type();

    void info(String var1);

    void error(String var1);

    void debug(String var1);

    void severe(String var1);

    String getName();

    String getPluginVersion();

    boolean isDbEnabled();

    Path getDataPath();

    Object pluginInstance();

    public static enum Type {
        SPIGOT,
        BUNGEECORD,
        VELOCITY;

        private Type() {
        }
    }
}
```