---
sidebar_position: 3
title: Commands
description: A comprehensive list of all available xdiscord commands and their functionalities.
---

# Commands

This document provides a comprehensive list of all available commands within the xdiscord resource, along with their descriptions and usage.

## Slash Commands

### /character
Manage character data.
  - **view** `<id>`: View online character data.
    - `<id>`: The player's server ID (source).
  - **update**:
    - **name** `<id> <firstname> <lastname>`: Update an online character's name.
      - `<id>`: The player's server ID (source).
      - `<firstname>`: The character's first name.
      - `<lastname>`: The character's last name.
    - **job** `<id> <job> <grade>`: Update an online character's job.
      - `<id>`: The player's server ID (source).
      - `<job>`: The job name.
      - `<grade>`: The job grade.
  - **offline**:
    - **view** `<id>`: View offline character data.
      - `<id>`: The player's server ID (source).
    - **name** `<id> <firstname> <lastname>`: Update an offline character's name.
      - `<id>`: The player's server ID (source).
      - `<firstname>`: The character's first name.
      - `<lastname>`: The character's last name.
    - **job** `<id> <job> <grade>`: Update an offline character's job.
      - `<id>`: The player's server ID (source).
      - `<job>`: The job name.
      - `<grade>`: The job grade.

### /connect
Show how to connect to the FiveM server.

### /inventory
Manage player inventories.
  - **view** `<id>`: View a player's inventory.
    - `<id>`: The player's server ID (source).
  - **give** `<id> <item> <amount>`: Give an item to a player.
    - `<id>`: The player's server ID (source).
    - `<item>`: The item name.
    - `<amount>`: The quantity of the item.
  - **remove** `<id> <item> <amount>`: Remove an item from a player.
    - `<id>`: The player's server ID (source).
    - `<item>`: The item name.
    - `<amount>`: The quantity of the item.
  - **clear** `<id>`: Clear a player's inventory.
    - `<id>`: The player's server ID (source).
  - **find-item** `<item> [min-amount] [limit]`: Find players with a specific item.
    - `<item>`: The item name.
    - `[min-amount]`: (Optional) The minimum amount of the item to search for.
    - `[limit]`: (Optional) The maximum number of players to return.

### /player
Manage online players.
  - **list**: List all online players.
  - **info** `<id>`: Get information about a player.
    - `<id>`: The player's server ID (source).
  - **message** `<id> <message>`: Send a direct message to a player in-game.
    - `<id>`: The player's server ID (source).
    - `<message>`: The message to send.
  - **screenshot** `<id>`: Take a screenshot of a player's screen.
    - `<id>`: The player's server ID (source).
  - **kill** `<id>`: Kill a player.
    - `<id>`: The player's server ID (source).
  - **revive** `<id>`: Revive a player.
    - `<id>`: The player's server ID (source).
  - **kick** `<id> [reason]`: Kick a player from the server.
    - `<id>`: The player's server ID (source).
    - `[reason]`: (Optional) The reason for the kick.
  - **ban** `<id> [reason]`: Ban a player from the server.
    - `<id>`: The player's server ID (source).
    - `[reason]`: (Optional) The reason for the ban.

### /resource
Manage server resources.
  - **list**: List all server resources.
  - **info** `<name>`: Get information about a resource.
    - `<name>`: The name of the resource.
  - **start** `<name>`: Start a resource.
    - `<name>`: The name of the resource.
  - **stop** `<name>`: Stop a resource.
    - `<name>`: The name of the resource.
  - **restart** `<name>`: Restart a resource.
    - `<name>`: The name of the resource.

### /server
Manage the FiveM server.
  - **info**: Get server information.
  - **performance**: Get server performance metrics.
  - **announce** `<message>`: Send a global announcement to the server.
    - `<message>`: The message to announce.
  - **time** `<hour> [minute]`: Set the in-game time.
    - `<hour>`: The hour to set (0-23).
    - `[minute]`: (Optional) The minute to set (0-59).
  - **weather** `<type>`: Set the in-game weather.
    - `<type>`: The weather type (e.g., `extrasunny`, `rain`).
  - **blackout** `<state>`: Enable or disable server-wide blackout.
    - `<state>`: `on` or `off`.
  - **freezetime** `<state>`: Freeze or unfreeze in-game time.
    - `<state>`: `on` or `off`.
  - **richest** `[limit]`: Show the richest players.
    - `[limit]`: (Optional) The number of players to show.
  - **clear**:
    - **all**: Clean up all entities (vehicles, peds, props).
    - **props**: Clean up all props.
    - **peds**: Clean up all peds.
    - **vehicles** `[owned]`: Clean up all vehicles.
      - `[owned]`: (Optional) Set to 'true' to include player-owned vehicles in the cleanup. If omitted, only unowned vehicles are cleaned up.
  - **tx**:
    - **restart**: Restart the server via txAdmin.

### /teleport
Teleport players.
  - **player**:
    - **location** `<id> <location> [vehicle]`: Teleport a player to a location.
      - `<id>`: The player's server ID (source).
      - `<location>`: A predefined location name.
      - `[vehicle]`: (Optional) `true` to teleport the player inside their vehicle.
    - **coords** `<id> <coords> [vehicle]`: Teleport a player to coordinates.
      - `<id>`: The player's server ID (source).
      - `<coords>`: Coordinates in `x,y,z` format.
      - `[vehicle]`: (Optional) `true` to teleport the player inside their vehicle.
  - **all**:
    - **location** `<location>`: Teleport all players to a location.
      - `<location>`: A predefined location name.
    - **coords** `<coords>`: Teleport all players to coordinates.
      - `<coords>`: Coordinates in `x,y,z` format.

### /vehicle
Manage vehicles.
  - **list**: List all spawned vehicles.
  - **give** `<id> <vehicle> [plate]`: Give a vehicle to a player.
    - `<id>`: The player's server ID (source).
    - `<vehicle>`: The vehicle spawn name.
    - `[plate]`: (Optional) The license plate for the vehicle.
  - **info**:
    - **entity** `<id>`: Get vehicle info by its server entity ID.
      - `<id>`: The vehicle's server entity ID.
    - **plate** `<plate>`: Get vehicle info by its license plate.
      - `<plate>`: The vehicle's license plate.

### /eval
(Developer) Execute JavaScript code.
  - `<code>`: The JavaScript code to execute.

### /help
Displays a list of all available commands.

### /ping
Check if the bot is alive.

### /sticky
Manage sticky messages in channels.
  - **add** `<channel> <content> [cooldown]`: Add a sticky message to a channel.
    - `<channel>`: The Discord channel to send the sticky message to.
    - `<content>`: The content of the sticky message.
    - `[cooldown]`: (Optional) The cooldown in seconds between sticky messages.
  - **remove** `<channel>`: Remove a sticky message from a channel.
    - `<channel>`: The Discord channel to remove the sticky message from.
  - **list**: List all active sticky messages.

## Context Menu Commands

  - **Online (User)**: Check if a Discord user is on the server.