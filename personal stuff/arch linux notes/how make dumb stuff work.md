1god i hate linux 

## xcape caps to esc

```
exec --no-startup-id xcape -e 'Caps_Lock=Escape' 
# in i3 config

setxkbmap -option 'caps:ctrl_modifier' && xcape -e 'Caps_Lock=Escape' &
in .xprofile
```