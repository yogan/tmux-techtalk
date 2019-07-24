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



# Terminal ⇄ Shell ⇄ Command Line ⇄ CLI ⇄ Console


## **TERM**inology

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

Command line, terminal, shell, console – they are all the same, right?

Well, actually…


## Terminals: History

- provides *input*/*output* to a computer system
    - external hardware device
    - connection typically via serial port or null modem cables
- originated in the 60s (IBM / DEC mainframes)
    - *hard-copy* terminals: typewriters / printers (*output* ≅ paper)
    - *TTY* actually stands for *TeleTYpewriter*
- shift to *visual display units* (VDUs) in late 60s / early 70s
    - nicknamed "glass TTYs"
    - possibility to set *cursor position*
    - introduction of *control codes*:
        - `^M`: carriage return, `^J`: line-feed, `^G`: bell
        - *escape sequences*, like *ANSI escape sequences*:
            - `ESC 4 ; 9 H`: move cursor to row 4, column 9


## Terminals: History (cont.)

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
    - more crazy stuff, but we'll have to skip that today…


## Terminal Emulators

- when we say *terminal* today, we usually mean a *terminal emulator*

- the *POSIX terminal interface* defines the expected behavior of a *Unix* terminal
    - *dumb* terminal: I/O as *character streams*

    - *capabilities*
        - *terminfo*, *termcap* (databases) / (*n*)*curses* (library)
        - *control chars* / *escape codes* (text attribs, colors, window title, …)
    - *job control*
        - `<^Z>` → `SIGTSTP`, `<^C>` → `SIGINT`
        - `fg`, `bg` (*shell* commands)


## Terminal Emulators (cont.)

Terminal emulators implement the features of physical terminals (and more).

- interpretation of *control characters* / *escape sequences*

- in Unix, there are *pseudoterminals* (*PTYs*), which abstract this
    (Linux: UNIX 98 pseudoterminals `/dev/pts/*`)

- Window added the [ConPTY API](https://devblogs.microsoft.com/commandline/windows-command-line-introducing-the-windows-pseudo-console-conpty/) in 1803
    - without that API, terminal emulators had to implement the
        pseudoterminal functionality on their own

## stty

- Unix tool `stty` to get/set terminal capabilities
- read `man 1 stty` if you want to understand its cryptic output and commands

```sh
> stty -a
speed 38400 baud; rows 77; columns 65; line = 0;
intr = ^C; quit = ^\; erase = ^?; kill = ^U; eof = ^D; # …
-parenb -parodd -cmspar cs8 -hupcl -cstopb cread -imaxbel iutf8 # …
```

If you just want something to play around with, try:

```sh
> stty -a
> stty cols 20
> stty -a
```

**Note:** *you will probably never ever have to deal with this, but it's cool. ;-)*


## Notable Terminal Emulators

- *Linux*: Linux console, xterm, (u)rxvt, GNOME Terminal, konsole
- *macOS*: Terminal, iTerm2, Terminator
- *Windows*: (Windows Console), Windows Terminal, mintty, PuTTY, ConEmu

### Distinctive Features

- *Ūñíçø𝐝𝑒* support (hopefully), maybe even Emoji 💩 (not for me)
- extended term caps: 256 colors / 24 bit colors, images
- look and feel
    - mouse selection / copy / paste
    - keyboard shortcuts
    - customization (color schemes, fonts)
- handling of multiple instances (tabs)
- simplified launch of different *shells*
- integrated applications
    - e.g. *PuTTY* is both a *terminal emulator* and a *ssh client*


## Shell

- a *shell* is a user interface to the operating system
    - can be used *interactively* or automated via *scripts*
    - different shells have their own syntax / scripting languages
    - examples: `bash`, `zsh`, `fish`, `PowerShell`, `cmd.exe`

- when a shell is used *interactively*, it runs *inside a terminal (emulator)*
    - standard I/O streams `stdin`, `stdout`, and `stderr` are by default
      connected to the surrounding terminal

- shells and terminals can be arbitrarily mixed):
    - `fish` shell inside an `xterm` terminal
    - `bash` shell inside an `mintty` terminal
    - `PowerShell` instance inside `ConEmu`
    - …


## Command Line / Command Line Interface (CLI)

- the *command line* is the main user interface of any *shell*

- offers *entering* / *editing* text (often via the `readline` lib) that gets
  interpreted according to the shell syntax

- non-shell programms can also offer their own *CLIs*
    - *REPLs* of programming languages (`node`, `python`, …)
    - *parameters* / *options* of a program are also a form of CLI
      (e.g. `git log --one-line -10`)


## Console

A not well-defined term.

Often means the combination of a physically connected keyboard and monitor, and
some form of terminal that runs in text-mode (fullscreen, outside of a graphical
desktop environment), e.g. the *Linux virtual console*.

Also commonly used to describe any form of terminal with some shell running
inside it, e.g. the *Windows Command Prompt* running `cmd.exe`.

Also referred to when an application is sending text to an output stream like
`stdout`/`stderr` ("printing something to the *console*"), which often, but not
necessarily, leads to the text being displayed in the terminal.


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
