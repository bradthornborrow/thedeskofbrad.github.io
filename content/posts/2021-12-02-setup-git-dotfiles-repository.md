+++
title = "Setup a Git 'dotfiles' repository"
date = 2021-12-02
tags = [ "linux", "git" ]
+++

This post covers the the basic steps to backup and sync your Linux system config files (a.k.a. `dotfiles`) using a __Git bare repository__. The process is based on an article I found at [Atlassian](https://www.atlassian.com/git/tutorials/dotfiles) which details the full process.  

To set things up, use the following console commands to create a bare Git repository and the `dotfiles` alias.

```bash
git init --bare $HOME/.dotfiles
alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
dotfiles config --local status.showUntrackedFiles no
```

Once this is done, the following code must be added to the `.bashrc` file in your home directory. This command recreates the `dotfiles` alias on each login.

```bash
# Add alias for git dotfiles sync command
alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
```

After the `.dotfiles` repo has been created, all standard git operations are permitted, using `dotfiles` instead of `git`. For example, the following commands will add the `.bashrc` file to your `dotfiles` repository and commit the changes:  

```
dotfiles add ~/.bashrc
dotfiles commit -am "Add .bashrc" 
```

To backup your config files remotely and sync with other systems, first setup a Git repository on your platform of choice (e.g. [GitHub](https://github.com)). Once this is done, the following commands will connect the remote repo and push the contents.  

```
dotfiles remote add origin git@github.com:yourname/blank-repo.git 
dotfiles push -u origin master
```

When setting up new systems, you can pull down your config files using a similar process. First, follow the initial steps to setup the bare repo and `dotfile` sync alias. After this is done, follow these commands to connect the remote repository and pull down the last saved config.  

```
dotfiles remote add origin git@github.com:yourname/repo-name.git 
dotfiles checkout
```

Once checked out locally, everything works the same to add, update or delete configuration files and push the changes. This will also allow you to pull down any changes to your other systems, keeping them in sync.

I drafted this post primarily for my own reference but if you found it helpful, you're welcome ツ. To contact me, please use the [Contact](/contact) page, or send me a direct message on [Twitter](https://twitter.com/TheDeskofBrad).  

Take care.  
