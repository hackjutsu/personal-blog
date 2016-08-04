# Linux Virtual Memory Management

title:  Linux Virtual Memory Management
date: 2016-06-20 18:00:00
tags:
- Linux

---
<img src="http://www.serverpoint.com/images-en/index-vps-linux.png" style="max-width: 550px;"/>
This post is about techniques to manage a Linux system's virtual memory. The first section discusses commands to check the system's space usage. The second section shows techniques to use swap files to increase system's virtaul memory.

<!--more-->

## Checking Space Usage
### Using `free` Command
The free command is used to display the amount of free and used system memory. Using the free command with -h option, which displays output in a human readable format.
![](http://i.imgur.com/CNhZMDZ.png)

From the output above, you can see that the last line provides information about the system swap space. More usage of `free` command can be found at:  [10 free Commands to Check Memory Usage in Linux](http://www.tecmint.com/check-memory-usage-in-linux/).

### Using `atop` Command
The `atop` command is a system monitor that reports about activities of various processes. But importantly it also shows information about free and used memory space.
```bash
atop
```
![](http://i.imgur.com/lJWIqIi.png)

### Using `df` Command
This command helps to  report file system disk space usage.
```bash
df -h
```
![](http://i.imgur.com/OuwGNQ6.png)


### Using `swap` Command
This command helps you to specify the devices on which paging and swapping will be done and we shall look at few important options.

To view all devices marked as swap in the `/etc/fstab` file you can use the `--all` option. Though devices that are already working as swap space are skipped.
```bash
swapon --all
```
If you want to view a summary of swap space usage by device, use the `--summary` option as follows.
```bash
swapon --summary
```

## How To Add Swap Files on Ubuntu 14.04
**Original post :** [How To Add Swap on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04)

One of the easiest way of increasing the responsiveness of your server and guarding against out of memory errors in your applications is to add some swap space. Swap is an area on a hard drive that has been designated as a place where the operating system can temporarily store data that it can no longer hold in RAM.

### Create a Swap File
One quick way of getting the same file is by using the `fallocate` program. This command creates a file of a preallocated size instantly, without actually having to write dummy contents.
```bash
sudo fallocate -l 4G /swapfile
```
The prompt will be returned to you almost immediately. We can verify that the correct amount of space was reserved by typing:
```bash
ls -lh /swapfile
```
```
-rw-r--r-- 1 root root 4.0G Apr 28 17:19 /swapfile
```
As you can see, our file is created with the correct amount of space set aside.

### Enabling the Swap File
Right now, our file is created, but our system does not know that this is supposed to be used for swap. We need to tell our system to format this file as swap and then enable it.

Before we do that though, we need to adjust the permissions on our file so that it isn't readable by anyone besides root. Allowing other users to read or write to this file would be a huge security risk. We can lock down the permissions by typing:
```bash
sudo chmod 600 /swapfile
```
Verify that the file has the correct permissions by typing:
```bash
ls -lh /swapfile
```
```
-rw------- 1 root root 4.0G Apr 28 17:19 /swapfile
```
As you can see, only the columns for the root user have the read and write flags enabled.

Now that our file is more secure, we can tell our system to set up the swap space by typing:
```bash
sudo mkswap /swapfile
```
```
Setting up swapspace version 1, size = 4194300 KiB
no label, UUID=e2f1e9cf-c0a9-4ed4-b8ab-714b8a7d6944
```
Our file is now ready to be used as a swap space. We can enable this by typing:
```bash
sudo swapon /swapfile
```
We can verify that the procedure was successful by checking whether our system reports swap space now:
```bash
sudo swapon -s
```
```
Filename                Type        Size    Used    Priority
/swapfile               file        4194300 0       -1
```
We have a new swap file here. We can use the free utility again to corroborate our findings:
```bash
free -m
```
```
             total       used       free     shared    buffers     cached
Mem:          3953        101       3851          0          5         30
-/+ buffers/cache:         66       3887
Swap:         4095          0       4095
```
Our swap has been set up successfully and our operating system will begin to use it as necessary.

### Make the Swap File Permanent
We have our swap file enabled, but when we reboot, the server will not automatically enable the file. We can change that though by modifying the `/etc/fstab` file.

Edit the file with root privileges in your text editor.  At the bottom of the file, you need to add a line that will tell the operating system to automatically use the file you created:
```
/swapfile   none    swap    sw    0   0
```
Save and close the file when you are finished.

## Resource
[Check Swap Space in Linux](http://www.wikihow.com/Check-Swap-Space-in-Linux)
[How to Change Swap Size on Ubuntu 14-04](https://www.digitalocean.com/community/questions/how-to-change-swap-size-on-ubuntu-14-04)
[Linux Check Swap Usage Command](http://www.cyberciti.biz/faq/linux-check-swap-usage-command/)


