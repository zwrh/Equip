# Equip macOS

Ansible automation for equipping a fresh macOS installation.

## Segments

- `packages`: command-line tools, runtimes, and libraries
- `apps`: graphical macOS applications
- `rice`: appearance, Dock, preferences, and dotfiles

The `packages` segment installs Homebrew when it is missing, updates Homebrew,
upgrades installed formulae and casks only when outdated items are found, and
installs the selected command-line package list. The initial list contains tmux
and Homebrew Vim.
The `apps` segment installs Brave, Spotify, Ghostty, Discord, Visual Studio Code,
and Microsoft Outlook when they are missing.
The `rice` segment pins Discord, Spotify, Brave, Ghostty, Visual Studio Code,
and Microsoft Outlook in that order; leaves Finder, Trash, and the Dock's folder
section intact; disables the suggested/recent-app section; and minimizes windows
into their application's Dock icon instead of creating separate thumbnails.
The `tmux-config` slice populates `~/.tmux.conf` and makes new windows and panes
inherit the active pane's working directory.
The `vim-config` slice populates `~/.vimrc` from the shared cross-platform static
files directory.

Run every segment:

```sh
ansible-playbook site.yml
```

Run one segment:

```sh
ansible-playbook site.yml --tags packages
ansible-playbook site.yml --tags apps
ansible-playbook site.yml --tags rice
ansible-playbook site.yml --tags homebrew
ansible-playbook site.yml --tags cli-packages
ansible-playbook site.yml --tags tmux-config
ansible-playbook site.yml --tags vim-config
ansible-playbook site.yml --tags applications
ansible-playbook site.yml --tags dock
```

Combine segments with a comma-separated tag list:

```sh
ansible-playbook site.yml --tags packages,apps
```
