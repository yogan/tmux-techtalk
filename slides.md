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
- a window manager for the terminal
    - split windows
    - layouts
- detachable sessions
    - keep shell programs running
        - on a server, after logging off
        - locally, after closing a terminal
    - re-attach later, find everything as you left it
- scripting / automation / customization / taka-taka



# Terminal ⇄ Shell ⇄ Command Line


## *$***TERM**inology

```text
         _________
        / ======= \
       / __________\
      |  _________  |
      | |>_       | |
      | |         | |
      | |_________| |
      \=____________/
      / """"""""""" \
     / ::::::::::::: \
    (_________________)    


```


## Terminals

### History

- provides *input*/*output* to a computer system
    - external hardware device
    - connection typically via serial port or null modem cables
- originated in the 60s (IBM / DEC mainframes)
    - *hard-copy* terminals: typewriters / printers (*output* ≅ paper)
    - *TTY* actually stands for *TeleTYpewriter*
- shift to *visual display units* (VDUs) in late 60s / early 70s
    - nicknamed "glass TTYs"
    - possibly to set *cursor position*
    - introduction of *control codes*:
        - `^M`: carriage return, `^J`: line-feed, `^G`: bell
        - *escape sequences*, like *ANSI escape sequences*:
            - `ESC 4 ; 9 H`: move cursor to row 4, column 9


## Terminal History (cont.)

- dozens of terminal manufacturers
    - Lear-Siegler, Hewlett Packard, IBM, DEC, Televideo, …
    - different incompatible command sequences
    - *Unix* systems introduce `termcap`/`terminfo` files and
      the `$TERM` variable to deal with this mess
- DEC *VT100* (1978)
    - one of the most common terminals
    - "advanced" features:
        - text formatting: blink, bold, reverse video, underline
        - box-drawing characters: `┘ ┐ ┌ └ ┼ ─ ├ ┤ ┴ ┬ │`
    - command codes compliant with *ANSI X3.64*
    - emulated by pretty much every modern terminal emulator
- DEC *VT220* (1983)
    - more crazy stuff, but we'll have to skip that…


## Modern Terminals (TTYs)

TODO cont. here

- when we say *terminal* today, we usually mean *terminal emulator*
- software implementations (VT220???) TODO
- notable examples:
    - *Linux*: Linux console, xterm, (u)rxvt, GNOME Terminal, konsole
    - *macOS*: Terminal, iTerm2, Terminator
    - *Windows*: Windows Console, Windows Terminal, mintty, PuTTY, ConEmu


## Shell

TODO


## Command Line

TODO



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



# Buffers


## Paste Buffers

How to make delicious copypasta using only your keyboard (completely organic).

- tmux has its own paste buffers to store text
    - those are *not* related to your OS clipboard!
    - copying does not overwrite, but add a new buffer (stack-like)
- a window can be in two modes
    - *default* *⁤* → everything is passed to the terminal
    - *copy mode* → allows selection of the terminal content

## *Copy* Text into a *tmux Buffer*

- switch to *copy mode*: `<prefix>` `<[>`
- *move cursor* to mark *start* position
- `<space>` to *start selection*
- *move cursor* to mark *end* position
- `<enter>` to copy selection into *buffer*
- `<esc>` at any point to cancel


## *Paste* Text from a *tmux Buffer*

- `<prefix>` `<]>` → *insert buffer* at cursor position
- pretty much as if you would type the pasted text by hand
- make sure that whatever runs in the active windows can deal with
  what you paste
    - vim in normal mode might freak out
    - pasting random stuff into a (root?) shell might hurt


## *Managing* Buffers

- `<prefix>` `<#>` → *list* buffers *(pretty boring)*
- `<prefix>` `<=>` → *choose* buffer **(awesome!)**
    - `<up>`/`<down>`, `<k>`/`<j>` → go through buffers
    - content of selected buffer shown in bottom pane
    - `<enter>` → *paste*
    - `<d>` → *delete buffer* *(no undo!)*


## Copypasta a la *vi*

- enable super powers with `set-window-option -g mode-keys vi`
    - ideally globally in `.tmux.conf`
- use crazy cursor *movements*
    - `<h>`, `<j>`, `<k>`, `<l>`, …
    - `<w>`, `<b>`, `<0>`, `<$>`, …
    - `<^D>`, `<^U>`, `<{>`, `<}>`, …
- use `<V>` instead of `<space>` to start *line selection* mode
- use `</>` to *search*
    - type search text, `<enter>` to start
    - `<n>`/`<N>` = next/prev result
    - unfortunately cancels selection mode

### Copypasta della *Emacs*

- also exists ¯\\\_(ツ)\_/¯


## Pro Tip

All tmux commands that produce output (e.g. `list-keys`, `list-windows`, …)
always start in *copy mode*. This is especially handy as you can start searching
right away with `</>` (no prefix required).



# tmuxinator

Is cool, yo. # TODO
