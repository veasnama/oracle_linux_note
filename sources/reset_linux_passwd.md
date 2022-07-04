# Reset Linux Password 

- Press 'e' at the boot menu to change kernel parameters
- Scroll down a little bit
- Replace "ro" to "rw init=/sysroot/bin/sh"
- Press ctrl-x to continue the system startup
 At the "#" prompt, enter the following:
```
 # chroot /sysroot
 # passwd 
 # touch /.autorelabel
 # exit
 # reboot

``` 

