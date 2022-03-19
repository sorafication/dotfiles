# dotfiles with chezmoi

Instructions WIP

# iTerm2 Configuration

iTerm2 preferences are stored in a custom backup folder in iCloud.
For a new iTerm2 setup open iTerm2 --> Preferences --> General --> Preferences and check "Load preferences from a custom folder or URL".
Select the folder where your config file is and restart iTerm.


# Raycast Configuration

Raycast has a lot of custom config like global hotkeys for window management.
To export your current config open Raycast and choose 'Export Preferences & Data'.
Importing can be done via 'Import Preferences & Data'.

A current version of my own backup file is stored in personal cloud folder.

# How to edit new apps/homebrew packages

Go to your source directory of chezmoi and edit the corresponding 'run_once_*' file.
The edit can be done via regular vim. chezmoi edit is not needed as you are already in the source directory.
Do not forget to commit and push your changes.

    chezmoi cd
    vim run_once_before_10-install-packages-darwin.sh.tmpl
    chezmoi apply
    git add
    git commit -m ""
    git push
