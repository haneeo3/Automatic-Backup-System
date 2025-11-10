
```
C:\Users\USER\Documents\automatic_backup.py
```

---

```python
import boto3
import os
import datetime

# -----------------------------
# CONFIGURATION
# -----------------------------
# S3 bucket name
bucket = 'automatic-backup-haneef'

# Local folder to back up
folder = r'C:\Users\USER\Downloads\AWS'

# Log file to track backups
log_file = r'C:\Users\USER\Documents\backup_log.txt'

# -----------------------------
# CONNECT TO AWS S3
# -----------------------------
s3 = boto3.client('s3')

# -----------------------------
# BACKUP PROCESS
# -----------------------------
with open(log_file, 'a') as log:
    log.write(f"\nBackup run on {datetime.datetime.now()}\n")
    log.write("-" * 40 + "\n")

    for file in os.listdir(folder):
        filepath = os.path.join(folder, file)
        if os.path.isfile(filepath):
            try:
                # Upload file with current date prefix
                s3.upload_file(filepath, bucket, f"backup_{datetime.date.today()}_{file}")
                log.write(f"SUCCESS: Uploaded {file}\n")
                print(f"Uploaded {file}")
            except Exception as e:
                log.write(f"ERROR: {file} failed to upload -> {e}\n")
                print(f"Failed to upload {file}: {e}")

    log.write("-" * 40 + "\n")
```

---

### **How It Works**

1. Connects to **your S3 bucket** using Boto3.
2. Loops through **all files** in the folder `C:\Users\USER\Downloads\AWS`.
3. Uploads each file to S3 with a **date prefix**, e.g., `backup_2025-11-10_myfile.txt`.
4. Writes a **log entry** for each file, showing whether it uploaded successfully or failed.
5. Safe for **automation via Task Scheduler** — logs will let you check backup status anytime.

---

### **Next Steps**

1. Save this script as `automatic_backup.py`.
2. Test it manually:

```bash
python C:\Users\USER\Documents\automatic_backup.py
```

3. Verify the files appear in your **S3 bucket** and logs in `backup_log.txt`.
4. Set up **Task Scheduler** to run daily for full automation.

