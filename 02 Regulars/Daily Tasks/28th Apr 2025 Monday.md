---
tags: []
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

### LUKS encryption

[LUKS Encryption- ChatGPT](https://chatgpt.com/s/dr_680f4ee10a308191adfd23f00f2b5fe1)

[Chapter 9. Encrypting block devices using LUKS | Red Hat Product Documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/security_hardening/encrypting-block-devices-using-luks_security-hardening#encrypting-existing-data-on-a-block-device-using-luks2-with-a-detached-header_encrypting-block-devices-using-luks)

```sh
# This is used to encrypt the data with detached header i.e, the header will be stored in seperate volume
cryptsetup reencrypt --encrypt --init-only --header /home/header /dev/xvdd xvdd_encrypted
```

```
cryptsetup reencrypt --encrypt --init-only --reduce-device-size 32M /dev/mapper/vg00-lv00 lv00_encrypted

```

```sh
cryptsetup reencrypt --encrypt --init-only --reduce-device-seze 32M /dev/xvdd xvdd_encrypted
```

```passphrase
this-is-a-demo-pass-!23
```

```sh
mount /dev/mapper/xvdd_encrypted /data
```

```sh
cryptsetup reencrypt --resume-only --header /home/header /dev/xvdd

```


```sh
cryptsetup luksClose xvdd_encrypted
```

---

#### After initialization a new device is created - Reason
Got it — you want to **encrypt `/dev/xvdd`**, but **you don't want** a new separate device like `/dev/mapper/xvdd_encrypted` —  
you just want `/dev/xvdd` itself to appear encrypted and mounted on `/data`, without introducing another device mapper path.

**Problem:**  
This is _NOT_ possible directly. Here's why:

- `cryptsetup` **always** creates a mapped device like `/dev/mapper/<name>`.
- The original device `/dev/xvdd` **cannot** be used directly after encryption — Linux will _always_ decrypt to a `/dev/mapper/...` path.
- **This is by design**, because `/dev/xvdd` is the raw encrypted block device now, not usable directly anymore.
- The decrypted (usable) version **must be accessed** via `/dev/mapper/xvdd_encrypted`.

---

**Solution for your case:**  
✅ Use `/dev/mapper/xvdd_encrypted` but **mount it on `/data`** —  
✅ Ignore `/dev/xvdd` for any direct use — just manage it via `/dev/mapper/xvdd_encrypted`.  
✅ Set crypttab and fstab properly to automate everything (so system understands it).

---

### Updated steps for your setup:

Since you have already done encryption initialization, just:

1. **Format the mapped device:**
    
```bash
mkfs.ext4 /dev/mapper/xvdd_encrypted
```

2. **Create mount point:**
    
```bash
mkdir -p /data
```

3. **Mount it:**
    
```bash
mount /dev/mapper/xvdd_encrypted /data
```

---

✅ After this:

- `/data` = your encrypted volume
- `/dev/mapper/xvdd_encrypted` = decrypted view of `/dev/xvdd`

---

If you want, I can also guide you to properly configure `/etc/crypttab` and `/etc/fstab` so that:

- At boot, system decrypts `/dev/xvdd` into `/dev/mapper/xvdd_encrypted`
- Then auto-mounts `/dev/mapper/xvdd_encrypted` onto `/data`
**No manual steps at reboot needed.**

---
