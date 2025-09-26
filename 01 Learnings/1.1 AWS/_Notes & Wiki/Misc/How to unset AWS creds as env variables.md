---
tags:
  - AWS
  - Credentials
  - clippings
---
---
## How to Unset the Variables
To remove the standard AWS credential variables, run the following commands:


```shell
unset AWS_ACCESS_KEY_ID
unset AWS_SECRET_ACCESS_KEY
unset AWS_SESSION_TOKEN
```

_(You may not have `AWS_SESSION_TOKEN` set, but it's good practice to unset it as well.)_

---

## How to Verify

To confirm that the variables have been removed, you can use `env` piped to `grep`.

```sh
env | grep AWS
```

If this command produces no output, the variables have been successfully unset from your session. ✅

---

## Important Note

Using `unset` only affects your **current terminal session**.1 If you have added the `export` commands to a shell startup file (like `.bashrc`, `.zshrc`, or `.profile`), the variables will be set again the next time you open a new terminal. To remove them permanently, you must delete those `export` lines from the relevant file.