# Dotfiles Manager

A Bash wrapper for **GNU Stow** designed to manage both user (`$HOME`) and system (`/etc`) configuration files. It automates moving files into a central repository and manages the pointers (symlinks) from system back to the main dotfiles repository.

## Features
- **Adopt (`-a`)**: Moves a real file/folder into your dotfiles repo and replaces the original with a symlink.
- **Remove (`-r`)**: Deletes the symlinks and moves the real files back to their original system locations.
- **Sync (`-s`)**: Forces the system to point to your dotfiles (ideal for updates or new machine setups).
- **Status List (`-l`)**: Displays all packages and checks if they are correctly linked (✅) or missing (❌).
- **Auto-Backup**: Any local files that conflict with your dotfiles during a sync are moved to `/tmp` for safety.
- **System Support**: Automatically handles files in `/etc` or `/usr` using `sudo`.

## Prerequisites
- **GNU Stow**: `sudo pacman -S stow`
- **Bash**: (Standard on Arch Linux)

## Installation

1. Create your dotfiles directory (default is `~/.dotfiles`):
   ```bash
   mkdir -p ~/.dotfiles
   ```

2. Place the `dotfiles` script into your local bin folder and paste the contents from dotfiles.
   ```bash
   mkdir -p ~/.local/bin
   cp dotfiles ~/.local/bin/
   chmod +x ~/.local/bin/dotfiles
   ```

3. **For Fish users**, ensure `~/.local/bin` is in your PATH:
   ```fish
   fish_add_path ~/.local/bin
   ```

## Usage

### 1. Adopt a new configuration
Move your current Neovim config into a package named "nvim":
```bash
dotfiles -a nvim ~/.config/nvim
```

### 2. List all packages and status
Check which packages are properly linked from the system to your repo:
```bash
dotfiles -l
```

### 3. Sync/Install packages on a different machine
Ensures the system points to your dotfiles. If a real file exists where a link should be, it moves the file to `/tmp` and creates the link:
```bash
dotfiles -sa
```
*Note: `-sa` syncs all packages. You can also sync a specific one: `dotfiles -s bash`.*

### 4. Restore files (Un-adopt)
Move files back from the dotfiles repo to the system and remove the package:
```bash
dotfiles -r nvim
```

### 5. Help
```bash
dotfiles --help
```

## How Sync Works
When you run `dotfiles -s`, the script:
1. Scans the system for files that should be symlinks but are currently real files/folders.
2. Moves those conflicting files to `/tmp/dotfiles_backup_[timestamp]` (it will tell you the exact path).
3. Creates fresh symlinks in the system pointing back to your dotfiles repo.

### Final Tip
Ensure your script file is named exactly `dotfiles` (no `.sh` extension) inside `~/.local/bin` so you can call it just by typing `dotfiles` in your terminal. I also have it under an alias `d` so it's super easy to type.
