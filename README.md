# tmux Tech Talk

Slides for a tech talk on [tmux](https://github.com/tmux/tmux).

The slides are rendered in a terminal via
[patat](https://github.com/jaspervdj/patat). The slides themselves are written
in *Markdown*, so they are also [readable right here](slides.md).

## Setup

- [install patat](https://github.com/jaspervdj/patat#installation), preferably
  from source
  - install
    [Stack](https://docs.haskellstack.org/en/stable/README/#how-to-install)
    *(Haskell package manager)*
  - `cd ~/src && git clone git@github.com:jaspervdj/patat.git`
  - `stack setup && stack install` *(this takes quite some time)*
- `patat -w slides.md`
