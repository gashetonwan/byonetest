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

> > > script en bash para ver la ram en la polybar

```bash
echo " FreeRam:$(/usr/bin/free -h | grep "Mem" | awk '{print $7}') %{F#1bbf3e}%{u-} Cache:$(/usr/bin/free -h | grep "Mem" | awk '{print $6}') "

```
