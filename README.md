# dotfiles

My dotfiles repository.  

# Using The Repository

This repository was created as a "bare" repository, so that it can track files from the home directory.  Because of this the checkout process is a little different than usual.

## Set up a new machine with those dotfiles 

  Clone the Repository

     git clone --bare git@github.com:sorafication/dotfiles.git $HOME/.dotfiles
 

  Setup Alias 

     alias dotfiles='$(which git) --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'


  Check which files will be changed

    dotfiles status -s -uno
    dotfiles config --local status.showUntrackedFiles no


  If everything is ok to be over-written

    dotfiles checkout -b original_files -f
    dotfiles commit -a  -m 'original files'
    dotfiles checkout main


  Configure branch so that you can push files from new machine

    dotfiles push --set-upstream origin main     


Done.


## How to configure your own dotfiles repo

[Source](https://www.anand-iyer.com/blog/2018/a-simpler-way-to-manage-your-dotfiles.html)


  Create your dotfiles folder
    
    mkdir $HOME/.dotfiles/
    git init --bare $HOME/.dotfiles

  Create your alias (and save it to your .zshrc)
    
    alias dotfiles='git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
  

  Configure your Repo 

    dotfiles config --local status.showUntrackedFiles no
    dotfiles remote add origin git@github.com:YOURUSERNAME/.dotfiles.git

  Start adding your files and push them

    cd $HOME
    dotfiles add .zshrc
    dotfiles commit
    dotfiles push

  Now there are finally some files in your dotfiles repo and you can start cloning the files as mentioned on top.

