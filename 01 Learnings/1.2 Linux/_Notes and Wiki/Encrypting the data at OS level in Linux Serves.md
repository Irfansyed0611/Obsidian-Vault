## Encrypting using cryptsteup

In Linux OS level encryption of data can be done using LUKS.
LUKS (Linux Unified Key Setup) is a standard for **encrypting entire disks or partitions**.
It ensures **data at rest** is protected — no one can access it without the decryption key/passphrase.

### Three ways of using LUKS

| Encrypt During OS installation              | Can be done | Low risk  | Recommended for Production     |
| ------------------------------------------- | ----------- | --------- | ------------------------------ |
| Encrypt **separate partition** like `/data` | can be done | Low risk  | Recommended for Production     |
| Encrypt **existing root** (in-place)        | Possible    | High risk | Not recommended for production |
### How LUKS works

> There is no in-place encryption available. We have to encrypt a new partition and then add the data to it.
- Need to install tools like `cryptsetup` to encrypt a partition with a strong passphrase which will be used to decrypt the partition at boot process.
- How to setup an encrypted partition.
```shell
# Run this command and set a strong passphrase. Note: This will format the partition if some data exists.
cryptsetup luksFormat /dev/sdb1 

# Open it with
cryptsetup luksOpen /dev/sdb1 secure_data

# create a filesystem
mkfs.ext4 /dev/mapper/secure_data

# Mount it
sudo mkdir /mnt/secure_data
sudo mount /dev/mapper/secure_data /mnt/secure_data

# For auto-mount add it in /etc/crypttab and /etc/fstab

```

### When does decryption happen
- For root partitions - Decryption happens **early during boot**, inside the **initramfs** before OS loads. You’re prompted for a passphrase
- For non-root partitions - Decryption happens when `cryptsetup` runs via `crypttab` during boot or **manually after boot**.

### Challenges
- Full disk partition has a high data loss issue and if the process is not correctly followed then system might not boot-up properly.
- High downtime for servers with huge data.
- Entering passkey for remoter servers like AWS EC2 is not practical during boot process.
