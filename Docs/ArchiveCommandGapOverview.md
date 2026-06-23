# Archive Command Gap Overview

This overview compares chat commands found in:

- `/home/owendb/Documents/GitHub/_ARCHIVE_/essentials-torch/Essentials`
- `/home/owendb/Documents/GitHub/_ARCHIVE_/CrunchUtils/CrunchUtils`
- current Essentials command modules under `ServerPlugin/Commands`

Scan rules:

- Counted C# `[Command("...")]` attributes.
- Ignored commented-out command attributes.
- Folded Torch `[Category("x")]` into paths, so `[Category("blocks")]` plus `[Command("on type")]` becomes `blocks on type`.
- Ignored current `!ess` prefix for matching, because all current commands live under `!ess`.
- Did not expand runtime/config-defined commands, such as auto command sequences or old info-command config.

Counts from the scan:

| Source | Commands |
|---|---:|
| Current Essentials | 72 |
| Archived essentials-torch | 102 |
| Archived CrunchUtils | 83 |

## Already Covered

Current Essentials covers these old Essentials/Torch command families:

| Current command(s) | Archive source | Notes |
|---|---|---|
| `blocks on type`, `blocks on subtype`, `blocks off type`, `blocks off subtype`, `blocks remove type`, `blocks remove subtype`, `blocks on general`, `blocks off general` | essentials-torch `BlocksModule` | Directly reimplemented under `!ess blocks ...`. |
| `econ give`, `econ take`, `econ set`, `econ reset`, `econ top`, `econ check`, `econ pay` | essentials-torch `EcoModule` | Directly reimplemented under `!ess econ ...`. |
| `say` | essentials-torch `PlayerModule` | Reimplemented under `!ess say`. |
| `vote`, `vote list` | essentials-torch `VotingModule` | Reimplemented under `!ess vote ...`. |
| `runauto`, `cancelauto`, `listauto` | essentials-torch `AdminModule` | Old paths were `admin runauto`, `admin cancelauto`, `admin listauto`, `admin listrunningauto`; current commands omit `admin`. |
| `vote yes`, `vote no` | essentials-torch `VotingModule` | Old commands were root-level `yes` and `no`; current commands are namespaced. |
| `cleanup scan`, `cleanup list`, `cleanup delete`, `cleanup delete floatingobjects`, `cleanup help` | essentials-torch `CleanupModule` | Reimplemented under `!ess cleanup ...`; destructive commands require repeat-to-confirm. |
| `identity clean`, `identity purge`, `identity clear`, `faction clean`, `faction remove`, `faction info`, `sandbox clean` | essentials-torch `WorldModule` | Reimplemented under `!ess`; identity cleanup defaults to excluding NPC identities unless `includeNpcs` is `true`. |
| `entities find`, `entities stop`, `entities delete`, `entities poweroff`, `entities poweron`, `entities eject`, `grids list`, `grids ejectall`, `grids stopall`, `grids static large` | essentials-torch `EntityModule`, `GridModule` | Reimplemented under `!ess`; delete/static commands require repeat-to-confirm. |
| `stats`, `playerlist`, `mute`, `unmute`, `list mute`, `motd` | essentials-torch `AdminModule`, `PlayerModule` | Reimplemented under `!ess`; MOTD uses Plugin SDK mission screens with chat fallback. |
| `msg`, `whis` | CrunchUtils `Commands` | Reimplemented under `!ess` as private-message aliases for online players. |
| `broadcast` | CrunchUtils `Commands` | Reimplemented under `!ess broadcast`, with simpler behavior. |

Partial coverage:

| Archive command(s) | Current command(s) | Gap |
|---|---|---|
| Crunch `eco give`, `eco take`, `eco top`, `eco pay` | `econ give`, `econ take`, `econ top`, `econ pay` | Function exists, but `eco` aliases do not. |
| Crunch `eco balance` | `econ check` | Function exists under different name and permission differs: Crunch requires Admin, current allows None. |
| Crunch `eco giveplayer`, `eco takeplayer` | `econ give`, `econ take` | Function exists through generic player-targeted commands, but old aliases do not. |
| Crunch `pcucount` | `pcu checkauthor`, `pcu checkowner` | Current reports ownership/authorship; old command was player-facing connected-grid PCU count. |
| essentials-torch `grids setowner` | `transferowner`, `transfer`, `forcetransfer` | Current transfer tools cover ownership transfer, but command shape and limit checks differ. |

## Likely Missing From essentials-torch

### Admin and Server Inspection

Missing commands:

- `admin playercount`
- `admin cancelautobyindex`
- `admin set toolbar`
- `admin setrank`
- `admin reserve`
- `admin unreserve`
- `admin give`

Purpose:

- Server stats, player list/max-player adjustment, default toolbar setup, promote/reserve/mute management, and item injection.

Notes:

- `admin runauto`, `admin cancelauto`, `admin listauto`, and `admin listrunningauto` are mostly covered by current `runauto`, `cancelauto`, and `listauto`.
- `admin cancelautobyindex` is not covered.

### Player Moderation and Player Utility

Missing commands:

- `tp`
- `tpto`
- `tphere`
- `kick`
- `ban`
- `unban`

Purpose:

- Teleporting and moderation actions.

Notes:

- `kick`, `ban`, and `unban` may overlap with vanilla/admin tooling, but they are not present in current Essentials.

### Reputation Maintenance

Missing commands:

- `rep wipe`

Purpose:

- Full reputation wipe command.

Notes:

- `sandbox clean` now removes stale reputation entries, but no direct `rep wipe` command has been ported.

### Entities and Grids

Missing commands:

- `entities refresh`
- `entities kill`
- `grids export`
- `grids import`

Partially covered:

- `grids setowner` overlaps with current transfer commands.

Purpose:

- Entity resync, player kill, and grid XML export/import.

### Homes

Missing commands:

- `home add`
- `home del`
- `home list`
- `home goto`

Purpose:

- Player home teleport locations.

### Voting Admin

Missing commands:

- `vote cancel`
- `vote debug`
- `vote reset`

Renamed:

- `yes` -> current `vote yes`
- `no` -> current `vote no`

Purpose:

- Admin control/debug/reset of voting state.

### Voxels

Missing commands:

- `voxels reset all`
- `voxels cleanup asteroids`
- `voxels cleanup distant`
- `voxels reset planets`
- `voxels reset planet`
- `voxels reset area`
- `voxels reset gps`

Purpose:

- Reset voxel maps, clean asteroid storage, reset planets, and reset voxel damage in an area or GPS point.

### Info Commands

Missing commands:

- `info list`

Purpose:

- Lists old config-driven info commands.

Notes:

- Old runtime info commands are not visible from attributes, so this scan only confirms the `info list` entry point.

## Likely Missing From CrunchUtils

### Stone, Inventory, and Player Fixes

Missing commands:

- `stone`
- `togglestone`
- `removebody`
- `fixrespawn`
- `fixme`
- `prediction`
- `fillhydro`
- `giveitem`
- `cleargrid`

Purpose:

- Player stone cleanup, admin body removal, respawn/prediction fixes, fill hydrogen tanks, give items, and clear grid inventories.

### Grid Ownership, Trade, and Conversion

Missing commands:

- `admin makeship`
- `admin makestation`
- `convert`
- `claim`
- `sellgrid`
- `acceptgrid`
- `denygrid`
- `admin rename`
- `rename`
- `gridtype`

Partially covered:

- `pcucount` is partly related to current `pcu checkauthor`/`pcu checkowner`, but not equivalent.

Purpose:

- Convert grids between station/ship, player claim flow, player-to-player grid sale flow, rename grids, and show station/ship type.

Notes:

- Current transfer commands are admin tools; Crunch had player-facing claim/sell/accept/deny flows.

### Economy and Wallets

Missing or only partially covered commands:

- `eco`
- `eco balance`
- `eco top`
- `eco give`
- `eco take`
- `eco pay`
- `eco giveplayer`
- `eco takeplayer`
- `eco resetplayer`
- `eco resetbalances`
- `eco resetplayers`
- `eco resetfactions`
- `eco deposit`
- `eco withdraw`
- `eco withdrawall`
- `eco givefac`
- `eco takefac`
- `eco resetfac`
- `TestEconSync`
- `fulleconsync`
- `FullEconSync`
- `SingleEconSync`
- `singleeconsync`

Purpose:

- Old `eco` command namespace, faction wallet deposit/withdraw, mass resets, player/faction balance changes, and economy sync debug/admin commands.

Notes:

- Current Essentials uses `econ`, not `eco`.
- Current player balance commands cover player give/take/top/check/pay, but not faction wallets, withdraw/deposit, mass reset-to-zero flows, or sync commands.
- Commands labeled "admin command no use" in Crunch are likely low priority.

### Factions, War, and Reputation

Missing commands:

- `fac search`
- `tags`
- `faction rep`
- `warstatus`
- `declarewar`
- `nofriendforyou`
- `sendpeace`
- `ac`
- `fac info`
- `facinfo`
- `fac promote`
- `fac kick`
- `faction rep change`
- `player rep change`
- `resetnpcrep`
- `resetallrep`

Purpose:

- Faction search/info/tags, war declaration/peace flow, faction descriptions, faction member admin, and faction/player reputation changes/resets.

Notes:

- `facinfo`, `fac info`, and `ac` appear to overlap around faction description/info.
- `fac promote` is described as broken in Crunch source; likely skip unless behavior is re-designed.

### Identity and Admin Lookups

Missing commands:

- `getsteamid`
- `listids`
- `listnames`
- `updatename`
- `lastlogin`
- `isnpc`
- `getfacid`
- `worldpcu`

Purpose:

- Identity/Steam ID lookup, name mismatch listing, identity name update, last-login lookup, NPC detection, faction ID lookup, and world PCU totals.

### Safe Zones, GPS, and NPC Stations

Missing commands:

- `zone`
- `ez hide`
- `ez show`
- `ez delete`
- `place station`
- `fixallstations`
- `fixstation`
- `isecon`
- `sywavefix`

Purpose:

- Safezone allow/deny list editing, GPS hide/show/delete helpers, NPC economy station placement and duplicate/fix utilities.

## Suggested Port Priority

1. Done: cleanup and world maintenance: `cleanup ...`, `identity ...`, `faction clean/remove/info`, `sandbox clean`. Remaining related command: `rep wipe`.
2. Done: grid/entity admin: `entities find/delete/stop/poweron/poweroff/eject`, `grids list/ejectall/stopall/static large`. Remaining related commands: `entities refresh`, `entities kill`, `grids export/import`.
3. Done: player/admin QoL: `motd`, `playerlist`, `stats`, `mute/unmute`, private messaging aliases. Remaining related commands: `admin playercount`, `kick/ban/unban`, teleport tools.
4. Voxel tools: `voxels reset ...`, `voxels cleanup ...`.
5. Crunch player grid/economy workflows: `claim`, `sellgrid`, `acceptgrid`, `denygrid`, `rename`, `pcucount`, `eco` compatibility aliases.
6. Niche or risky tools after explicit decision: homes, NPC station commands, reputation/war system, debug economy sync commands.

## Raw Scan Pointers

Primary source files:

- Current: `ServerPlugin/Commands/*.cs`
- Current docs: `Docs/Commands.md`
- Old Essentials: `_ARCHIVE_/essentials-torch/Essentials/Commands/*.cs`
- CrunchUtils: `_ARCHIVE_/CrunchUtils/CrunchUtils/Commands.cs`
