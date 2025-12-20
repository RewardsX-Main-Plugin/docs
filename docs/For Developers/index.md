# Getting Started

# 👨‍💻 RewardsX – Developer Documentation

This page is aimed at **developers** who want to integrate, extend, or interact with **RewardsX** through addons or custom plugins.  
It assumes you already have RewardsX installed and running on your Spigot/Paper server.

## 🧱 Architecture Overview

**RewardsX** is designed as a modular **reward engine** for Minecraft servers. It provides:

- A core system for **features** (daily rewards, streaks, playtime, referrals, trades, etc.).
- A flexible **command-based reward layer**, so any external plugin can be triggered via commands.
- A **configuration-driven** approach using YAML, allowing server owners to define reward logic without touching code.
- An **addon system**, so developers can hook into RewardsX logic and expose their own features (e.g. RewardADs, DiscordRewards).

At a high level:

1. RewardsX listens to **events** (login, playtime, AFK, job actions, trades, referrals, etc.).
2. When a condition is met, it evaluates configured **features/rules**.
3. If the rule passes, it executes the associated **reward commands**.
4. Those commands can target **any plugin** installed on the server.

---

You cand find the library here: [RewardsX Library](https://github.com/RewardX-Main/RewardsX-Library)