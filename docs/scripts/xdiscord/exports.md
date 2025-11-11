---
sidebar_position: 3
title: Export
description: Detailed documentation on the exported functions available in xdiscord, with examples in Lua and JavaScript for integration with other FiveM resources.
---

# Exports

This document explains how to use the exported functions from `xdiscord` in your other resources, using either Lua or JavaScript.

## `getDiscordId`

This function retrieves a player's Discord ID from their game identifier.

- **`identifier`**: The player's server ID.

**Returns:** The player's Discord ID (string) or `nil` if not found.

**Lua Example**
```lua
local src = source
local discordId = exports.xdiscord:getDiscordId(src)
if discordId then
    print("Player's Discord ID: " .. discordId)
end
```

**JavaScript Example**
```js
const src = global.source;
const discordId = global.exports.xdiscord.getDiscordId(src);
if (discordId) {
    console.log(`Player's Discord ID: ${discordId}`);
}
```

---

## `getName`

This function retrieves a player's Discord nickname or username.

- **`identifier`**: The player's server ID or Discord ID.

**Returns:** The player's Discord name (string) or `nil` if not found.

**Lua Example**
```lua
local src = source
local name = exports.xdiscord:getName(src)
if name then
    print("Player's Discord Name: " .. name)
end
```

**JavaScript Example**
```javascript
const src = global.source;
const name = await global.exports.xdiscord.getName(src);
if (name) {
    console.log(`Player's Discord Name: ${name}`);
}
```

---

## `getRoles`

This function retrieves a list of a player's Discord role IDs.

- **`identifier`**: The player's server ID or Discord ID.

**Returns:** A table/array of the player's Discord role IDs.

**Lua Example**
```lua
local src = source
local roles = exports.xdiscord:getRoles(src)
for _, roleId in ipairs(roles) do
    print("Role ID: " .. roleId)
end
```

**JavaScript Example**
```javascript
const src = global.source;
const roles = await global.exports.xdiscord.getRoles(src);
roles.forEach(roleId => {
    console.log(`Role ID: ${roleId}`);
});
```

---

## `hasRole`

This function checks if a player has one or more specific Discord roles.

- **`identifier`**: The player's server ID or Discord ID.
- **`roles`**: A single role ID (string) or a table/array of role IDs.

**Returns:** `true` if the player has at least one of the specified roles, `false` otherwise.

**Lua Example**
```lua
local src = source
local hasRole = exports.xdiscord:hasRole(src, "YOUR_ROLE_ID")
if hasRole then
    print("Player has the role!")
end

local rolesToCheck = {"ROLE_ID_1", "ROLE_ID_2"}
local hasAnyOfRoles = exports.xdiscord:hasRole(src, rolesToCheck)
if hasAnyOfRoles then
    print("Player has one of the roles!")
end
```

**JavaScript Example**
```javascript
const src = global.source;
const hasRole = await global.exports.xdiscord.hasRole(src, "YOUR_ROLE_ID");
if (hasRole) {
    console.log("Player has the role!");
}

const rolesToCheck = ["ROLE_ID_1", "ROLE_ID_2"];
const hasAnyOfRoles = await global.exports.xdiscord.hasRole(src, rolesToCheck);
if (hasAnyOfRoles) {
    console.log("Player has one of the roles!");
}
```

---

## `isInGuild`

- **`identifier`**: The player's server ID or Discord ID.

**Returns:** `true` if the player is in the server, `false` otherwise.

**Lua Example**
```lua
local src = source
local inGuild = exports.xdiscord:isInGuild(src)
if inGuild then
    print("Player is in the Discord server!")
else
    print("Player is not in the Discord server.")
end
```

**JavaScript Example**
```javascript
const src = global.source;
const inGuild = await global.exports.xdiscord.isInGuild(src);
if (inGuild) {
    console.log("Player is in the Discord server!");
} else {
    console.log("Player is not in the Discord server.");
}
```

---

## `createLog`

This function sends a message to a pre-configured webhook.

- **`webhookName`**: The name of the webhook to use (e.g., `default`, `money`), as configured in `data/webhooks.json`.
- **`data`**: An embed data object. See the [Discord Developers documentation](https://discord.com/developers/docs/resources/message#embed-object) for the available fields.

**Lua Example**
```lua
local embedData = {
    title = "My Lua Embed",
    description = "This is a test from Lua!",
    color = 0x4A70A9, -- Integer color
    fields = {
        {name = "Field 1", value = "Value 1", inline = true},
        {name = "Field 2", value = "Value 2", inline = true}
    },
    footer = {
        text = "My Footer"
    },
    timestamp = os.date("!%Y-%m-%dT%H:%M:%S.000Z")
}

exports.xdiscord:createLog("default", embedData)
```

**JavaScript Example**
```javascript
const embedData = {
    title = "My JS Embed",
    description = "This is a test from JS!",
    color = 0x4A70A9, // Integer color
    fields: [
        {name = "Field 1", value = "Value 1", inline = true},
        {name = "Field 2", value = "Value 2", inline = true}
    ],
    footer: {
        text = "My Footer"
    },
    timestamp = new Date().toISOString()
};

global.exports.xdiscord.createLog("default", embedData);
```
