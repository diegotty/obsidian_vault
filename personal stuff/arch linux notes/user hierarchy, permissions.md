# users
`cat /etc/paswd` to see all users !
### system users
users used for executing non-interactive processes (users made by things like systemd, sddm, etc)
### regular users
### superusers
root !!! the root is not restricted by permissions, he can perform changes AND change permissions of files/users

# file permissions
file systems use permissions and attributes to regulate the level of interaction that users can have with files and dirs
`ls -l` to show permissions set for the contents of a directory
```
drwxr-xr-x 2 archie archie  4096 Jul  5 21:03 Desktop
```
there are always 11 characters:

| d                   | first rwx               | second rwx             | third rwx                           | +                                      |
| ------------------- | ----------------------- | ---------------------- | ----------------------------------- | -------------------------------------- |
| d : dir<br>- : file | owner’s <br>permissions | group’s<br>permissions | all the other users’<br>permissions | alternate access<br>method (+/./space) |

## zsh
https://thevaluable.dev/zsh-install-configure-mouseless/


reads:
https://themouseless.dev/