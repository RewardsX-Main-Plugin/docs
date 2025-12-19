## Installation
1. Download the **DiscordRewards** addon JAR file from [Spigot](https://www.spigotmc.org/resources/discordrewards-%E2%AD%90-%E2%80%A2-best-rewards-system.130919/).
2. Place the JAR file inside the `plugins/RewardsX/Addons` folder of your Minecraft server.
3. Start or restart your server so that RewardsX loads **DiscordRewards** and generates its default configuration file.
4. Create a Discord bot in the [Discord Developer Portal](https://discord.com/developers/applications) and invite it to your Discord server with permission to read and send messages.
5. Copy the bot token and your Discord server (guild) ID to use in the configuration.

## Configuration
After the first start, a configuration file for **DiscordRewards** is created inside the RewardsX addons folder. Edit this file to connect the addon with your Discord bot and define how message tracking works and when rewards should be given.

### Example `config.yml`

```yaml
discord-settings:
  token: YOUR_DISCORD_BOT_TOKEN   # You can find the token in your Discord Developer Portal
  guild: YOUR_DISCORD_GUILD_ID    # Right-click your Discord server icon and click "Copy Server ID"
  track-channels:
    - '123456789012345678'        # Channel where you want to track message count
    - '234567890123456789'
  dm-message:
    enable: true    # Enable/Disable Dm Message For Reward Congratulation!
  public-message:
    enable: false   # Enable/Disable Public Message For Reward Congratulation!
```

After editing, run `/rewardsx reload` (or restart the server) to apply changes.

### Example features.yml
```yaml
messages:
  10messages:
    enabled: true
    amount: 10
    commands:
    - '[discordReward] dsreward1'
  50messages:
    enabled: true
    amount: 50
    commands:
    - '[discordReward] dsreward2'
```

Here you can customize:

The amount of messages required for each goal.

The commands that will be executed when a player reaches that goal (for example, RewardsX reward commands or custom commands).

You can also configure additional rewards directly in `plugins/RewardsX/rewards.yml` to combine Discord activity with existing RewardsX reward setups.
```yaml
discord-rewards:
  dsreward1:
    enabled: true
    name: dsreward1
    commands:
    - '[message] hello'
    - '[console] say hello'
  dsreward2:
    enabled: true
    name: dsreward2
    commands:
    - '[message] hello'
    - '[console] say hello'
  dsreward3:
    enabled: true
    name: dsreward3
    commands:
    - '[message] hello'
    - '[console] say hello'
```