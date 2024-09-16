# users
`cat /etc/paswd` to see all users !
### system users
users used for executing non-interactive processes (users made by things like systemd, sddm, etc)
### regular users
you
### superusers
root !!! the root is not restricted by permissions, he can perform changes AND change permissions of files/users

# permissions
file systems use permissions and attributes to regulate the level of interaction that users can have with files and dirs
`ls -l` to show permissions set for the contents of a directory
```
drwxr-xr-x 2 archie archie  4096 Jul  5 21:03 Desktop
drwsrwxrwx+  owner  group
```
there are always 11 characters:

| d                                          | first rwx               | second rwx             | third rwx                           | +                                      |
| ------------------------------------------ | ----------------------- | ---------------------- | ----------------------------------- | -------------------------------------- |
| d : dir<br>- : file<br>l : link<br>(more…) | owner’s <br>permissions | group’s<br>permissions | all the other users’<br>permissions | alternate access<br>method (+/./space) |

what do the rwx triads mean ?

|          | read permissions(r)      | write permissions(w)                      | execute permissions(x)                           |
| -------- | ------------------------ | ----------------------------------------- | ------------------------------------------------ |
| **file** | can read file            | can modify/delete file                    | can execute the file(also need read permissions) |
| **dir**  | can view files contained | can delete dir and modify files contained | can browse dir (ls, cd, etc)                     |

## changing permissions with octal notation
change the permissions of your file in a very concise way !
the 3 digits represent in order : user, group, world
each digit can have a value between 0-7, following this logic:
read: 4 
write: 2
execute: 1
so the value of the digit is just the sum of the permissions we want to give 
```
chmod 730 dirname/filename
```
## zsh
https://thevaluable.dev/zsh-install-configure-mouseless/


reads:
https://themouseless.dev/