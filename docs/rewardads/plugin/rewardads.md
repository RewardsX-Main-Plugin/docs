# Getting Started

Monetize your platform in a few easy steps with RewardADs.

## Requirements

1. Your platform must be set up and verified at [dash.rewardsx.net](https://dash.rewardsx.net).  
   If you haven't completed this yet, follow the steps in our [Getting Started](/rewardads/website/getting-started/) guide.

This guide explains all configuration files for the **RewardADs** plugin used with **RewardsX** monetization system.

## File Locations

**Paper/Spigot:**
```
plugins/RewardsX/addons/RewardADs/
├── main.yml
├── messages.yml
├── rewards.yml
└── userdata.yml
```

**BungeeCord/Velocity:**
```
plugins/RewardADs/
├── main.yml
├── messages.yml
├── rewards.yml
└── userdata.yml
```

## main.yml - Main Configuration

Controls plugin behavior, and general options.

```yaml
platform_id: XXXXXXX # You can find id here --> https://dash.rewardsx.net
debug: false  #Enable/Disable debug messages
proxy: false  # ENABLE THIS ONLY IF YOU USE REWARDADS ALSO IN BUNGEECORD
```


## messages.yml - Customizable Messages

All player-facing messages with full PlaceholderAPI support.

```yaml
prefix: '&7[&6R&7] '
onlyConsole: Hey, you are only a console, so you can't execute this command!
reload: '&6RewardADs &areloaded successfully'
outOfDate: |-
  &6RewardADs &eis out of date &7[&6%s &7> &6v%s&7]&e!
  &7Download the new update from:
  &7- &b%s
  &7- &b%s
  &7- &b%s&r
currentVersion: '&6RewardADs &ecurrent version: &6%s'
previousPage: '&cPrevious Page'
nextPage: '&aNext Page'
emailAndPassword: '&cInsert valid email and password'
guiTitle: '&8Rewards - Page %s'
loginSuccess: '&aLogin successful!'
invalidPassword: '&cInvalid password'
accountNotFound: '&cAccount not found'
logout: '&aLogout successful!'
alreadyLogout: '&cYou are already logged out'
alreadyLogin: '&cYou are already logged in'
currentBalance: '&eYour current balance: &6%s AdBits'
configurePlatform: '&cYour platform ID in main.yml does not appear to exist in our
  systems, make sure you have entered the code found in https://rewardads.me/console
  correctly&r'
welcome: '&eWelcome &a%s &ein &6RewardADs&e!&r'
ownPlatform: '&cYou can''t buy for your own platform'
waitBuy: '&cYou must wait at least 10 seconds between buy requests'
insufficientAdBits: '&cYou don''t have enough AdBits'
buySuccess: |-
  &aBought reward successfully!
  &7Please wait up to a minute to receive your reward
notLogin: '&cYou are not logged in, please execute the login with &b/rewardads login'
rewardReceived: '&aYou have received the reward!'
actionNotFound: '&cAction reward not found, please contact an administrator'
platformNotReady: |-
  &cThis platform is not ready or it is in Proxy only
  If you think this is an error, please contact an administrator
noPermission: '&cNo permission'
confirmTitle: '&cConfirm Purchase'
confirmYes: '&aYes, buy it!'
confirmNo: '&cNo, go back'
currentBuys: '&eYour current buys: &9%s'
topAdbitsValue: '&6#%index% &r%player% &7| &6&l%value%'
topAdbitsNoData: '&6#%index% &r________ &7| &rNo data'
topBuysValue: '&9#%index% &r%player% &7| &9&l%value%'
topBuysNoData: '&9#%index% &r________ &7| &rNo data'

```
## rewards.yml - Reward Definitions

**Core file** defining all available rewards players can purchase with ads.

```yaml
!!str <REWARD_ID>:
commands:
- "give %player% pumpkin 100"
- "broadcast %player% received 100 pumpkins!"
server: "all"
``` 

