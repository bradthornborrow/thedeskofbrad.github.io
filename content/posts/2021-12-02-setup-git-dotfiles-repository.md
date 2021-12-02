+++
title = "Setup a Git 'dotfiles' repository"
date = 2021-12-02
tags = [ "linux", "git" ]
+++

This post covers the basic steps to setup a __Git bare repository__ for tracking local system configuration file changes. The local repository can be pushed remotely to backup or sync with other systems. This process is based on an article I found at [Atlassian](https://www.atlassian.com/git/tutorials/dotfiles).  

To set things up, run the following commands to create the bare Git repository and add the `dotfiles` alias in your system `.bashrc` file. These commands can be run individually or pasted into a single bash script.  

```
git init --bare $HOME/.dotfiles
alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'
dotfiles config --local status.showUntrackedFiles no

echo -e "\n# Add alias for git dotfiles sync command" >> $HOME/.bashrc
echo "alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'" >> $HOME/.bashrc
```

If run as a script, it must be executed using the `source` command (as shown below) or the `dotfiles` alias will not remain active in the current shell context.

```
source ./setup-dotfile-sync.sh
```

Once the local `.dotfiles` repo has been created, all the usual `git` operations are permitted, using `dotfiles` in place of `git`. For example:  

```
dotfiles status  
dotfiles add ~/.bashrc
dotfiles commit -am "Add .bashrc" 
```

To backup the files remotely, setup a remote Git repository using your platform of choice (e.g. [GitLab](https://gitlab.com)). Once this is done, use the following commands to connect and push any local changes:  

```
dotfiles remote add origin git@gitlab.com:yourname/blankrepo.git 
dotfiles push -u origin master
```

If setting up a new system and you need to pull down the previous configuration, follow the first steps above to setup `dotfile` sync. Once setup, use these commands to connect your remote repo and pull down the latest config:  

```
dotfiles remote add origin git@gitlab.com:yourname/remoterepo.git 
dotfiles checkout
```

I posted this here primarily for my own reference but if you found it helpful, you're welcome. To contact me, please use the [Contact](/contact) page, or message me on [Twitter](https://twitter.com/TheDeskofBrad).  

Take care.  
