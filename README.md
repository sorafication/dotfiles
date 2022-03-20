# Personal dotfiles with chezmoi
https://www.chezmoi.io

# Install from scratch on a new device

## without brew, binary install 

    sh -c "$(curl -fsLS chezmoi.io/get)" -- init --apply sorafication

## with brew

    brew install chezmoi
    chezmoi init https://github.com/sorafication/dotfiles.git
    chezmoi diff
    chezmoi apply

## as a one-liner

    chezmoi init --apply --verbose https://github.com/sorafication/dotfiles.git

First all "run once_" scripts will run and install all used homebrew and appstore apps.
Afterwards personal config files will be installed to home dir.

# Additional Configuration

## iTerm2

iTerm2 preferences are stored in a custom backup folder in iCloud.
For a new iTerm2 setup open

    iTerm2 --> Preferences --> General --> Preferences 

and check "Load preferences from a custom folder or URL".
Select the folder where your config file is and restart iTerm.


## Raycast 

Raycast has a lot of custom config like global hotkeys for window management.
To export your current config open Raycast and choose 'Export Preferences & Data'.
Importing can be done via 'Import Preferences & Data'.

A current version of my own backup file is stored in personal cloud folder.

# Daily Setup
## How to edit new apps/homebrew packages

Go to your source directory of chezmoi and edit the corresponding 'run_once_*' file.
The edit can be done via regular vim. chezmoi edit is not needed as you are already in the source directory.
Do not forget to commit and push your changes.

    chezmoi-cd
    vim run_once_before_10-install-packages-darwin.sh.tmpl
    chezmoi apply
    git add
    git commit -m ""
    git push

## How to change config files and deploy on other machines

Do not change directly config files with your editor!

You can change your config in the source directory (chezmoi cd) or via:

     chezmoi edit ~/.zshrc or 
     chezmoi edit --apply ~/.zshrc

This will edit automatically the file in the source file and update changes to git.
--apply will automatically apply those changes. 

Some Personal aliases:

    cm -> chezmoi
    cmcd -> chezmoi cd
    cmedit -> chezmoi edit --apply
