# Essentials Documentation

Documentation for the Essentials Magnetar server plugin for Space Engineers.

## Contents

- **[Auto Commands](AutoCommands.md)** — timed, scheduled and triggered server
  command sequences: restart countdowns, cleanup passes, MOTD reminders, player
  votes and shell-script hooks.
- **[Chat Commands](Commands.md)** — reference for every `!ess …` command:
  auto-command/server control, voting, blocks, economy, ship fixer, and
  PCU/ownership.

## Configuration

All settings are edited in the Quasar web UI, generated from the plugin's
`PluginConfig` schema (`Shared/Config`). The tabs are:

| Tab | What it covers |
|---|---|
| **General** | Enable the plugin, code-change detection, matchmaking tags, grid-list output, stop-on-start. |
| **MOTD** | Connect messages and the Steam-overlay URL, with new-user variants. |
| **Cleanup** | Empty-backpack limit per player. |
| **PCU Tools** | PCU transfer limit checking (BlockLimits integration). |
| **Ship Fixer** | `fixship` cooldown, confirmation window, projector/eject behaviour. |
| **Auto Commands** | The auto-command list plus the restart/shutdown sequences and vote duration — see [Auto Commands](AutoCommands.md). |

![Config dialog example](ConfigDialogExample.png)
