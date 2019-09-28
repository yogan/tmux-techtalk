# tmux Tech Talk

A talk on **[tmux](https://github.com/tmux/tmux)**, a terminal multiplexer.  
With a not-so-short introduction to the history of terminals.

Go *[here to read the slides](slides.md)*.

## Presentation Slides Setup

The presentation is rendered in a terminal via
[patat](https://github.com/jaspervdj/patat).  
 The slides themselves are written in markdown, so they are also
 [readable right here](slides.md).

- [install patat](https://github.com/jaspervdj/patat#installation), preferably
  from source
    - install
      [Stack](https://docs.haskellstack.org/en/stable/README/#how-to-install)
      *(Haskell package manager)*
    - `cd ~/src && git clone git@github.com:jaspervdj/patat.git`
    - `stack setup && stack install` *(this takes quite some time)*
- `patat -w slides.md`

