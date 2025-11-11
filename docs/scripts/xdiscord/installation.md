---
sidebar_position: 1
title: Installation
description: A step-by-step guide to installing and configuring xdiscord on your FiveM server, from creating a Discord bot to deploying slash commands.
---

After purchasing **xdiscord** from [Tebex](https://ricepunk.tebex.io):

1. Go to **[https://portal.cfx.re](https://portal.cfx.re)**  
2. Log in with the same account you used to make the purchase  
3. Navigate to **Assets → Granted Assets**  
4. You will find **two granted assets**:  
   - `xdiscord-build.zip`  
   - `xdiscord-source.zip`

### What’s the difference?

- **xdiscord-build.zip** → Ready-to-use version for quick installation.  
- **xdiscord-source.zip** → Full source code, for developers who want to modify or extend features.

> 💡 For most users, we recommend using the **build** version.

For a standard installation, unzip **xdiscord-build.zip** into your `resources` folder and follow the setup guide below.


---

This guide will walk you through setting up the xdiscord bot on your FiveM server.

---

## 1. Create a Discord Bot

First, you need to create a bot application in Discord.

1.  Go to the [Discord Developer Portal](https://discord.com/developers/applications).
2.  Click **"New Application"** and give it a name.
3.  Navigate to the **"Bot"** tab on the left menu.
4.  Under **"Privileged Gateway Intents"**, enable both **"SERVER MEMBERS INTENT"** and **"MESSAGE CONTENT INTENT"**.
5.  Click **"Reset Token"** to reveal your bot's token. **Copy this token** – you will need it later.
6.  Go to the **"General Information"** tab and copy the **"APPLICATION ID"** (this is your Client ID).

## 2. Invite the Bot to Your Server

Now, you need to generate an invitation link to add the bot to your Discord server.

1.  Go to the **"OAuth2"** tab and then **"URL Generator"**.
2.  In the **"SCOPES"** section, select `bot` and `applications.commands`.
3.  A new **"BOT PERMISSIONS"** section will appear below. Select **"Administrator"** for simplicity.
4.  Copy the **generated URL** at the bottom, paste it into your browser, and follow the steps to invite the bot to your desired server.
5.  Once the bot is in your server, right-click on the server name and select **"Copy Server ID"**. You'll need this for the configuration. (If you don't see this option, enable Developer Mode in Discord settings: `Settings > Advanced > Developer Mode`).

## 3. Configure the Resource

1.  In your `xdiscord` resource folder, rename `config.example.cfg` to `config.cfg`.
2.  Open `config.cfg` and fill in the following values:

    ```cfg
    # The token you copied from the Discord Developer Portal
    set XDISCORD:TOKEN "YOUR_BOT_TOKEN_HERE"

    # The Application ID you copied
    set XDISCORD:CLIENT_ID "YOUR_APPLICATION_ID_HERE"

    # The ID of the server you invited the bot to
    set XDISCORD:GUILD_ID "YOUR_SERVER_ID_HERE"
    ```

3.  Optionally, you can configure the rest of the settings:
    *   `XDISCORD:DEV_ROLES`, `XDISCORD:ADMIN_ROLES`, `XDISCORD:MOD_ROLES`: Add the Role IDs for different permission levels.
    *   `XDISCORD:SERVER_IP`: Your FiveM server's IP and port (e.g., `127.0.0.1:30120`).
    *   `XDISCORD:CFX_LINK`: The direct connect link to your server.
    *   `XDISCORD:EMBED_COLOR`: The default color for Discord embeds (hex code).
    *   `XDISCORD:COMMAND_LOG_CHANNEL`: The ID of the channel where command usage will be logged.

## 4. Start the Resource

Add the following lines to your `server.cfg` file to start the resource and load its configuration.

```cfg
# Add this at the top of your server.cfg
exec @xdiscord/config.cfg

# Add this with your other resources. It's recommended to start it after other main resources.
ensure xdiscord
```

## 5. Deploy Slash Commands

After starting your server for the first time with the bot configured:

1.  Go to your **txAdmin** dashboard.
2.  Navigate to the **Live Console**.
3.  Enter the command `deploy-commands` and press Enter.
4.  You should see a confirmation message that the commands were successfully deployed to your Discord server.

Your bot is now set up and ready to use!

## 6. Generate Vehicle Data (for Autocompletion)

This process generates a `vehicles.json` file in the `data/` directory, which is used for autocompletion in Discord commands like `/vehicle give`.

1.  **Ensure you are in-game**: This command must be run by a player in the FiveM game client.
2.  **Open the in-game console**: Press `F8` (default key) to open the FiveM console.
3.  **Run the command**: Type `generate-vehicle-data` and press Enter.
    *   Optionally, you can specify a delay between vehicle spawns in milliseconds (e.g., `generate-vehicle-data 100`). A higher delay might prevent crashes on lower-end systems. The minimum delay is 50ms.
4.  **Wait for completion**: The command will sequentially spawn every vehicle model in the game, extract its data, and then clean it up. This process can take a significant amount of time (e.g., 5-10 minutes or more, depending on your system and the number of vehicle models).
5.  **Verify the output**: Once completed, a `vehicles.json` file will be created or updated in your `xdiscord/data/` directory.

---

## Data Files Explanation

The `data/` directory contains several JSON files that are used to store data for the resource. Here is an explanation of each file:

### `connect.json`

This file contains the template for the embed message sent by the `/connect` command. You can customize the title, description, color, fields, and buttons of the embed message.

### `locations.json`

This file contains a list of named locations with their coordinates. These locations are used by the `/teleport` command for autocompletion. You can add, remove, or modify locations in this file.

### `vehicles.json`

This file is generated by the `generate-vehicle-data` command. It contains a list of all vehicles in the game, with their name, hash, and type. This file is used for autocompletion in vehicle-related commands like `/vehicle give`.

### `weather.json`

This file contains a list of all available weather types in the game. This is used by the `/weather` command for autocompletion.
