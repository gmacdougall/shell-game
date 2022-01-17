%title: Improve Your Shell Game
%author: Gregor MacDougall
%date: 2022-01-19

# Tools to up your command line game

Covered in this presentation

- Fuzzy finding
- Aliases
- Tools to improve your development experience

---

# Who am I?

Gregor MacDougall
Engineer @ Gusto

---

# Unix Philosophy

- Write programs that do one thing and do it well.
- Write programs to work together.
- Write programs to handle text streams, because that is a universal interface.

---

# tmux

https://github.com/tmux/tmux

A terminal multiplexer

It allows you to run multiple shells, and multiple shell sessions within a
single terminal.

If you're watching my presentation, it's the green bar at the bottom (and so
much more).

Usage:

```
  <Ctrl>+b n
  <Ctrl>+b (pane_number)
  <Ctrl>+b s
```

Plus lots more...

---

# ripgrep

https://github.com/BurntSushi/ripgrep

Find file or stdin content. Replacement for command line grep.

Features:

- Uses Rust's regex engine, which is _fast_.
- Respects gitignore to avoid useless searches

Usage:

```
  # Instead of
  grep -ril commandbar *
  git grep -il 'command.*bar'

  # Use
  rg -il 'commandbar.*bar'

  # Additional features, type filtering
  rg -il --type ts 'commandbar.*bar'
  rg -il --type ruby 'commandbar.*bar'
```

---

# fzf

https://github.com/junegunn/fzf

Command line fuzzy finder

Usage:

```
  fzf
  vim $(fzf)
  yarn jest $(git ls-files | rg '(spec|test)\.[jt]sx?' | fzf)
```

---

# bat

https://github.com/sharkdp/bat

Output files with syntax highlighting

```
  # Instead of
  cat Gemfile

  # Use
  bat Gemfile
```

---

# Integration (Part 1)

fzf + bat (for previews)
```
  fzf --preview 'bat --color=always --style=numbers {}'
```

fzf + ripgrep + bat (for live code searching)
https://github.com/junegunn/fzf/blob/master/ADVANCED.md#using-fzf-as-interative-ripgrep-launcher
```
  rfv
```

---

# fd

https://github.com/sharkdp/fd

A replacement for find.

Usage:

```
  fd
  fd use_command_bar
  fd use_command_bar -e ts
```

Integrate into fzf:
```
  export FZF_DEFAULT_COMMAND="fd --type file --color=always"
```

---

# Starship Prompt

https://starship.rs/

Cross-shell, cross-platform prompt helper

I don't use this prompt, but I recommend it.
It's probably better than your prompt.

---

# direnv

https://direnv.net/

Per directory environment variable management

## .envrc
```
  export PORT=7190
  export CONFIG__DATABASE__PASSWORD='my_postgres_password'
```

---

# tmuxp

https://github.com/tmux-python/tmuxp

Create pre-configured tmux sessions

```
  session_name: g-sup
  start_directory: ~/dev/gusto-support
  windows:
    - window_name: code
      panes:
      -
    - window_name: rails s
      panes:
      - shell_command: rs
    - window_name: rails c
      panes:
      - shell_command: rc
    - window_name: webpack
      panes:
      - shell_command: bin/webpack-dev-server
    - window_name: test
      panes:
      -
```

---

# GitHub CLI

https://cli.github.com/

Command Line Interface for GitHub

Usage:

```
  gh pr list
  gh pr checkout 123
  gh pr create
```

---

# Integration (Part 2)

GitHub CLI + fzf

```
  alias review="gh pr checkout (gh pr list | less | fzf | cut -f1)"
```

Fuzzy checkout recent branches

```
  git checkout $(git for-each-ref --sort=-committerdate refs/heads
    --format="%(refname:short)!%(committerdate:relative)!%(subject)" | column -ts'!' | fzf | cut -d' ' -f1)
```

---

# Other Stuff I Use

I recommend you try the other things I listed above.

If you want to know what else I use, but carries a significant workflow
change or has a steep learning curve, you can also check out:

- Arch Linux
- Neovim
  - Lots of plugins
- Docker + Docker Compose
- Fish Shell
  - Tide Prompt
  - fzf.fish

---

# Where do I start

https://github.com/gmacdougall/shell-game

https://github.com/gmacdougall/dotfiles
