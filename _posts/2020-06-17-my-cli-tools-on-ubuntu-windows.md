---
layout: post
title: My CLI tools for Ubuntu and Windows
tags: cli ubuntu
---

I this tutorial, I'll present some cli tools I use on Windows and Ubuntu, which make daily tasks easier when dealing with commands and keep me focusing on what is really important. Why? Your terminal can be much, MUCH more productive.

Terminals ship with tab completion, but it is not helpful. I used to add aliases, and I am tired of maintaining their configuration. Oh my ZSH is one of the more popular alternatives to the bare Bash shell. Oh my ZSH predict what you want to type, save many repetitive commands, give you a really powerful autocomplete.

For Ubuntu users, we can install it with these simple commands: 

1. install `apt install zsh` 
2. install Oh mZSH `sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

For Windows users, I suggest to use Git Bash with Windows Terminal:

1. install https://git-scm.com/downloads

2. install https://aka.ms/terminal

3. Download the latest ZSH package: https://packages.msys2.org/package/zsh?repo=msys&variant=x86_64

4. Extract the content to your Git Bash installation directory usually `C:\Program Files\Git`

5. install Oh mZSH `sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

6. edit the `~/.bashrc` file and add

   ```bas
   # Launch Zsh
   if [ -t 1 ]; then
   exec zsh
   fi
   ```

7. open Terminal and add Git Bash as a profile using this command `C:/Program Files/Git/bin/bash.exe -i -l`, starting directory `%USERPROFILE%` and the icon `C:/Program Files/Git/mingw64/share/git/git-for-windows.ico`.

Oh my ZSH has cool features and can be added as a plugin. One of my favorite plugins are `zsh-syntax-highlighting` and `zsh-autosuggestions`:

1. run `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ~/.oh-my-zsh/custom/plugins/zsh-syntax-highlighting` and add `zsh-syntax-highlighting` in your plugins list under `~/.zshrc`.

2. run `git clone https://github.com/zsh-users/zsh-autosuggestions ~/.oh-my-zsh/custom/plugins/zsh-autosuggestions` and add `zsh-autosuggestions` in your plugins list under `~/.zshrc`.

With Oh my ZSH, I use SDKMAN! SDKMAN! helps to manage parallel versions of multiple SDKs. To install it on Ubuntu or Windows, use these commands:

1. run `curl -s "https://get.sdkman.io" | bash`
2. run `source "$HOME/.sdkman/bin/sdkman-init.sh"`

Now you can install and manages multiple versions of Java, Maven, Kotlin, etc.

I hope that these tools help to reduce the time spent in the terminal :D

