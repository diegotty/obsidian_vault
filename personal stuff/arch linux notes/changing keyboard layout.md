what im trying to do is actually a little bit more complex:
i want to use AltGr as a modifier, to add a 3rd layout to my layout (US) to type grave vowels. there arent layouts that do this given by xkb (us(intl) is very very close, but the vowels are acute, which is not what im looking for )

my physical keyboard layout is US. according to the wiki, this means that my AltGr key, when clicked, doesn’t call the key you’d think.
``The AltGr key on non-US keyboards calls modifier ISO_Level3_Shift. (On US keyboards, the right-alt `Alt_R` has the same function as the left-alt `Alt_L`, which makes setting the layout as US international preferable. See [#Keymap table](https://wiki.archlinux.org/title/Xmodmap#Keymap_table).)`
and also, this is the modifier keys needed to access different layers of the keyboard:
1. Key
2. Shift + Key
3. Mode_switch + Key
4. Mode_switch + key
5. ISO_Level3_Shift + key
6. ISO_Level3_Shift + Shift + key
you can use `xmodmap -pm` to show you bindings to the modifier keys/ groups the are in. 
- groups : mod1, mod2, lock, etc..
- modifier keys: Alt_L, Mode_switch, Control_L, etc….
the altGr key has the keycode 108(0x6c in binary), so look for that inside the parenthesis of the modifiers shown. i had this line:
```
mod1        Alt_L (0x40),  Alt_R (0x6c),  Alt_L (0xcc),  Meta_L (0xcd)
```
so my AltGr calls Alt_R as suggested by the wiki.
as i’m trying to use AltrGr to use my 3rd layer on the layout, i need to remap my AltGr key to call Mode_switch when pressed.
also, this is why, if you use i3wm and Mod1 as the mod key,
AltGr might work for you like Alt. as i’m trying to use AltGr as a keyboard modifier, i need to move it outside of mod5.

to create a new layout, you can use the command:
```zsh
xmodmap -pke > ~/.Xmodmap
## the ~/.Xmodmap is a canonic file 
```
you can then edit the ~/.Xmodmap file,and change they layout to your liking. every keycode has a number of values associated to it, and every column is a layer. so to change the keycode output while pressing AltGr, i changed the 3rd column on the keycodes i needed.

i also fixed the modifier keys thing here ! to change AltGr, i had to clear the group AltGr was in (mod1) and the group i wanted AltGr to be in (mod5, in my case(i’m clearing the mod5 group because i don’t need the ISO_Level_Shift key assigned to it)), 
after clearing them, i need to add the modifiers to the groups i just cleared. so i added Alt_L and Meta_L to mod1 and Meta_switch to mod5
i then mapped the keycode 108 to call Mode_switch in all 4 layers of the layout
```
clear mod1
clear mod5
add mod5 = Mode_switch
add mod1 = Alt_L Meta_L
....
keycode 108 = Mode_switch Mode_switch Mode_switch Mode_switch
```
[here is the wiki link :)](https://wiki.archlinux.org/title/Xmodmap#Reassigning_modifiers_to_keys_on_your_keyboard)
notice: do not assign ISO_Level_3_Shift and Mode_switch to the same keycode OR the same group. those modifiers are meant to be used for different things, and, in fact, can access another layer if pressed together in some keyboards.

you can now test your changes with 
```
xmodmap ~/.Xmodmap
```
this activates your custom layer for the current session.