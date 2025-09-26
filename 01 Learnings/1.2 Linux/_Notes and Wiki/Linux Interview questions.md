---
tags:
---


- **Question 1:** How to set a username and password to never expire?
    - **Solution:** Use the command `chage -M -1 username`. \[[03:00](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=180)]
    
- **Question 2:** Why can't `/etc/passwd` and `/etc/shadow` files be merged into one?
    - **Solution:**
        - `/etc/passwd` is a text file readable by any application. Merging it with `/etc/shadow` would expose hashed passwords to potential attackers. \[[03:51](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=231)]
        - `/etc/shadow`, containing hashed passwords, is only accessible by the root user, enhancing security. \[[05:15](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=315)]
        
- **Question 3:** How to list all open files by a particular process ID (PID)?
    - **Solution:** Use the command `lsof -p PID`. \[[06:22](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=382)]
    
- **Question 4:** What are the reasons for being unable to unmount a file system?
    - **Solution:**
        - You are in the same directory. \[[07:40](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=460)]
        - Users are present in the directory and using its content. Use `fuser -c -u /dev/sda`. \[[08:04](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=484)]
        - Files are open in that directory. Use `lsof` and the partition name. \[[08:43](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=523)]
        
- **Question 5:** What could be the reason if a server takes more time after a reboot?
    - **Solution:** The file system may be corrupt. \[[09:18](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=558)]
    
- **Question 6:** We are trying to create a file under any partition, but we are getting a permission denied alert. What could be the reason?
    - **Solution:** You are running out of inodes. Use `df -i` to check inode usage. \[[11:01](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=661)]
        - If 100% of inodes are used:
            - Find and delete or move unwanted files. \[[14:35](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=875)]
            - Find and delete or move unwanted large files. \[[14:45](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=885)]
            
- **Question 7:** How to check the kernel routing table information?
    - **Solution:** Use one of the following commands: `route -n`, `netstat -rn`, or `ip route`. \[[15:58](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=958)]
    
- **Question 8:** How to set a sticky bit, and what is the difference between small 's' and capital 'S'?
    - **Solution:**
        - Set sticky bit: `chmod o+t directory` (symbolic) or `chmod 1757 directory` (numerical). \[[16:46](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=1006)]
        - Small 's': setuid and executable. \[[18:22](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=1102)]
        - Capital 'S': setuid and non-executable. \[[18:30](https://www.youtube.com/watch?v=cFhWlBkeGxA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=1&ab_channel=Engr.AbhishekRoshan&t=1110)]
        
* **Question 9:** Which file is used to specify the default gateway?
    * The file `/etc/sysconfig/Network` is used. Inside this file, there's an `ifcfg` file containing IP address information, including details for network interfaces like `eth1`, `eth2`, and `eth0` \[[01:13](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=73)\]. This file also contains subnet mask and other network-related information \[[01:48](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=108)\].

* **Question 10:** In RHEL7, how to switch between two run levels?
    * Run levels represent different states of the Linux operating system \[[02:32](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=152)\].
    * To switch between runlevels, you can use commands like `systemctl isolate multi-user.target` \[[03:04](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=184)\], `systemctl isolate runlevel3.target` \[[03:12](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=192)\], and `systemctl set-default multi-user.target` \[[03:20](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=200)\].
    * `systemctl get-default` can also be used \[[03:30](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=210)\].

* **Question 11:** What is NFS? How to configure it? What is the port number that NFS uses?
    * NFS stands for Network File System, used for network file sharing \[[05:26](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=326)\].
    * To configure NFS, you need to configure both the NFS server and the NFS client \[[05:52](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=352)\].
        * **NFS Server Configuration:**
            * Install the `nfs-utils` package \[[06:18](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=378)\].
            * Create a directory on a partition and add data to it \[[06:31](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=391)\].
            * Export the directory by editing the `/etc/exports` file and using the `exportfs` command \[[07:10](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=430)\].
            * Restart the service and make it permanent by adding it to `/etc/fstab` \[[08:12](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=492)\].
        * **NFS Client Configuration:**
            * Install the NFS package \[[09:53](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=593)\].
            * Start the NFS service \[[09:59](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=599)\].
            * Use the `showmount` command to check exported directories \[[09:59](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=599)\].
            * Make a directory and mount the NFS directory to it \[[10:13](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=613)\].
            * Add data and verify it's updated on the server side \[[10:18](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=618)\].
    * The port number used by NFS is 2049 \[[10:40](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=640)\].

* **Question 12:** What is the nice value and how to set a nice value for particular processes?
    * The nice value represents the priority of a process, ranging from -20 to 19 \[[10:53](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=653)\].
    * A lower (more negative) nice value means higher priority, while a higher (more positive) value means lower priority \[[12:34](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=754)\].
    * To increase the priority of a process, use the `nice` command with a negative value, for example, `nice -n -1 command` \[[13:04](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=784)\].

* **Question 13:** What is the difference between FTP and TFTP?
    * FTP (File Transfer Protocol) and TFTP (Trivial File Transfer Protocol) are both used for file transfer \[[13:32](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=812)\].
    * FTP uses ports 20 and 21 and the TCP protocol, while TFTP uses port 69 and the UDP protocol \[[14:35](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=875)\].
    * FTP is more complex and requires authentication, whereas TFTP does not \[[14:42](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=882)\].

* **Question 14:** How to extend the file system in Linux?
    * To extend the file system, use Logical Volume Management (LVM) \[[16:06](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=966)\].
    * The command to extend is `lvextend -L +1G /dev/your_volume_group/your_logical_volume` \[[17:31](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1051)\].
    * After extending, use `resize2fs /dev/your_volume_group/your_logical_volume` to update the file system \[[18:51](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1131)\].

* **Question 15:** How will you roll back the packages after patching?
    * Use `yum history info 8` to see a list of recent commands \[[21:40](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1300)\].
    * Then, use `yum history undo 8` to roll back a specific installation \[[21:46](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1306)\].

* **Question 16:** What is rsync and what is the Syntax for rsync command?
    * `rsync` is used for backing up and mirroring directories and files between servers \[[22:09](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1329)\].
    * Unlike `scp`, `rsync` resumes copying from where it left off if the connection is interrupted \[[22:52](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1372)\].
    * The syntax is `rsync [options] [source] [destination_ip:location]` \[[25:20](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1520)\].
    * For example: `rsync -rv /source_directory/ destination_ip:/destination_location/` \[[25:34](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1534)\].

* **Question 17:** How to reduce the file system in Linux? Can file system reduction be done online or not?
    * Use `lvreduce` to reduce the file system size \[[27:11](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1631)\].
    * File system reduction cannot be done online; it requires downtime \[[27:37](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1657)\].
    * Steps to reduce:
        * Check the size of the LV using `df -h` \[[29:16](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1756)\].
        * Unmount the logical volume using `umount` \[[29:27](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1767)\].
        * Organize the data using `e2fsck -f /dev/your_volume_group/your_logical_volume` \[[29:33](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1773)\].
        * Update the file system using `resize2fs /dev/your_volume_group/your_logical_volume 300M` (where 300M is the approximate size after reduction) \[[30:26](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1826)\].
        * Reduce the size using `lvreduce -L -200M /dev/your_volume_group/your_logical_volume` \[[31:46](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1906)\].
        * Mount the file system and verify the reduction using `df -h` \[[32:04](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1924)\].

* **Question 18:** Uptime command shows the load average of the system but we can see total three values of load average is there any specific meaning of those values?
    * The `uptime` command displays three load average values, representing the load over the last 1, 5, and 15 minutes \[[32:35](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=1955)\].
    * These values indicate the computational work done by the system, reflecting the load on the RAM \[[33:33](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=2013)\].
    * For example, a load average of 1.05, 0.7, and 5.90 indicates the system was overloaded by 5% in the last minute \[[35:35](https://www.youtube.com/watch?v=b-m0Oz_8W3k&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=2&ab_channel=Engr.AbhishekRoshan&t=2135)\].

- **Question 19:** How to set the numbering of lines for a file using VI editor?
    - **Answer:** Use the command `:set nu` in command mode within the VI editor \[[02:44](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=164)].
    
- **Question 20:** How to scan for newly attached disks in Linux?
    - **Answer:** Use the command `echo "---" > /sys/class/scsi_host/host0/scan` (and repeat for other hosts like host1, host2, etc. as needed) \[[04:05](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=245)].
    
- **Question 21:** How many fields are there in the `/etc/fstab` file, and what does each field mean?
    - **Answer:** There are six fields:
        - Device name \[[07:04](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=424)]
        - Mount point \[[07:16](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=436)]
        - Type of file system \[[07:33](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=453)]
        - Mount options \[[07:45](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=465)]
        - Dumping option (for backup) \[[07:52](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=472)]
        - Checking sequence \[[08:30](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=510)]
        
- **Question 22:** What is the difference between a soft link and a hard link, and how do you create them?
    - **Answer:**
        - Soft links are like shortcuts, while hard links are like backups \[[09:07](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=547)].
        - Soft links can span partitions; hard links cannot \[[11:04](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=664)].
        - Soft links have different inode numbers from the original file; hard links share the same inode number \[[11:41](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=701)].
        - If the original file is deleted, a soft link breaks, but a hard link still contains the data \[[13:07](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=787)].
        - **Creating Soft Link:** `ln -s <source_directory/file_name> <destination>` \[[14:54](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=894)]
        - **Creating Hard Link:** `ln <source_file> <link_name>` \[[15:15](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=915)]
        
- **Question 23:** Can we create a soft link across the partition?
    - **Answer:** Yes, because soft links are like shortcuts, and their inode numbers differ from the original file \[[15:52](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=952)].
    
- **Question 24:** What is the location of NFS shared info?
    - **Answer:** `/etc/fstab` \[[16:38](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=998)]
    
- **Question 25:** How to check if a port is listening in Linux?
    - **Answer:** Use the command `netstat -anp | grep <port_number>` (e.g., `grep 80` for port 80) \[[17:40](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=1060)].
    
- **Question 26:** What is the difference between the `du` and `df` commands?
    - **Answer:**
        - `du` (disk usage) shows the space used by files within a specific directory \[[18:38](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=1118)].
        - `df` (disk free) shows the overall disk space usage and free space for file systems \[[20:03](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=1203)].
        
- **Question 27:** What is the concept behind journaling, and what are its pros and cons?
    - **Answer:** Journaling is a feature that tracks file system changes to prevent corruption in case of a system crash \[[21:01](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=1261)].
    
- **Question 28:** While doing patching, suppose one package is not allowing you to do patch which command would you execute and how will you troubleshoot?
    - **Answer:** `rpm -ivh <package_name> --nodeps` \[[24:33](https://www.youtube.com/watch?v=eGDAocvkKcA&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=3&ab_channel=Engr.AbhishekRoshan&t=1473)]
    
- **Question 29:** What is the booting process of Linux?
    - The booting process involves several stages \[[02:39](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=159)]:
        - **BIOS (Basic Input/Output System)**: Performs system integrity checks and searches for a bootloader \[[03:47](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=227)].
        - **MBR (Master Boot Record)**: Located in the first sector of the bootable disk, it's 512 bytes divided into three parts: primary boot loader, partition table information, and MBR validation check \[[04:05](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=245)].
        - **GRUB (Grand Unified Bootloader)**: Allows selection of kernel images if multiple are installed. The kernel information is located in `/boot/grub/grub.conf` \[[06:17](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=377)].
        - **Kernel**: Mounts the root file system as specified in `grub.conf` and executes the `/sbin/init` program. It also uses `initrd` (initial RAM disk) as a temporary root file system \[[10:59](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=659)].
        - **Init**: The `/etc/inittab` file contains run level information (0-6), with level 5 being the typical GUI-supported mode \[[13:07](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=787)].
        - **Run Level**: Executes scripts in directories like `/etc/rc.d/rc0.d`, `/etc/rc.d/rc1.d`, etc., to start services \[[14:46](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=886)].
        
- **Question 30:** What is the meaning of jcpu and pcpu in W command?
    - `jcpu` **total CPU time** used by all processes attached to the current terminal (TTY), including background and foreground processes. \[[17:05](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1025)].
    - `pcpu` CPU time used by the current active process \[[17:22](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1042)].
    
- **Question 31:** What happens when you type an URL in the browser and press enter?
    - The browser checks its cache for the DNS record \[[19:15](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1155)].
    - If not found, the ISP's DNS server queries for the IP address \[[20:32](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1232)].
    - The browser initiates a TCP connection with the server \[[20:52](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1252)].
    - The browser sends an HTTP request to the web server \[[21:16](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1276)].
    - The server handles the request and sends back an HTTP response \[[21:39](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1299)].
    - The browser displays the HTML content (web page) \[[24:10](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1450)].
    
- **Question 32:** What is the difference between RPM and YUM? How to check the package for a particular command?
    - Both are software package management tools \[[26:16](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1576)].
    - RPM (Red Hat Package Manager) is a powerful tool for installing, uninstalling, verifying, querying, and updating software packages \[[26:36](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1596)].
    - YUM (Yellow Dog Updater, Modified) also manages packages, but requires configuration \[[29:18](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1758)].
    - To check if a package is installed using RPM: `rpm -qa <package_name>` \[[26:56](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1616)].
    - To check if a package is installed using YUM: `yum list installed <package_name>` \[[31:17](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1877)].
    
- **Question 33:** What is ACL and how to set a ACL for a particular user to provide a particular file/directory access?
    - ACL (Access Control List) provides fine-grained discretionary access control for files and directories at the file system level \[[31:51](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=1911)].
    - Check if ACL is applied: `getfacl <directory/file_name>` \[[35:23](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2123)].
    - Set ACL: `setfacl [options] <argument> <file/directory>` \[[35:53](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2153)].
    
- **Question 34:** What is inode?
    - An inode (index node) is a unique identifier for a specific piece of metadata on a given file system \[[36:36](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2196)].
    - Check inode information: `df -i` \[[39:16](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2356)].
    - Check inode number for a specific file: `ls -i` \[[39:56](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2396)].
    
- **Question 35:** Can we schedule two second in crontab?
    - No, crontab only supports a minimum of 60 seconds (1 minute) \[[40:35](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2435)].
    - Crontab entries have five fields: minute, hour, day of month, month, and day of week \[[41:40](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2500)].
    
- **Question 36:** In production environment any server is rebooted automatically how will you troubleshoot that server is rebooted by someone or it's rebooted automatically if it automatically then how and where you are going to check the logs?
    - Check reboot history: `last reboot | less` \[[44:35](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2675)].
    - Check logs in: `/var/log/cpi`, `/var/log/message`, `/var/log/syslog` \[[44:53](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2693)].
    
- **Question 37:** Which command is used to change the user password expiration limit?
    - `chage -E <date> <username>` \[[45:54](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2754)].
    
- **Question 38:** How to extract a single file from tar archive?
    - `tar -xpzf <tar_file.tar.gz> <single_file>` \[[46:12](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2772)].
    
- **Question 39:** Which command is used to see the memory?
    - `free -g` (for GB) or `free -m` (for MB) \[[47:02](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2822)].
    
- **Question 40:** Which command is used to see the architecture of a Linux server?
    - `uname -a` \[[49:09](https://www.youtube.com/watch?v=g0nnBZHVanc&list=PLPVaGLSxvigJfF9WWW0hD-yhMWq40ZY_G&index=4&ab_channel=Engr.AbhishekRoshan&t=2949)].

