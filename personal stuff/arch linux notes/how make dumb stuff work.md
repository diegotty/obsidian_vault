1god i hate linux 

## xcape caps to esc

```
exec --no-startup-id xcape -e 'Caps_Lock=Escape' 
# in i3 config

setxkbmap -option 'caps:ctrl_modifier' && xcape -e 'Caps_Lock=Escape' &
in .xprofile
```

## ntp (time)
- install ntp package
- enable ntp service
- timedatectl set-ntp true
- datetimectl set-timezone CET
## fix screen tearing (kinda)
https://christitus.com/fix-screen-tearing-linux/


https://www.reddit.com/r/archlinux/comments/1ek0r5i/how_do_i_solve_the_pulseaudio_pipewirepulse/ 4 pulseaudio