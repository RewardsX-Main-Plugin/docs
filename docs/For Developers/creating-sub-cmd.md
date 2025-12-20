# Creating a Sub Cmd

## Creating a Sub Command (SubCmd)

Below is an example implementation of a **sub command** using the `SubCmd` interface.  

```java
public class YourCommandClass implements SubCmd {
    @Override
    public String name() {
        return "Your Command Name";
    }

    @Override
    public String description() {
        return MessagesPath.MESSAGES_COMMANDS_SUB_DESC.replace("%key%", name());
    }

    @Override
    public String permission() {
        return "your.plugin.permission";
    }

    @Override
    public boolean execute(RPlayer sender, String[] strings) {
        // ..Implements Your Execute Command
    }
}
```

### What is a `SubCmd`?

A class implementing `SubCmd` represents a **sub command** handled by the RewardsX command system.  
For example, if your main command is `/rewardsx`, a `SubCmd` with `name() == "discord link"` will usually be executed as:
- `/rewardsx discord link`

The `SubCmd` interface typically includes:

- `name()` – the sub command name and path (e.g. `"discord link"`).  
- `description()` – a short description shown in help menus.  
- `permission()` – the permission node required to run this sub command. (leave "" to have no permissions)  
- `execute(RPlayer sender, String[] args)` – the actual logic that runs when the command is executed.

## Description()

- `description()` builds a description string, usually using a messages path or language file. 

## How To Build A Description SubCmd in Different Languages?

```java
public class Language {
    public void setupMessages(){

        Map<String, String> descMapIt = new LinkedHashMap<>();
        Map<String, String> descMapEn = new LinkedHashMap<>();

        descMapIt.put("discord link", "Collega il tuo account minecraft con il server discord");
        descMapEn.put("discord link", "Link your minecraft account with our server discord");

        for(net.rewardsxdev.rewardsx.api.spigot.configuration.Language l : net.rewardsxdev.rewardsx.api.spigot.configuration.Language.getLanguages()){
            YamlConfiguration yml = l.getYml();
            switch (l.getIso()){
                case "en":
                    for (Map.Entry<String, String> entry : descMapEn.entrySet()) {
                        String path = MessagesPath.MESSAGES_COMMANDS_SUB_DESC.replace("%key%", entry.getKey());
                        yml.addDefault(path, entry.getValue());
                    }
                case "it":
                    for (Map.Entry<String, String> entry : descMapIt.entrySet()) {
                        String path = MessagesPath.MESSAGES_COMMANDS_SUB_DESC.replace("%key%", entry.getKey());
                        yml.addDefault(path, entry.getValue());
                    }
                case "iso_language":
                    // .. Rest of Your Code ..
            }

            l.save();
        }
        
    }
}
```

## Registering The SubCmd

To registry your subCmd, in your main class of your addon you should put this line:
```java
MainCmd.registerSubCommand(new YourCommandClass());
```

This pattern keeps sub commands modular, testable, and easy to extend as your addon or integration grows.
