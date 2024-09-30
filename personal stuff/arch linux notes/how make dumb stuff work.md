1god i hate linux 
## caps2esc n ctrl
- install interception-tools, caps2esc
- enable udevmon.service, raise its priority
- add config to `/etc/interception/udevmon.yaml`
```
- JOB: intercept -g $DEVNODE | caps2esc | uinput -d $DEVNODE
  DEVICE:
    EVENTS:
      EV_KEY: [KEY_CAPSLOCK, KEY_ESC]
```
## ntp (time)
- install ntp package
- enable ntp service
- timedatectl set-ntp true
- datetimectl set-timezone CET
## fix screen tearing (kinda)
https://christitus.com/fix-screen-tearing-linux/


https://www.reddit.com/r/archlinux/comments/1ek0r5i/how_do_i_solve_the_pulseaudio_pipewirepulse/ 4 pulseaudio

# macbook
keyboard fix files are somewhere in /etc/modprobe.d(?)/ish