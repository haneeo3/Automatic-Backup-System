## **Problems Faced and Solutions**

### **1. Python Not Found by Task Scheduler**

**Problem:**
When setting up Task Scheduler, the script didn’t run because it couldn’t find Python.

**Solution:**

* Use the **full path to the real Python executable**, e.g.:

  ```
  C:\Users\USER\AppData\Local\Programs\Python\Python313\python.exe
  ```
* Avoid the shortcut in `WindowsApps` — it doesn’t work in Task Scheduler.

---

### **2. AWS Credentials Not Configured**

**Problem:**
The script failed with errors like `botocore.exceptions.NoCredentialsError` — AWS didn’t know who I was.

**Solution:**

* Run `aws configure` in Command Prompt.
* Enter **Access Key ID**, **Secret Key**, **Default Region**, and **Output format**.
* Test manually with `aws s3 ls` to confirm credentials work.

---

### **3. Files Not Appearing in S3**

**Problem:**
Script ran but no files showed up in the bucket.

**Solution:**

* Check **bucket name** — it must match exactly in the script (`automatic-backup-haneef`).
* Ensure **folder path** is correct in the script (use raw string `r'C:\path\to\folder'` in Python).
* Make sure files are **regular files**, not shortcuts or folders.

---

### **4. Task Scheduler Runs But Nothing Happens**

**Problem:**
Task runs at scheduled time but no files are uploaded.

**Solution:**

* Make sure **Add arguments** field has your script path in quotes:

  ```
  "C:\Users\USER\Documents\automatic_backup.py"
  ```
* Make sure **Program/script** points to the real Python executable.
* Check **Start in** folder — optional but prevents path issues:

  ```
  C:\Users\USER\Documents\
  ```

---

### **5. Too Many Files / Free Tier Limit**

**Problem:**
Uploading too many large files can exceed the Free Tier limit of **5GB**.

**Solution:**

* Keep total S3 storage **≤ 5GB**.
* Optionally enable **Lifecycle rules** to automatically delete old versions after 90 days.

---

### **6. Script Errors (Permission / Read Issues)**

**Problem:**
Script crashed due to a file being open, locked, or read-only.

**Solution:**

* Ensure files in the folder are **not open in other programs** while the script runs.
* Use **error handling** in the script (already added in the logging version) — failed files are logged but script continues.

