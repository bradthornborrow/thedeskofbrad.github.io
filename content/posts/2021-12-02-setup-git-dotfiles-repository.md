+++
title = "Setup a Git 'dotfiles' repository"
date = 2021-12-02
tags = [ "linux", "git" ]
+++

This post covers the basic steps to setup a __Git bare repository__ for tracking and backing up local system config files (a.k.a. `dotfiles`). The process is based on an article I found posted at [Atlassian](https://www.atlassian.com/git/tutorials/dotfiles) which goes into much greater detail.  

To set things up, use the following script to create a bare Git repository and add the `dotfiles` alias to your system `.bashrc` file. These commands could also be run individually if preferred.

```bash
#!/bin/sh
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

Once the `.dotfiles` repo has been created, all the usual `git` operations are permitted, using `dotfiles` in place of `git`. For example:  

```
dotfiles status  
dotfiles add ~/.bashrc
dotfiles commit -am "Add .bashrc" 
```

To backup the config remotely, setup a Git repository on your platform of choice (e.g. [GitLab](https://gitlab.com)). Once this is done, the following commands will connect your local repo and push the contents to the remote.  

```
dotfiles remote add origin git@gitlab.com:yourname/blank-repo.git 
dotfiles push -u origin master
```

When setting up a new system, you can pull down the config using a similar process. First, follow the initial steps to setup the bare repo and `dotfile` sync alias. After this is done, these commands will connect the remote repository and pull down the config.  

```
dotfiles remote add origin git@gitlab.com:yourname/repo-name.git 
dotfiles checkout
```

Once this has been checked out locally, everything works the same to add, update or delete configuration files and push the changes remotely.

I drafted this post primarily for my own reference but if you found it helpful, you're welcome. To contact me, please use the [Contact](/contact) page, or message me on [Twitter](https://twitter.com/TheDeskofBrad).  

Take care.  
