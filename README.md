# tmux TechTalk

## What is tmux

- [tmux](https://github.com/tmux/tmux) is a terminal multiplexer
- similar to [GNU Screen](https://www.gnu.org/software/screen/)
- basically a window manager for the terminal

## Terminal, Shell, Command Line

- TODO explain this shit

## Sessions

- detaching
- listing
- (re-)attaching

### Good Practice: Naming Sessions

- `tmux new-session -s <name>`
- `tmux attach-session -t <name>`
- `tmux list-lessions`

Short forms:

- `tmux new  -s <name>`
- `tmux attach -t <name>`
- `tmux ls`

## Windows, Panes

## various commands / keys

TODO: ensure those are defaults (not customized mappings)

- `<prefix>` `<?>` → `list-keys` → show all defined key bindings
  - move around cursor keys or vi-style
  - start search with `</>`

## tmuxinator

