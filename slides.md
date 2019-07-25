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



# Terminal ⇄ Shell ⇄ Command Line ⇄ CLI ⇄ TUI ⇄ Console


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

*Command line*, *terminal*, *shell*, *console* – they are all the same, right?

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

- they offer *input* and *output* functionality like traditional *physical terminals*

- they do **not** offer any *CLI*/*prompt*/*language*/… – a *shell* does that!

- the *POSIX terminal interface* defines the expected behavior of a *Unix* terminal

    - *dumb* terminal: I/O as *character streams*

    - *capabilities*
        - *terminfo*, *termcap* (databases) / (*n*)*curses* (library)
        - *control chars* / *escape codes* (text attribs, colors, window title, …)
    - *job control*
        - `<^Z>` → `SIGTSTP`, `<^C>` → `SIGINT`
        - *terminal* handles control keys, job management is done by the *shell*
        - `fg`, `bg`, `jobs` (*shell* commands)


## Terminal Implementation / API

- Unix systems provide `tty` *devices*, controlled via `ioctl` system calls

- *terminal emulators* use those to implement their behavior
  (example: `ioctl(tty_fd, TIOCSWINSZ, &winsize);` set number of cols/rows)

- *shells* and text-based applications (*TUIs*) also use these APIs
  (often via libraries like (*n*)*curses*)

- Linux uses (UNIX 98) *pseudoterminals* (*PTYs*) via `/dev/pts/*`

- Windows added the [ConPTY API](https://devblogs.microsoft.com/commandline/windows-command-line-introducing-the-windows-pseudo-console-conpty/) in 1803
    - without that API, terminal emulators had to implement the full
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

- *Ūñíçø𝐝𝑒* support (hopefully)
- maybe even *Emoji* 💩 (not for me)
- extended term *capabilities*: 256 colors / 24 bit colors, images

- look and feel
    - mouse selection, copy / paste
    - keyboard shortcuts
    - customization (color schemes, fonts)
- handling of multiple instances (tabs)
- simplified launch of different *shells*
- integrated applications
    - e.g. *PuTTY* is both a *terminal emulator* and a *ssh client*


## Shell

- when we say *shell*, we usually mean a *command-line shell*

- a *shell* is a user interface to the operating system
    - can be used *interactively* or automated via *scripts*
    - different shells have their own syntax / scripting languages
    - examples: `bash`, `zsh`, `fish`, `PowerShell`, `cmd.exe`

- when a shell is used *interactively*, it runs within a *terminal (emulator)*
    - standard I/O streams `stdin`, `stdout`, and `stderr` are by default
      connected to the surrounding terminal
    - shell does not "see" the output of commands it runs
      → *scrollback* (output history) is a *terminal* concept!
    - *terminal control* is passed to started applications

- shells and terminals can be arbitrarily mixed):
    - `fish` shell inside an `xterm` terminal
    - `bash` shell inside a `mintty` terminal
    - `PowerShell` instance inside `ConEmu`
    - …


## Command Line / Command Line Interface (CLI)

- the *command line interface* is the main user interface of any *shell*,
  but also other programs

- shells usually print some kind of *prompt* near the place where the user
  can enter *commands*

- offers *entering* / *editing* a *command line* (often via the `readline` lib)
  that gets *interpreted* according to the *shell language* syntax

- convenience features like *completion*, *command history*, *keyboard shortcuts*, …

- *non-shell programs* can also offer their own *CLIs*

    - *REPLs* of programming languages (`node`, `python`, …)
    - database clients
    - *parameters* / *options* of a program are also a form of CLI
      (e.g. `git log --one-line -10`)


## Text-based User Interface (TUI)

- a *text-based user interface (TUI)* is a form of user interface used by
  applications that makes only use of text (including drawing characters)
  instead of graphical UI elements (in contrast to to *GUIs*)

- run inside *terminals*, usually started from a *shell*

- often fill the whole terminal

- re-create UI elements known from GUIs like menus and windows by using
  *box-drawing characters*, *foreground*/*background* colors

- can be implemented by sending (ANSI) *escape sequences* to put the cursor
  at arbitrary screen positions, set colors, etc.
    - usually via a library like *ncurses*
    - sends correct control characters depending on the used terminal

- notable examples: `vim`, `mutt`, MS-DOS Editor, Midnight Commander,
  `irssi`, `patat` (what you see right now), and also: `tmux`!


## Console

**Console is not a very well-defined term.**

The term *console* is often used to describe the combination of a physically
connected keyboard and monitor, and some form of terminal that runs in text-mode
(fullscreen, outside of a graphical desktop environment), e.g. the *Linux
virtual console*.

Another common meaning is any form of (software-based) terminal with some shell
running inside it, e.g. the *Windows Command Prompt* running `cmd.exe`.

Also referred to when an application is sending text to an output stream like
`stdout`/`stderr` ("printing something to the *console*"), which often, but not
necessarily, leads to the text being displayed in the terminal.



# So what the **%#$&** is *tmux*, then?


## *tmux* – A Window Manger for the Terminal

*tmux* is to terminal applications what a *window manager* / desktop environment
is to GUI applications.

- *multitasking*
    - run terminal applications in parallel

- *window management*
    - align applications side-by-side, in a grid, etc.
    - tmux calls those split areas *panes*
- *tabs*
    - similar to browser tabs: they occupy the whole terminal
    - can be split into *panes* (see above)
    - tmux calls those tabs *windows*
- *sessions*
    - group related applications, e.g. for different projects
    - comparable to how some people use *virtual desktops* in GUI window managers


## Layers on Layers on Layers on…

```text
 Yo dawg, I herd you like terminals, so we put a terminal in yo terminal. 
```

*tmux*, like *terminal emulators*, is a software-based terminal.

However, it is kind of special: it needs *another terminal* to run inside.
So you usually start *tmux* from a shell, running in a terminal.
To get another terminal in your terminal, yo.

It provides (one or many) *virtual terminals*, and renders them on the underlying
terminal. One of those could be *active*, meaning it will receive *keyboard input*
from the surrounding terminal.

The fancy term for this whole voodoo is *terminal multiplexing*, where *tmux*
derives its name from.


## Terminal Multiplexing

```text
   ┌──────────────────┐   ┌──────────────────┐
   │       bash       │   │       vim        │
   └─────────┬────────┘   └─────────┬────────┘
             │                      │
   ┌─────────┴────────┐   ┌─────────┴────────┐
   │ virtual terminal │   │ virtual terminal │             …
   └─────────┬────────┘   └─────────┬────────┘
             │                      │                      │
   ┌─────────┴──────────────────────┴──────────────────────┴─────────┐   
   │                              tmux                               │
   └────────────────────────────────┬────────────────────────────────┘
                                    │
                    ┌───────────────┴───────────────┐
                    │           Terminal            │
                    └───────────────────────────────┘
```



# Server, Clients, Sessions

## Sessions

The other main feature of *tmux*, beside the window management,
are *detachable sessions*.

This means that the applications currently running in *tmux'* virtual terminals
can be kept alive, even when the surrounding terminal vanishes.

Typical scenarios for this are:

- closing the graphical terminal emulator
- logging off a remote server (or loosing the connection)
- switching to another project, but easily come back to the group of currently
  running terminal applications later (without killing/restarting them)

### Concepts

- *session*: group of virtual terminals and the applications running in them
- *detaching*: dropping the connection to the underlying terminal
  (while keeping the *session* alive)
- *(re-)attaching*: resurrecting a session from the background


## Attached and Detached Sessions

```text
    ┌────────┐  ┌────────┐            ┌────────┐  ┌────────┐  ┌────────┐
    │  vim   │  │  fish  │            │  node  │  │  jest  │  │  htop  │
    └───┬────┘  └───┬────┘            └───┬────┘  └───┬────┘  └───┬────┘
        │           │                     │           │           │
    ┌───┴────┐  ┌───┴────┐            ┌───┴────┐  ┌───┴────┐  ┌───┴────┐
    │ v-term │  │ v-term │            │ v-term │  │ v-term │  │ v-term │
    └───┬────┘  └───┬────┘            └───┬────┘  └───┬────┘  └───┬────┘
        │           │                     │           │           │
   ┌────┴───────────┴──────┐         ┌────┴───────────┴───────────┴─────┐   
   │ attached tmux session │         │      detached tmux session       │
   └──────────┬────────────┘         └────────────────┬─────────────────┘
        ┌─────┴──────┐                                ×
        │  Terminal  │
        └────────────┘
```

## Client / Server

- sessions are managed by a single *server*

- displayed (i.e. *attached* session) are handled by a *client*

- client/server communication via socket (`/tmp/tmux-$UID/default`)

- usually you don't even notice that there is a server

    - server is *started* (forked) when *first session* is created
    - server *keeps running* as long as sessions *exist*
    - server is *terminated* when the last session is *destroyed*

- explicitly start the server without a session: `tmux start-server`
- explicitly kill the server (and all of its sessions): `tmux kill-server` ☠

- configuration (`~/.tmux.conf`) is read when the server is *started*
    - can be re-evaluated later with the `source-file` command



# Let's Do This!


## Controlling tmux

- tmux *CLI*
    - `tmux some-command [--maybe-with-options]`
    - can be executed in any shell – within or outside of a tmux session

- tmux *key bindings*
    - always begin with a *prefix* key combination
    - default prefix: `<^B>` (this means `<Ctrl>` + `<b>` at the same time)
    - `screen` veterans remap this to `<^A>`: `set-option -g prefix C-a`
    - in the following, `<prefix>` `<key>` means:
      press *prefix* combo (e.g. `<Ctrl>` + `<b>`), release, press `<key>`

- tmux *command prompt*: in running client, `<prefix>` `<:>`
    - enter any tmux *command* to execute it
    - has basic completion (`<tab>`) and history navigation (`<up>`/`<down>`)

- automated via *config file* (`.tmux.conf`)
    - is just a list of tmux *commands* executed sequentially
    - same syntax as in *command prompt* or when binding keys
    - evaluate a config while tmux is running: `source-file ~/.tmux.conf`


## The Basics

### Create Session, Detach, Re-Attach

- start new *tmux* session and attach: `tmux`
- single window, running a fresh instance of your default *shell*
- do shell stuff (ideally start anything that keeps running, e.g. `vim` or `top`)
- *detach*: `<prefix>` `<d>` → *tmux* prints `[detached (from session n)]`
- *re-attach*: `tmux attach` → yay, my shell thingy is alive and kicking!

### Destroy Session

- when attached, close any running application
- also close the shell (`exit` / `<^D>`) → *tmux* prints `[exited]`
- `tmux attach` → *tmux* prints `no sessions`


## Session Commands

Long forms:

- `tmux new-session -s <name>`
- `tmux attach-session -t <name>`
- `tmux list-lessions`

Short forms:

- `tmux new -s <name>`
- `tmux attach -t <name>`
- `tmux at -t <name>`
- `tmux ls`

### Session Names

- when no *session name* is passed, *tmux* will just assign numbers when
  creating sessions
- existing sessions can be rename with `rename-session`
- the `attach` command defaults to the last recently used unattached session


# Windows & Panes

## Windows

- *tmux* has a tabbed interface (similar to browsers, IDEs, etc.)
    - confusingly, *tmux* calls those tabs *windows*
    - *windows* belong to a *session*
    - the tmux *status bar* lists the windows
- each *window* has a *name*
    - default name when windows is created (usually the shell name)
    - can be renamed (`rename-window` / `<prefix>` `<,>`)
- each *window* has an *index*
    - the index defines the position in the status bar
    - index can be used to directly switch to a window
    - new windows are assigned the next free index
    - there might be "holes"


## Managing Windows

- *create* new windows
    - `<prefix>` `<c>` → `new-window`
    - starts your login shell by default

- *close* windows
    - exit all applications / shells within
    - `<prefix>` `<&>` → kill current window (prompts)
    - `kill-window -t <index>` → kill window with given index (**no prompt!**)
    - closing a window does not change indices remaining windows (→ hole)

- *navigate* between windows
    - `<prefix>` `<n>` → `next-window`
    - `<prefix>` `<p>` → `previous-window`
    - `<prefix>` `<l>` → jump to last active window
    - `<prefix>` `<0>–<9>` → jump to window with given index

- *move* windows
    - `<prefix>` `<.>` → move window (prompts for index)
    - index has to be free


## Panes

**CONTINUE** here

## SOMEWHERE IN PANES

- `<prefix>` `<!>` → `break-pane` → move *pane* to its own *window*





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
- `<esc>`/`<q>` at any point to cancel


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



# Getting Help


## Documentation

- `man tmux`
    - pretty heavy, but it's the definite reference
    - [tmux manpage](http://man.openbsd.org/OpenBSD-current/man1/tmux.1) online version

- *The Tao of tmux* E-Book (Kindle) by Tony Narlock
    - free online version: [The Tao of tmux](https://leanpub.com/the-tao-of-tmux/read)

- [tmux FAQ](https://github.com/tmux/tmux/wiki/FAQ)

### Quick Tip

- `<prefix>` `<?>` → `list-keys`
    - shows all defined *key bindings*
    - use *search* (`</>`), or just read whats available



## Selected Cool Commands

- `<prefix>` `<s>` → `choose-tree`
    - shows *tree* of *sessions*, *windows*, and *panes*
    - easily jump around
- `<prefix>` `<f>` → `find-window`
    - search for window names, titles, and *visible content*
    - same tree as in `choose-tree`, but filtered



# tmuxinator

Is cool, yo. # TODO
