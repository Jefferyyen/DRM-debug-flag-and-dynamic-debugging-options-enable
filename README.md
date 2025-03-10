# DRM(Direct Rendering Manager) debug flag and dynamic debugging options enable 

<br><br><br>

## 1. ctrl + alt +T open ternimel 
<br><br><br>
## 2. enable debug flag in grub
Input:

```
sudo nano /etc/default/grub  
```

find this line:

```
GRUB_COMLINE_LINUX_DEFAULT="quiet splash"   
```

change to:  
```
GRUB_COMLINE_LINUX_DEFAULT="quiet splashd drm.debug=0x10e log_buf_len=4M"  
```
<br><br><br>
## 3.Save and exit 

Save and exit: `Ctrl + X`, press `Y`, then `Enter`.
 
```
sudo update-grub  
```
<br><br><br>
## 4. Restart OS
<br><br><br>
## 5. ctrl + alt +T open ternimel and input follow commends to enables dynamic debugging options 



```
sudo bash -c 'echo -n "file drivers/acpi/* +pt" > /sys/kernel/debug/dynamic_debug/control'  
sudo bash -c 'echo -n "file drivers/base/power/* +pt" > /sys/kernel/debug/dynamic_debug/control'  
sudo bash -c 'echo -n "file kernel/power/suspend.c +pt" > /sys/kernel/debug/dynamic_debug/control'  
```
>This change will be reset if system reboot, please see Note if prefer to automatically enabling

<br><br><br>
## 6. Start issue repro 
## 7. End issue repro 
## 8. sudo dmesg > dmesg.txt  ==> copy this file 



<br>
<br>
<br>
<br>
<br>





---
# Note: Automatically Enable Dynamic Debugging via Systemd Service 
<br><br><br>
## a. Create the script：

Open a new file

```
sudo nano /usr/local/bin/dynamic_debug.sh
```

Input following content

```
#!/bin/bash
echo -n "file drivers/acpi/* +pt" > /sys/kernel/debug/dynamic_debug/control
echo -n "file drivers/base/power/* +pt" > /sys/kernel/debug/dynamic_debug/control
echo -n "file kernel/power/suspend.c +pt" > /sys/kernel/debug/dynamic_debug/control
```
Save and exit: `Ctrl + X`, press `Y`, then `Enter`.
<br><br><br>
## b. Set execution permission

```
sudo chmod +x /usr/local/bin/dynamic_debug.sh
```
<br><br><br>
## c. Create a systemd service:

Open new file

```
sudo nano /etc/systemd/system/dynamic_debug.service
```

Input the content:

```
[Unit]
Description=Set Dynamic Debugging

[Service]
Type=oneshot
ExecStart=/usr/local/bin/dynamic_debug.sh

[Install]
WantedBy=multi-user.target
```
<br><br><br>
## d. Enable and start the service

```
sudo systemctl enable dynamic_debug.service
```

```
sudo systemctl start dynamic_debug.service
```

