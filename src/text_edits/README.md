# Text Editor

## Installing NvChad on WSL2 (Windows 11)

This guide provides step-by-step instructions on how to install NvChad, a modern Neovim configuration, on WSL2 in Windows 11.
Prerequisites

Before proceeding, ensure you have the following:

 - Windows Subsystem for Linux (WSL2) installed on your Windows 11 machine.
 - A Linux distribution (like Ubuntu) installed on WSL2.
 - Git installed in your WSL2 Linux distribution.
 - Neovim installed in your WSL2 Linux distribution.

Installation Steps
### 1. Open WSL2

Launch your WSL2 terminal from Windows 11.
### 2. Install Neovim (if not already installed)

```bash
sudo apt update
sudo apt install neovim
```
### 3. Clone NvChad

Clone the NvChad repository into your Neovim configuration directory.

```bash
git clone https://github.com/NvChad/NvChad ~/.config/nvim --depth 1
```
### 4. Open Neovim

Run Neovim to complete the setup.

```bash
nvim
```
NvChad will automatically install all the required plugins. Wait for the process to complete.
Post-Installation

After installation, you can start using Neovim with NvChad. Customize your setup by editing the configuration files in ~/.config/nvim.
Saving Your Configuration as a Code Repository

To save your Neovim configuration (including NvChad customizations) as a code repository, follow these steps:
### 1. Navigate to the Configuration Directory

```bash
cd ~/.config
```
### 2. Initialize a Git Repository

```bash
git init
```
### 3. Add Remote Repository (Optional)

If you have a remote repository (like on GitHub), add it as the remote.

```bash
git remote add origin <your-repository-url>
```
Replace <your-repository-url> with your actual repository URL.
### 4. Add Files to the Repository

```bash
git add nvim
```
### 5. Commit the Changes

```bash
git commit -m "Initial commit of NvChad configuration"
```
### 6. Push to Remote Repository (If applicable)

```bash
git push -u origin main
```
Replace main with your default branch name if it's different.
