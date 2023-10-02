---
layout: post
author: gashetonwan
title: Archivos de Configuracion sxhkd
---

### Configuracion de SXHKDRC

Configuracion de sxhkd en /home/<'username'>/.config/sxhkd/sxhkdrc

{% highlight java %}

# wm independent hotkeys

# terminal emulator

super + Return
gnome-terminal

# program launcher

super + d
rofi -show run

# make sxhkd reload its configuration files:

super + Escape
pkill -USR1 -x sxhkd

super + shift + f
firefox

#
super + shift + h
firejail /opt/firefox/firefox

#steam(dota2)
super + shift + g
flatpak run com.valvesoftware.Steam

#
# bspwm hotkeys
#

# quit/restart bspwm

super + alt + {q,r}
bspc {quit,wm -r}

# close and kill

super + {\_,shift + }w
bspc node -{c,k}

# alternate between the tiled and monocle layout

super + m
bspc desktop -l next

# send the newest marked node to the newest preselected node

super + y
bspc node newest.marked.local -n newest.!automatic.local

# swap the current node and the biggest window

super + g
bspc node -s biggest.window

# state/flags
#
# set the window state

super + {t,shift + t,s,f}
bspc node -t {tiled,pseudo_tiled,floating,fullscreen}

# set the node flags

super + ctrl + {m,x,y,z}
bspc node -g {marked,locked,sticky,private}

# focus/swap
#
# focus the node in the given direction

super + {\_,shift + }{Left,Down,Up,Right}
bspc node -{f,s} {west,south,north,east}

# focus the node for the given path jump

super + {p,b,comma,period}
bspc node -f @{parent,brother,first,second}

# focus the next/previous window in the current desktop

super + {\_,shift + }c
bspc node -f {next,prev}.local.!hidden.window

# focus the next/previous desktop in the current monitor

super + bracket{left,right}
bspc desktop -f {prev,next}.local

# focus the last node/desktop

super + {grave,Tab}
bspc {node,desktop} -f last

# focus the older or newer node in the focus history

super + {o,i}
bspc wm -h off; \
 bspc node {older,newer} -f; \
 bspc wm -h on

# focus or send to the given desktop

super + {\_,shift + }{1-9,0}
bspc {desktop -f,node -d} '^{1-9,10}'

# preselect
#
# preselect the direction

super + ctrl + alt + {Left,Down,Up,Right}
bspc node -p {west,south,north,east}

# preselect the ratio

super + ctrl + {1-9}
bspc node -o 0.{1-9}

# cancel the preselection for the focused node

super + ctrl + alt + space
bspc node -p cancel

# cancel the preselection for the focused desktop

super + ctrl + shift + space
bspc query -N -d | xargs -I id -n 1 bspc node id -p cancel

# move a floating window

super + ctrl + {Left,Down,Up,Right}
bspc node -v {-20 0,0 20,0 -20,20 0}

# resize a floating window

alt + super + {Left,Down,Up,Right}
/home/franco/.config/bspwm/scripts/bspwm_resize {west,south,north,east}
#screenshot
super + k
scrot -q 100 "$HOME/Escritorio/screen/shot-%Y-%m-%dT%H%M%S.png"

super + z
zoom-client
#----------------------------------------------------------------------------#
{% endhighlight %}

### Bspwm_resize

{% highlight bash %}
#!/usr/bin/env bash

if bspc query -N -n focused.floating > /dev/null; then
step=20
else
step=100
fi

case "$1" in
        west) dir=right; falldir=left; x="-$step"; y=0;;
east) dir=right; falldir=left; x="$step"; y=0;;
        north) dir=top; falldir=bottom; x=0; y="-$step";;
south) dir=top; falldir=bottom; x=0; y="$step";;
esac

bspc node -z "$dir" "$x" "$y" || bspc node -z "$falldir" "$x" "$y"

{% endhighlight %}

### Configuraciones de polybar

> script en bash para ver la ram en la polybar

```bash
echo " FreeRam:$(/usr/bin/free -h | grep "Mem" | awk '{print $7}') %{F#1bbf3e}%{u-} Cache:$(/usr/bin/free -h | grep "Mem" | awk '{print $6}') "
```

> script en bash para ver la conexion de red en la polybar
{% highlight bash %}
#!/bin/sh
 
echo "%{F#ff0000} %{F#ff0000}$(/usr/sbin/ifconfig enp3s0 | grep "inet " | awk '{print $2}')%{u-}"
{% endhighlight %}


> script en bash para ver la conexion de red de la vpn HackTheBox en la polybar
{% highlight bash %}
#!/bin/sh

IFACE=$(/usr/sbin/ifconfig | grep tun0 | awk '{print $1}' | tr -d ':')

if [ "$IFACE" = "tun0" ]; then
	echo "%{F#aeff9f} %{F#aeff9f}$(/usr/sbin/ifconfig tun0 | grep "inet " | awk '{print $2}')%{u-}"
else
	echo "%{F#aeff9f}%{u-} Disconnected"
fi
{% endhighlight %}
### Configuraciones de sxhkd

>todavia probando

{% highlight bash %}


# Enable Powerlevel10k instant prompt. Should stay close to the top of ~/.zshrc.
# Initialization code that may require console input (password prompts, [y/n]
# confirmations, etc.) must go above this block; everything else may go below.
if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

# If you come from bash you might have to change your $PATH.
# export PATH=$HOME/bin:/usr/local/bin:$PATH

# Path to your oh-my-zsh installation.
export ZSH="$HOME/.oh-my-zsh"

# Set name of the theme to load --- if set to "random", it will
# load a random theme each time oh-my-zsh is loaded, in which case,
# to know which specific one was loaded, run: echo $RANDOM_THEME
# See https://github.com/ohmyzsh/ohmyzsh/wiki/Themes

# Set list of themes to pick from when loading at random
# Setting this variable when ZSH_THEME=random will cause zsh to load
# a theme from this variable instead of looking in $ZSH/themes/
# If set to an empty array, this variable will have no effect.
# ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" )

# Uncomment the following line to use case-sensitive completion.
# CASE_SENSITIVE="true"


typeset -g POWERLEVEL9K_INSTANT_PROMPT=quiet

# Uncomment the following line to use hyphen-insensitive completion.
# Case-sensitive completion must be off. _ and - will be interchangeable.
# HYPHEN_INSENSITIVE="true"

# Uncomment one of the following lines to change the auto-update behavior
# zstyle ':omz:update' mode disabled  # disable automatic updates
# zstyle ':omz:update' mode auto      # update automatically without asking
# zstyle ':omz:update' mode reminder  # just remind me to update when it's time

# Uncomment the following line to change how often to auto-update (in days).
# zstyle ':omz:update' frequency 13

# Uncomment the following line if pasting URLs and other text is messed up.
# DISABLE_MAGIC_FUNCTIONS="true"

# Uncomment the following line to disable colors in ls.
# DISABLE_LS_COLORS="true"

# Uncomment the following line to disable auto-setting terminal title.
# DISABLE_AUTO_TITLE="true"

# Uncomment the following line to enable command auto-correction.
# ENABLE_CORRECTION="true"

# Uncomment the following line to display red dots whilst waiting for completion.
# You can also set it to another string to have that shown instead of the default red dots.
# e.g. COMPLETION_WAITING_DOTS="%F{yellow}waiting...%f"
# Caution: this setting can cause issues with multiline prompts in zsh < 5.7.1 (see #5765)
# COMPLETION_WAITING_DOTS="true"

# Uncomment the following line if you want to disable marking untracked files
# under VCS as dirty. This makes repository status check for large repositories
# much, much faster.
# DISABLE_UNTRACKED_FILES_DIRTY="true"

# Uncomment the following line if you want to change the command execution time
# stamp shown in the history command output.
# You can set one of the optional three formats:
# "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
# or set a custom format using the strftime function format specifications,
# see 'man strftime' for details.
# HIST_STAMPS="mm/dd/yyyy"

# Would you like to use another custom folder than $ZSH/custom?
# ZSH_CUSTOM=/path/to/new-custom-folder

# Which plugins would you like to load?
# Standard plugins can be found in $ZSH/plugins/
# Custom plugins may be added to $ZSH_CUSTOM/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(git)

source $ZSH/oh-my-zsh.sh

# User configuration

alias tecladoes='sudo setxkbmap -layout 'es''
alias tecladous='sudo setxkbmap -layout 'us''
alias ls='lsd -lh --color=auto'
alias l='lsd'
alias la='ls -la'
alias dir='dir --color=auto'
alias vdir='vdir --color=auto'
alias grep='grep --color=auto'
alias fgrep='fgrep --color=auto'
alias egrep='egrep --color=auto'
alias v='nvim'
alias b='lvim'
alias t='tmux'
alias c='cd ..'
alias d='cd'
alias k='kill -9 -1'
alias p='poweroff'
alias e='exit'
alias cat='bat'


# export MANPATH="/usr/local/man:$MANPATH"


plugins=( ... web-search)
source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh
source /usr/share/zsh-web-search/web-search.zsh

# You may need to manually set your language environment
# export LANG=en_US.UTF-8

# Preferred editor for local and remote sessions
# if [[ -n $SSH_CONNECTION ]]; then
#   export EDITOR='vim'
# else
#   export EDITOR='mvim'
# fi


# zstyle ':completion:*' auto-description 'specify: %d'
# zstyle ':completion:*' completer _expand _complete _correct _approximate
zstyle ':completion:*' format 'Completing %d'
zstyle ':completion:*' group-name ''
zstyle ':completion:*' menu select=2
eval "$(dircolors -b)"
zstyle ':completion:*:default' list-colors ${(s.:.)LS_COLORS}
zstyle ':completion:*' list-colors ''
zstyle ':completion:*' list-prompt %SAt %p: Hit TAB for more, or the character to insert%s
zstyle ':completion:*' matcher-list '' 'm:{a-z}={A-Z}' 'm:{a-zA-Z}={A-Za-z}' 'r:|[._-]=* r:|=* l:|=*'
zstyle ':completion:*' menu select=long
zstyle ':completion:*' select-prompt %SScrolling active: current selection at %p%s
zstyle ':completion:*' use-compctl false
zstyle ':completion:*' verbose true

zstyle ':completion:*:*:kill:*:processes' list-colors '=(#b) #([0-9]#)*=0=01;31'
zstyle ':completion:*:kill:*' command 'ps -u $USER -o pid,%cpu,tty,cputime,cmd'


# Compilation flags
# export ARCHFLAGS="-arch x86_64"

# Set personal aliases, overriding those provided by oh-my-zsh libs,
# plugins, and themes. Aliases can be placed here, though oh-my-zsh
# users are encouraged to define aliases within the ZSH_CUSTOM folder.
# For a full list of active aliases, run `alias`.
#
# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"
source ~/powerlevel10k/powerlevel10k.zsh-theme

# To customize prompt, run `p10k configure` or edit ~/.p10k.zsh.
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh

# Change cursor shape for different vi modes.
function zle-keymap-select {
  if [[ $KEYMAP == vicmd ]] || [[ $1 = 'block' ]]; then
    echo -ne '\e[1 q'
  elif [[ $KEYMAP == main ]] || [[ $KEYMAP == viins ]] || [[ $KEYMAP = '' ]] || [[ $1 = 'beam' ]]; then
    echo -ne '\e[5 q'
  fi
}
zle -N zle-keymap-select

# Start with beam shape cursor on zsh startup and after every command.
zle-line-init() { zle-keymap-select 'beam'}

##############################
#

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

typeset -g POWERLEVEL9K_INSTANT_PROMPT=quiet
{% endhighlight %}
