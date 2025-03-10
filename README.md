# drm-debug-flag-enable
enable drm log 


#1.ctrl + alt +T open ternimel 
#2. enable debug flag in grub
$ sudo nano /etc/default/grub
find this line: 
GRUB_COMLINE_LINUX_DEFAULT="quiet splash" 
change to:
GRUB_COMLINE_LINUX_DEFAULT="quiet splashd drm.debug=0x10e log_buf_len=4M"
#3.Save and exit 
Ctrl +X => Y and click enter to save
$sudo update-grub
#4.Restart OS
#5. ctrl + alt +T open ternimel and input follow commends
$sudo bash -c 'echo -n "file drivers/acpi/* +pt" > /sys/kernel/debug/dynamic_debug/control'
$sudo bash -c 'echo -n "file drivers/base/power/* +pt" > /sys/kernel/debug/dynamic_debug/control'
$sudo bash -c 'echo -n "file kernel/power/suspend.c +pt" > /sys/kernel/debug/dynamic_debug/control'
#6. Start issue repro 
#7. End issue repro 
#8. sudo dmesg > dmesg.txt  ==> copy this file 
