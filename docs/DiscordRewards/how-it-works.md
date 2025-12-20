# How it works

## Account linking

To correctly track Discord messages and give in‑game rewards, players must link their Minecraft account with their Discord account.

- Join the Minecraft server.
- Run the command: `/rewardsx discord link`
- Follow the instructions provided in chat / Discord to complete the linking process.

If the account is not linked, DiscordRewards will not be able to match Discord messages to the player in‑game, and no rewards will be given.


When a user sends messages in any of the tracked Discord channels, DiscordRewards increases their message counter.

Once the user’s total messages reach the configured message-threshold, the addon runs the configured reward-command through RewardsX for that player.

The reward can be anything RewardsX supports, such as items, money, or custom commands.