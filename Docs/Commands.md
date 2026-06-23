# Chat Commands

All Essentials commands live under the **`!ess`** prefix and are typed in the
global chat (or run from the server console). The host suppresses recognised
command lines from normal chat.

- `!ess` — overview of available commands.
- `!ess help` — list commands; `!ess help <command>` shows usage for one.
- `!help` — list every server command across all plugins.

**Arguments:** `<required>`, `[optional]`. Quote `"multi word"` values. Booleans
accept `true/false`, `yes/no`, `on/off`, `1/0`.

**Permissions** use Space Engineers promote levels, lowest to highest:
`None` < `Scripter` < `Moderator` < `SpaceMaster` < `Admin` < `Owner`. A caller
below a command's level does not see it in the overview. Commands without an
explicit level require **Admin**.

## Auto commands & server control

Drive [auto command](AutoCommands.md) sequences and the server lifecycle.

| Command | Permission | Description |
|---|---|---|
| `!ess say <message>` | Admin | Broadcast a plain chat message to all players. |
| `!ess broadcast <message>` | Admin | Broadcast a highlighted (yellow) chat message. |
| `!ess notify <message> [durationMs] [font]` | Admin | HUD notification to all players (default 5000 ms, font `White`). |
| `!ess save` | Admin | Save the world. |
| `!ess reload` | Admin | Save, then reload the dedicated-server config (MOTD etc.). |
| `!ess restart [now]` | Admin | Run the configured restart sequence, or restart immediately with `now`. |
| `!ess stop [now]` | Admin | Run the configured shutdown sequence, or stop immediately with `now`. |
| `!ess runauto <name>` | Admin | Start an auto command now, regardless of its trigger. |
| `!ess cancelauto <name>` | Admin | Cancel running instances of an auto command. |
| `!ess listauto` | Admin | List configured and running auto commands. |

### Voting

| Command | Permission | Description |
|---|---|---|
| `!ess vote <name>` | None | Start a vote for the `Vote`-triggered auto command `<name>`. |
| `!ess vote yes` | None | Vote yes on the current vote. |
| `!ess vote no` | None | Vote no on the current vote. |
| `!ess vote list` | None | List the configured vote commands. |

## Player & admin QoL

MOTD opens through the Plugin SDK mission-screen API when MagnetarMod is
available; otherwise it falls back to private chat output.

| Command | Permission | Description |
|---|---|---|
| `!ess motd` | None | Show the configured MOTD and MOTD URL. |
| `!ess stats` | Admin | Show server runtime stats, sim speed, entity counts, PCU and memory. |
| `!ess playerlist` | Admin | List online players with identity id, Steam id and promote level. |
| `!ess mute <player> [minutes]` | Admin | Mute a player in chat; `0` or omitted means indefinite. |
| `!ess unmute <player>` | Admin | Remove a chat mute. |
| `!ess list mute` | Admin | List muted players and remaining time. |
| `!ess msg <player> <message>` | None | Send a private message to an online player. Quote names with spaces. |
| `!ess whis <player> <message>` | None | Alias for `msg`. |

## Blocks

Toggle or remove functional blocks across all (non-projected) grids. Block
**type** is the object-builder type without the `MyObjectBuilder_` prefix
(e.g. `Reactor`, `ShipWelder`); **subtype** is the block subtype id.

| Command | Permission | Description |
|---|---|---|
| `!ess blocks on type <type>` | Admin | Enable all blocks of a type. |
| `!ess blocks off type <type>` | Admin | Disable all blocks of a type. |
| `!ess blocks on subtype <subtype>` | Admin | Enable all blocks of a subtype. |
| `!ess blocks off subtype <subtype>` | Admin | Disable all blocks of a subtype. |
| `!ess blocks remove type <type>` | Admin | Remove all blocks of a type. |
| `!ess blocks remove subtype <subtype>` | Admin | Remove all blocks of a subtype. |
| `!ess blocks on general <category>` | Admin | Enable a category: `Power`, `Production`, `Weapons`. |
| `!ess blocks off general <category>` | Admin | Disable a category: `Power`, `Production`, `Weapons`. |

## Cleanup & world maintenance

Destructive cleanup and world-maintenance commands require running the same
command again within 30 seconds to confirm. Cleanup grid commands evaluate
logical grid groups and skip piloted grids unless `haspilot` is included.

Cleanup conditions include: `name <regex>`, `blockslessthan <count>`,
`blocksgreaterthan <count>`, `pcugreaterthan <pcu>`, `pculessthan <pcu>`,
`hasgridtype <large|small|ship|static>`, `hasownertype <npc|player|nobody>`,
`haspower` / `nopower`, `insideplanet`, `playerdistancelessthan <meters>` /
`playerdistancegreaterthan <meters>`, `poweredgriddistancegreaterthan <meters>`,
`centerdistancelessthan <meters>` / `centerdistancegreaterthan <meters>`,
`ownedby <player|identityId|steamId|nobody|npc|pirates>`, `hastype <type>` /
`notype <type>`, `hastype-fast <type[,type...]>` /
`notype-fast <type[,type...]>`, `hassubtype <subtype>` /
`nosubtype <subtype>`, `hassubtype-fast <subtype[,subtype...]>` /
`nosubtype-fast <subtype[,subtype...]>`, and `haspilot`.

| Command | Permission | Description |
|---|---|---|
| `!ess cleanup scan [conditions...]` | Admin | Count grids matching cleanup conditions. |
| `!ess cleanup list [conditions...]` | Admin | List matching grids with block count, PCU and entity id. |
| `!ess cleanup delete [conditions...]` | Admin | Delete matching grids after confirmation. |
| `!ess cleanup delete floatingobjects` | Admin | Delete all floating objects after confirmation. |
| `!ess cleanup help` | Admin | List cleanup conditions. |
| `!ess identity clean <days> [includeNpcs]` | SpaceMaster | Remove old identities and preserve/reassign their grids. `includeNpcs` defaults to `false`. |
| `!ess identity purge <days> [includeNpcs]` | SpaceMaster | Remove old identities and close grids solely owned by them. `includeNpcs` defaults to `false`. |
| `!ess identity clear <player>` | Admin | Remove one identity and close grids solely owned by it. |
| `!ess faction clean [memberCount]` | Admin | Remove factions with fewer than `memberCount` valid members (default `1`). |
| `!ess faction remove <tag>` | Admin | Remove a faction by tag. |
| `!ess faction info [tag]` | Admin | List factions and their members. |
| `!ess sandbox clean` | SpaceMaster | Clean stale identities, factions, GPS/camera/procedural data and block ownership. |

## Voxels

Voxel reset commands restore storage from the original data provider. Destructive
commands require running the same command again within 30 seconds to confirm.

| Command | Permission | Description |
|---|---|---|
| `!ess voxels reset all` | Admin | Reset all voxel maps and planets. |
| `!ess voxels cleanup asteroids` | Admin | Reset asteroid voxel maps with no grid or character nearby. |
| `!ess voxels cleanup distant [distance]` | Admin | Reset asteroid voxel maps with no grid or character within `distance` meters (default `1000`). |
| `!ess voxels reset planets` | Admin | Reset all planets. |
| `!ess voxels reset planet <planetName>` | Admin | Reset one planet matched by storage name. |
| `!ess voxels reset area <radius>` | Admin | Reset voxel damage within `radius` meters of the caller. |
| `!ess voxels reset gps <x> <y> <z> <radius>` | Admin | Reset voxel damage within `radius` meters of a GPS point. |

## Entities & grids

Find, stop, delete and power-toggle entities, plus common grid administration.
`entities delete` and `grids static large` require running the same command again
within 30 seconds to confirm. `[gridName]` defaults to the grid the caller is
looking at where supported.

| Command | Permission | Description |
|---|---|---|
| `!ess entities find <name>` | SpaceMaster | Find entities by display name or voxel storage name. |
| `!ess entities stop <entityName>` | SpaceMaster | Clear an entity's linear/angular speed. |
| `!ess entities delete <entityName>` | SpaceMaster | Delete a non-character entity after confirmation. |
| `!ess entities poweroff <gridName>` | SpaceMaster | Disable reactors, batteries, solar panels and hydrogen engines on a grid. |
| `!ess entities poweron <gridName>` | SpaceMaster | Enable reactors, batteries, solar panels and hydrogen engines on a grid. |
| `!ess entities eject <playerName|all>` | Admin | Eject one seated player, or every seated player with `all`. |
| `!ess grids list` | None | List grids owned by the caller; position/GPS output follows config. |
| `!ess grids ejectall [gridName]` | SpaceMaster | Eject all pilots from a named or targeted mechanical grid group. |
| `!ess grids stopall` | SpaceMaster | Clear speed on all non-projected grids. |
| `!ess grids static large` | SpaceMaster | Convert all large ship grids to stations after confirmation. |

## Economy

`<player>` is a player name or identity id. `*` affects all players. `onlyOnline`
limits to online players; `excludeNpcs` (default `true`) skips NPC identities.

| Command | Permission | Description |
|---|---|---|
| `!ess econ give <player> <amount> [onlyOnline] [excludeNpcs]` | Admin | Add credits. |
| `!ess econ take <player> <amount> [onlyOnline] [excludeNpcs]` | Admin | Remove credits. |
| `!ess econ set <player> <amount> [onlyOnline] [excludeNpcs]` | Admin | Set the balance. |
| `!ess econ reset <player> [onlyOnline] [excludeNpcs]` | Admin | Reset the balance to 10,000. |
| `!ess econ top [onlyOnline] [excludeNpcs]` | None | List balances, highest first. |
| `!ess econ check <player>` | None | Show a player's balance. |
| `!ess econ pay <player> <amount>` | None | Pay another online player from your account. |

## Ship fixer

Cuts and re-pastes a grid to clear physics/clang issues. Behaviour (cooldown,
confirmation window, projector handling, ejecting players) is governed by the
**Ship Fixer** config tab; the player `!ess fixship` command can be disabled
there.

| Command | Permission | Description |
|---|---|---|
| `!ess fixship [gridName]` | None | Fix the grid you are looking at, or one by name. |
| `!ess fixshipmod [gridName]` | Moderator | Fix by look target or grid name (moderator, no cooldown). |
| `!ess fixshipmodid [gridId]` | Moderator | Fix a grid by entity id. |

## PCU & ownership

Inspect ownership/authorship and transfer PCU/ownership of a grid. `[gridName]`
defaults to the grid you are looking at. Transfers respect block limits unless a
`force…` variant is used.

| Command | Permission | Description |
|---|---|---|
| `!ess pcu checkowner [gridName]` | Moderator | Report block ownership on a grid. |
| `!ess pcu checkauthor [gridName]` | Moderator | Report PCU authorship on a grid. |
| `!ess transfer [playerName] [gridName]` | SpaceMaster | Transfer PCU and ownership to a player. |
| `!ess forcetransfer [playerName] [gridName]` | SpaceMaster | Same, ignoring limits. |
| `!ess transferpcu [playerName] [gridName]` | SpaceMaster | Transfer PCU only. |
| `!ess forcetransferpcu [playerName] [gridName]` | SpaceMaster | Transfer PCU only, ignoring limits. |
| `!ess transferowner [playerName] [gridName]` | SpaceMaster | Transfer ownership only. |
| `!ess transfernobody [gridName]` | SpaceMaster | Remove PCU and ownership. |
| `!ess transferpcunobody [gridName]` | SpaceMaster | Remove PCU. |
| `!ess transferownernobody [gridName]` | SpaceMaster | Remove ownership. |
