# RewardADs Integration Guide

Monetize your platform in a few easy steps with RewardADs.

## Requirements

1. Your platform must be set up and verified at [dash.rewardsx.net](https://dash.rewardsx.net).  
   If you haven't completed this yet, follow the steps in our [Getting Started](https://docs.rewardsx.net/getting-started/) guide.

## Configuration Parameters

| Parameter       | Description                                              | Required |
|-----------------|----------------------------------------------------------|----------|
| `<REWARD_ID>`   | Your unique reward identifier from the dashboard         | Yes      |
| `buyText`       | Custom purchase button text                              | Optional |
| `cancelText`    | Custom cancel button text                                | Optional |
| `onSuccess`     | Callback function for successful purchases               | Optional |
| `onError`       | Callback function for failed purchases                   | Optional |
| `commands`      | Enter commands that must be executed when purchasing the reward | Yes |
| `server`        | Choose which server the plugin should be sent to, proxy only | Yes |

## Minecraft Integration

### Requirements

- **Paper/Spigot:** [RewardsX Core Plugin](https://www.spigotmc.org/resources/rewardsx-%E2%AD%90-%E2%80%A2-best-rewards-system.128841/) + [RewardADs Addon](https://spi.rewardads.me)
- **BungeeCord/Velocity:** [RewardADs Standalone](https://spi.rewardads.me)

Install plugins in `/plugins/` directory (RewardADs Addon goes in `/plugins/RewardsX/addons/` for Paper/Spigot).

### 1. Rewards Setup

Configure rewards in `rewards.yml`:
```
!!str <REWARD_ID>:
commands:
- "give %player% pumpkin 100"
- "broadcast %player% received 100 pumpkins!"
server: "all"
```

**File Locations:**  
- **Paper/Spigot:** `/plugins/RewardsX/addons/RewardADs/rewards.yml`  
- **BungeeCord/Velocity:** `/plugins/RewardADs/rewards.yml`

### 2. Message Customization

Customize plugin messages in `messages.yml`:

**File Locations:**  
- **Paper/Spigot:** `/plugins/RewardsX/addons/RewardADs/messages.yml`  
- **BungeeCord/Velocity:** `/plugins/RewardADs/messages.yml`

**Configuration Options:**  
- `commands`: Server commands to execute when reward is claimed  
- `server`: Target server for proxy networks (`"all"` or specific server name)  
- **Variables:** All PlaceholderAPI variables supported

**You're ready!** Players can now watch ads to earn rewards.

## Website Integration

### Requirements

[RewardADs SDK](https://sdk.rewardsx.net/rewardads.js) must be installed and configured. Follow the setup guide when creating your platform at [dash.rewardsx.net](https://dash.rewardsx.net).

### 1. Load SDK

Add the RewardADs SDK script to your website's `<head>` section:
```
<script src="https://sdk.rewardsx.net/rewardads.js"></script>
```
### 2. Initialize SDK

Initialize the SDK in your website's JavaScript code:
```
RewardADsSDK.init('PLATFORM_ID', 'PLATFORM_SECRET');
```

Replace `PLATFORM_ID` with your unique platform identifier and `PLATFORM_SECRET` with your platform's secret key.

### 3. Security Considerations

**Important:** Store your `PLATFORM_SECRET` securely and never expose it in client-side code. Consider implementing server-side initialization for production environments to protect sensitive credentials.

## SDK Methods

### 1. Checkout Button

Assign checkout functionality to an existing button:
```
const button = document.getElementById('button-checkout');
RewardADsSDK.buttonCheckout(button, {
rewardId: '<REWARD_ID>',
buyText: 'Buy',
cancelText: 'Cancel',
onSuccess: function(result) {
console.log('Purchase success!', result);
},
onError: function(error) {
console.error('Purchase failed:', error);
}
});
```


### 2. Single Reward Display

Render a specific reward in a container:

```
const container = document.getElementById('reward-container');
RewardADsSDK.renderReward(container, {
rewardId: '<REWARD_ID>',
buyText: 'Buy',
cancelText: 'Cancel',
onSuccess: function(result) {
console.log('Purchase success!', result);
},
onError: function(error) {
console.error('Purchase failed:', error);
}
});
```

### 3. All Platform Rewards

Display all rewards for your platform:

```
const container = document.getElementById('rewards-container');
RewardADsSDK.renderRewards(container, {
buyText: 'Buy',
cancelText: 'Cancel',
onSuccess: function(result) {
console.log('Purchase success!', result);
},
onError: function(error) {
console.error('Purchase failed:', error);
}
});
```


### 4. Programmatic Checkout

Trigger checkout programmatically within any function:
```
RewardADsSDK.renderCheckout({
rewardId: '<REWARD_ID>',
buyText: 'Buy',
cancelText: 'Cancel',
onSuccess: function(result) {
console.log('Purchase success!', result);
},
onError: function(error) {
console.error('Purchase failed:', error);
}
});
```

## Additional Notes

- Ensure the SDK loads before attempting to initialize it
- The SDK requires `PLATFORM_ID` and `PLATFORM_SECRET` parameters to function properly
- Test your implementation in a development environment before deploying to production

**You're ready!** Your platform can now monetize through RewardADs.
