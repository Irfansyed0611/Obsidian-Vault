Installing `boto3` directly in the terminal is not supported anymore. You'll see the following error when you try `pip install boto3`
![[Pasted image 20250807165921.png | 400]]

You're encountering this error because you're trying to use `pip` to install packages into the **system Python environment**, and modern Linux distributions (especially Debian/Ubuntu-based ones) now protect the system Python from direct `pip` installations to avoid breaking OS components that depend on it.

The error message is telling you:

> ✅ **Don't install packages globally with `pip`**  
> ❌ Using `pip install boto3` directly is not allowed anymore on system Python

---

### ✅ Solution: Use a Virtual Environment

The recommended and safe way is to **create a virtual environment**.

Here’s how to fix it:
#### Step 1: Install `python3-venv` (if not already installed)
```bash
sudo apt update
sudo apt install python3-venv
```

#### Step 2: Create a Virtual Environment
Navigate to your project folder and run:
```bash
python3 -m venv venv
```
This creates a local environment in the `venv` folder.

#### Step 3: Activate the Virtual Environment
```bash
source venv/bin/activate
```

Your prompt should change to show `(venv)` like:
```
(venv) syed@ip-10-100-5-206:~/Documents/lambda$
```

#### Step 4: Now Install `boto3`
```bash
pip install boto3
```

✅ This will work now because you're installing inside an isolated environment.

#### Step 5: Run Your Code
Make sure your Lambda code (or test script) runs within this activated environment.

> 💡 Note: When deploying to AWS Lambda, you don’t need to worry about this — AWS provides the correct environment. But for **local testing**, using a virtual environment is essential.

---

### Optional: Deactivate When Done
When you're done working:
```bash
deactivate
```

### Summary

🔧 **Fix the `externally-managed-environment` error with:**

```bash
# 1. Install venv support
sudo apt install python3-venv

# 2. Create virtual environment
python3 -m venv venv

# 3. Activate it
source venv/bin/activate

# 4. Now install safely
pip install boto3
```

✅ You can now run your Python script locally without breaking your system.
