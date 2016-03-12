---
title: 'Byobu, Tmux & Vim'
date: 2015-10-01
categories:
  - Guides
tags:
  - linux
  - shell
  - programming
---

This is a little writeup on how to get a very useful terminal setup on any Debian based Linux. Example of my setup below:

![Terminal Preview](/images/byobu.jpg){:width="600px"}

My custom .bashrc file, give me a customized prompt (current directory and size as well as previous commands exit status). In addition to this it provides a whack of aliases to make commands simpler to type, as well as some pseudo functions. Download from pastebin [HERE](http://pastebin.com/Jx8iVqE2)

------------------------------------------------------------------------------

# Installing my custom bashrc

```
sudo add-apt-repository ppa:apt-fast/stable
sudo apt-get update
sudo apt-get install apt-fast apt-file nano lynx more less tmux byobu
```

Open bashrc from pastebin above and save to ~/.bashrc Open a terminal and try it out!

------------------------------------------------------------------------------

## Useful apt-get aliases
- `install` : sudo apt-fast install
- `addrepo` : sudo add-apt-repository
- `searchIn` : apt-file search

## Useful shorthand aliases
- `murder` : 'kill -9'  
- `alert` : Useful for things like "make; alert" and when complete will pop up an on screen notification.  
- `hist` : Grep through bash history, very useful.  

## Useful pseudo programs
- `extract` : Automatically appends the needed options to tar for you.
- `netinfo` : shows network information for your system
- `downforme` : Check to see if a site is down for everyone or just me
- `cp_pro` : copy with progress bar
- `mkdir_go` : make a directory and enter into it
- `bu` : Back up a file by appending UNIXTIME.backup

------------------------------------------------------------------------------

## Solarized dark256 color scheme
- Get the color scheme for tmux [HERE](https://github.com/seebi/tmux-colors-solarized) just copy 256 pallet to ~/.tmux.conf  
- Vim plugins and highlighters [HERE](https://github.com/amix/vimrc)  
- Edit ~./vimrc and add 'set number' and 'set relativenumber'  
- Solarizer 'ls' dircolors [HERE](https://github.com/seebi/dircolors-solarized)  
- Gnome-term solerized pallet [HERE](https://github.com/Anthony25/gnome-terminal-colors-solarized). I had to select the background to be slightly darker than the tmux status bar manually in gnome-term as I did not like the blue/green default.
