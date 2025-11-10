---
sidebar_position: 3
title: Development
description: Development Guide
---

This guide provides instructions on how to set up, build, and extend the xdiscord bot with new commands and features.

---

## 1. Setup and Installation

Follow these steps to get the bot running in your development environment.

### Prerequisites
- **Bun:** This project uses `bun` for package management. You can install it from [here](https://bun.sh/).

### Installation & Configuration
1. **Get the Source Code:**
   Download the source code from the cfx portal and unzip it into your resources folder.
   ```bash
   cd xdiscord # Navigate to the unzipped folder
   ```

2. **Install Dependencies:**
   ```bash
   bun install
   ```

3. **Configure the Bot:**
   - Rename `config.example.cfg` to `config.cfg`.
   - Open `config.cfg` and fill in the required values. You can refer to `config.example.cfg` for the structure and available options.
     - `XDISCORD:TOKEN`: Your bot's token.
     - `XDISCORD:CLIENT_ID`: Your bot's application/client ID.
     - `XDISCORD:GUILD_ID`: The ID of your Discord server.
     - `XDISCORD:DEV_ROLES`: Role IDs with developer permissions.
     - `XDISCORD:ADMIN_ROLES`: Role IDs with admin permissions.
     - `XDISCORD:MOD_ROLES`: Role IDs with moderator permissions.
     - `XDISCORD:SERVER_IP`: Your FiveM server's IP and port.
     - `XDISCORD:CFX_LINK`: The direct connect link to your server.
     - `XDISCORD:EMBED_COLOR`: The default color for Discord embeds.
     - `XDISCORD:COMMAND_LOG_CHANNEL`: The ID of the channel for command logs.

4. **Build the Project:**
   This project is written in TypeScript and needs to be compiled.
   ```bash
   bun run build
   ```
   This command compiles the TypeScript files from `src/` into JavaScript files in the `dist/` directory.

5. **Start the Resource:**
   - Add the following line to the top of your `server.cfg` file:
     ```cfg
     exec @xdiscord/config.cfg
     ```
   - To ensure `xdiscord` loads last, place `ensure xdiscord` after all other resource in your `server.cfg`. For example:
     ```cfg
     ensure other-resources
     ensure xdiscord # starts after all other resources
     ```
   - Start your FiveM server.


6. **Deploy Commands:**
   The first time you run the bot, or anytime you add/modify a command's structure, you must deploy the commands to Discord.
   - Go to your **txAdmin** dashboard and open the **Live Console**.
   - Run the following command:
     ```
     deploy-commands
     ```
   - You should see a confirmation that the commands have been deployed.

---

## 2. Development Workflow (Autobuild)

For continuous development, you can use `bun run dev` to automatically recompile your TypeScript code whenever changes are detected. This eliminates the need to manually run `bun run build` after every change.

1.  **Start the Development Watcher:**
    In your terminal, navigate to the `xdiscord` resource folder and run:
    ```bash
    bun run dev
    ```
    This command will watch for changes in your `src/` directory and automatically compile them to `dist/`.

2.  **Restart the Resource in FiveM:**
    After `bun run dev` recompiles your changes, you will need to restart the `xdiscord` resource in your FiveM server to apply them. You can do this via txAdmin or by typing `restart xdiscord` in the server console.

---

## 3. How to Create a New Command

Commands are located in `src/server/bot/commands/`. They are split into `fivem/` (for server interaction) and `misc/` (for general Discord utilities).

### Example: Creating a `/mycommand`

1. **Create the Command File:**
   - Create a new file: `src/server/bot/commands/misc/mycommand.ts`.

2. **Write the Command Code:**
   - Use the following template for your new command. This example creates a simple command that replies to the user.

   ```typescript
   import { SlashCommandBuilder } from 'discord.js';
   import { type Command, Permission } from 'types/discord';

   const mycommand: Command = {
     // Optional: Set the permission level required to use this command.
     // Remove this line to allow everyone to use it.
     permissions: Permission.MOD,

     // Define the command's name, description, and options.
     data: new SlashCommandBuilder()
       .setName('mycommand')
       .setDescription('This is a new example command.')
       .addStringOption(option =>
         option
           .setName('message')
           .setDescription('The message to send back.')
           .setRequired(true)
       ),

     // The function that executes when the command is run.
     execute: async (client, interaction) => {
       if (!interaction.isChatInputCommand()) return;

       const message = interaction.options.getString('message', true);

       await interaction.reply(`You said: ${message}`);
     },
   };

   export default mycommand;
   ```

3. **Generate Index Files:**
   - After creating a new command file, run the `generate` script to automatically update the `index.ts` files.
   ```bash
   bun run generate
   ```
   This script scans the command and event directories and updates their respective `index.ts` files to include your new command.

4. **Build and Deploy:**
   - Run `bun run build` in your terminal (if not using `bun run dev`).
   - Restart the `xdiscord` resource in your FiveM server.
   - Run `deploy-commands` in the server console (if command structure changed).

Your new `/mycommand` should now be available in Discord!

---

## 4. How to Add a New Feature (Client/Server Interaction)

Many features require communication between the Discord bot (server-side) and the FiveM client (client-side). This project uses a callback system for this purpose.

### Example: Creating a feature to get a player's current health.

#### Step 1: Create the Client-Side Callback

The client needs to listen for a request from the server and return the player's health.

- **Edit `src/client/events.ts`:**
  - Add a new `registerClientCallback`. This function will be executed when the server triggers it.

  ```typescript
  import { registerClientCallback } from '@shared/callback';
  // ... other imports

  registerClientCallback('xdiscord:getPlayerHealth', () => {
    const playerPed = PlayerPedId();
    const health = GetEntityHealth(playerPed);
    return health; // Return the health value
  });
  ```

#### Step 2: Create the Server-Side Command

The Discord command will trigger the client callback and display the result.

- **Create a new command file `src/server/bot/commands/fivem/health.ts`:**

  ```typescript
  import { triggerClientCallback } from '@shared/callback';
  import { SlashCommandBuilder } from 'discord.js';
  import { type Command, Permission } from 'types/discord';
  import { framework } from '@server/framework';

  const health: Command = {
    permissions: Permission.MOD,
    data: new SlashCommandBuilder()
      .setName('health')
      .setDescription("Get a player's current in-game health.")
      .addIntegerOption(option =>
        option
          .setName('id')
          .setDescription('The server ID of the player.')
          .setAutocomplete(true) // Use autocomplete for player names
          .setRequired(true)
      ),

    // Add the autocomplete handler to suggest online players
    autocomplete: async (interaction) => {
      const focused = interaction.options.getFocused(true);
      const inputValue = focused.value.toLowerCase();
      const players = framework.getPlayers();

      const choices = players.map((player) => ({
        name: `${player.source} - ${player.name}`,
        value: player.source,
      }));

      const filtered = inputValue
        ? choices.filter((choice) => choice.name.toLowerCase().includes(inputValue))
        : choices;

      await interaction.respond(filtered.slice(0, 25));
    },

    execute: async (client, interaction) => {
      if (!interaction.isChatInputCommand()) return;

      await interaction.deferReply();

      const playerId = interaction.options.getInteger('id', true);
      const playerName = GetPlayerName(String(playerId));

      if (!playerName) {
        await interaction.editReply("Player not found.");
        return;
      }

      try {
        // Trigger the callback on the specified client
        const health = await triggerClientCallback<number>('xdiscord:getPlayerHealth', playerId);
        await interaction.editReply(`**${playerName}**'s health is **${health}**.`);
      } catch (error) {
        console.error(error);
        await interaction.editReply("Failed to get player health. The player may have disconnected.");
      }
    },
  };

  export default health;
  ```

#### Step 3: Register, Build, and Deploy

- Register the new `health` command in `src/server/bot/commands/index.ts`.
- Run `bun run build` (if not using `bun run dev`).
- Restart the `xdiscord` resource in your FiveM server.
- Run `deploy-commands` in the console (if command structure changed).

You can now use the `/health` command in Discord to get the live health of any player on your server. This same pattern can be used for a wide variety of interactive features.

---

## 5. Generating Vehicle Data (for Autocompletion)

This process generates a `vehicles.json` file in the `data/` directory, which is used for autocompletion in Discord commands like `/vehicle give`.

1.  **Ensure you are in-game**: This command must be run by a player in the FiveM game client.
2.  **Open the in-game console**: Press `F8` (default key) to open the FiveM console.
3.  **Run the command**: Type `generate-vehicle-data` and press Enter.
    *   Optionally, you can specify a delay between vehicle spawns in milliseconds (e.g., `generate-vehicle-data 100`). A higher delay might prevent crashes on lower-end systems. The minimum delay is 50ms.
4.  **Wait for completion**: The command will sequentially spawn every vehicle model in the game, extract its data, and then clean it up. This process can take a significant amount of time (e.g., 5-10 minutes or more, depending on your system and the number of vehicle models).
5.  **Verify the output**: Once completed, a `vehicles.json` file will be created or updated in your `xdiscord/data/` directory.
