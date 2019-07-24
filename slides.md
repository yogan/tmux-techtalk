---
title: ░▒▓ tmux Tech Talk ▓▒░
author: ⦀ Frank Blendinger ⦀
patat:
  slideLevel: 2
  margins:
    left: 2
    right: 2
---

<!-- behold, invisible space shit ahead: -->
⁤  
⁤  
⁤  

<!-- end of invisible space magic -->

```text
  ╔═════════════════════════════════════════════════════════════════╗  
  ║    ★     ༝     ☆     ﾟ    *   ✧  ⁺      ﾟ            .   ✧      ║
  ║        ☾     ﾟ ･   ﾟ __    ⁺        ✧     ★    .ﾟ    ༓        ⁺ ║
  ║ ･           ▹       / /_____ﾟ___  __  ___  __     ｡       *     ║
  ║     ✧    ⁺  ･      / __/ __ `__ \/ / / / |/_/          ✧     ｡  ║
  ║        ✮   *      / /_/ / / / / / /_/ />  <    ‥        ★       ║
  ║    ･           ﾟ  \__/_/ /_/ /_/\__,_/_/|_|   ﾟ✧       ｡        ║
  ║       ✧    ﾟ           .   ༓                ☆       ﾟ         ༝ ║
  ║ ⁺             ★  ﾟ    *          ✧      ﾟ .      *    ･         ║
  ╚═════════════════════════════════════════════════════════════════╝
```

## What is tmux?

- [tmux](https://github.com/tmux/tmux) is a terminal multiplexer
- similar to [GNU Screen](https://www.gnu.org/software/screen/)
- basically a window manager for the terminal

## Terminal, Shell, Command Line

- TODO explain this shit

# Sessions

## Session Commands

- creating
- attaching / detaching
- listing

## Session Command Syntax

Long forms:

- `tmux new-session -s <name>`
- `tmux attach-session -t <name>`
- `tmux list-lessions`

Short forms:

- `tmux new -s <name>`
- `tmux attach -t <name>`
- `tmux at -t <name>`
- `tmux ls`

# Windows, Panes

# various commands / keys

TODO: ensure those are defaults (not customized mappings)

- `<prefix>` `<?>` → `list-keys` → show all defined key bindings
  - move around cursor keys or vi-style
  - start search with `</>`

# tmuxinator

Is cool, yo. # TODO
