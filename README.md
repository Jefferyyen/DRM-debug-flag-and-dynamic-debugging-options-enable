# drm-debug-flag-enable



### 1. ctrl + alt +T open ternimel 
### 2. enable debug flag in grub
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
### 3.Save and exit 
Ctrl +X => Y and click enter to save  
```
sudo update-grub  
```
### 4.Restart OS
### 5. ctrl + alt +T open ternimel and input follow commends
```
sudo bash -c 'echo -n "file drivers/acpi/* +pt" > /sys/kernel/debug/dynamic_debug/control'  
sudo bash -c 'echo -n "file drivers/base/power/* +pt" > /sys/kernel/debug/dynamic_debug/control'  
sudo bash -c 'echo -n "file kernel/power/suspend.c +pt" > /sys/kernel/debug/dynamic_debug/control'  
```
### 6. Start issue repro 
### 7. End issue repro 
### 8. sudo dmesg > dmesg.txt  ==> copy this file 




# 您可以創建一個 Systemd 服務，更現代化也更靈活。

創建一個自定義腳本：

```
sudo nano /usr/local/bin/dynamic_debug.sh
```
在腳本中寫入命令：
```
#!/bin/bash
echo -n "file drivers/acpi/* +pt" > /sys/kernel/debug/dynamic_debug/control
echo -n "file drivers/base/power/* +pt" > /sys/kernel/debug/dynamic_debug/control
echo -n "file kernel/power/suspend.c +pt" > /sys/kernel/debug/dynamic_debug/control
```
保存並讓腳本可執行：

```
sudo chmod +x /usr/local/bin/dynamic_debug.sh
```
創建一個 systemd 服務：

```
sudo nano /etc/systemd/system/dynamic_debug.service
```
內容如下：
```
[Unit]
Description=Set Dynamic Debugging

[Service]
Type=oneshot
ExecStart=/usr/local/bin/dynamic_debug.sh

[Install]
WantedBy=multi-user.target
```
啟用並啟動服務：

```
sudo systemctl enable dynamic_debug.service
```
```
sudo systemctl start dynamic_debug.service
```
以上方法可以在系統啟動時自動設置你的動態調試選項。選擇方法取決於你的系統環境和配置習慣。Systemd 方法通常是更推薦的方式，因為它是現代 Linux 的標準初始化管理器，提供了更多的控制和靈活性。
