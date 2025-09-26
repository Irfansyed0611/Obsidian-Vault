---
tags:
  - daily
  - luks
---
--------
## Task to complete

- [ ] 
- [ ]   

-----
##  Action items from daily meeting

| Task | Further Action item |
| ---- | ------------------- |
|      |                     |


----

## Notes

```sh
SIZE=$(blockdev --getsize64 /dev/xvdg) 
NEWSIZE=$((SIZE - 33554432))
NEWSIZE_K=$((NEWSIZE / 1024))
e2fsck -f /dev/xvdg
resize2fs -p /dev/xvdg ${NEWSIZE_K}K                                                              
cryptsetup reencrypt --encrypt --init-only --reduce-device-size 32M /dev/xvdg xvdg_encrypted
lsblk
mount /dev/mapper/xvdg_encrypted /data
cryptsetup reencrypt --resume-only /dev/xvdg 
umount /data
cryptsetup luksClose xvdg_encrypted
cryptsetup luksOpen /dev/xvdg xvdg_encrypted
mount /dev/mapper/xvdg_encrypted /data 
mkdir -m 700 /etc/luks-key
echo "xvdg_encrypted /dev/xvdg none luks" >> /etc/crypttab
echo "/dev/mapper/xvdg_encrypted /data ext4 defaults 0 2" >> /etc/fstab
cat /etc/fstab
umount /data 
systemctl daemon-reload
mount -a
df -Th 
dd if=/dev/urandom of=/etc/luks-keys/xvdd_key bs=32 count=1 
dd if=/dev/urandom of=/etc/luks-keys/xvdg_key bs=32 count=1
chmod 400 /etc/luks-keys//xvdg_key
cryptsetup luksAddKey /dev/xvdg /etc/luks-keys/xvdg_key                                          
echo "xvdg_encrypted /dev/xvdg /etc/luks-keys/xvdg_key luks" > /etc/crypttab                      
```
   
   ---
   
Encryption.
Working with Samseena on patching and upgradation.
Submitted 3 SOPs to Vamsi.
Authentication issues faced.





