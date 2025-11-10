# **Automatic S3 Backup System**

## **Project Overview**

The **Automatic S3 Backup System** is a simple, free-tier-safe project that automatically backs up files from your local machine to an AWS S3 bucket. It uses **Python** and **Boto3** to upload files, with optional automation via **Windows Task Scheduler** or **cron**.

This project demonstrates **cloud storage, automation, and version control** using AWS services — all without paying a dime, making it ideal for learning or portfolio purposes.

---

## **Features**

* Automatically uploads files from a specified local folder to **AWS S3**.
* Adds the **current date** to the file names to track backups.
* Works with **Windows Task Scheduler** or **Linux/Mac cron** for fully automated backups.
* Uses **S3 versioning** to store multiple versions of the same file safely.
* Optional: **Lifecycle rules** to delete old versions after a set number of days.
* Optional: **SNS notifications** for email alerts when a backup occurs.

---

## **Technologies Used**

| Technology                    | Purpose                                          |
| ----------------------------- | ------------------------------------------------ |
| Python                        | Programming language for the backup script       |
| Boto3                         | AWS SDK for Python — interacts with S3           |
| AWS S3                        | Cloud storage to store backup files              |
| AWS CLI                       | Configures AWS credentials on your local machine |
| Windows Task Scheduler / cron | Automates script execution                       |

---

## **Project Setup**

### **1. Prerequisites**

* Python 3.x installed (check with `python --version`)
* AWS account (Free Tier is sufficient)
* AWS CLI installed and configured
* Boto3 library installed:

  ```bash
  pip install boto3
  ```

---

### **2. Create an S3 Bucket**

1. Log in to AWS Console → search **S3** → **Create bucket**.
2. Bucket name: `automatic-backup-haneef` (or any name).
3. Region: `eu-west-2` (London) recommended.
4. Keep **Block all public access** checked.
5. Click **Create bucket**.

---

### **3. Configure AWS CLI on Your Laptop**

1. Open Command Prompt or Terminal
2. Run:

   ```bash
   aws configure
   ```
3. Enter your credentials:

   ```
   AWS Access Key ID: <your key>
   AWS Secret Access Key: <your secret>
   Default region name: eu-west-2
   Default output format: json
   ```

---

### **4. Prepare the Local Folder**

* Create a folder containing files to back up, e.g.:

  ```
  C:\Users\USER\Downloads\AWS
  ```

---

### **5. Create the Python Backup Script**

Save as `automatic_backup.py`:

```python
import boto3, os, datetime

# Connect to S3
s3 = boto3.client('s3')

# Bucket name
bucket = 'automatic-backup-haneef'

# Local folder to back up
folder = r'C:\Users\USER\Downloads\AWS'

# Loop through files
for file in os.listdir(folder):
    filepath = os.path.join(folder, file)
    if os.path.isfile(filepath):
        # Upload to S3 with current date in filename
        s3.upload_file(filepath, bucket, f"backup_{datetime.date.today()}_{file}")
        print(f"Uploaded {file}")
```

---

### **6. Test the Script**

1. Open Command Prompt and navigate to the script location:

   ```bash
   cd C:\Users\USER\Documents
   ```
2. Run the script:

   ```bash
   python automatic_backup.py
   ```
3. Check your S3 bucket — files should appear with **today’s date** in their names.

---

### **7. Automate Backups**

#### **Windows Task Scheduler**

1. Open Task Scheduler → **Create Basic Task**
2. Name: `Daily S3 Backup`
3. Trigger: **Daily**, choose a time
4. Action: **Start a Program**

   * Program/script:

     ```
     C:\Users\USER\AppData\Local\Programs\Python\Python313\python.exe
     ```
   * Add arguments:

     ```
     "C:\Users\USER\Documents\automatic_backup.py"
     ```
   * Start in:

     ```
     C:\Users\USER\Documents\
     ```
5. Click **Finish** → Task will run daily.

#### **Linux/Mac cron**

1. Open terminal → `crontab -e`
2. Add line to run daily at 2 AM:

   ```
   0 2 * * * /usr/bin/python3 /home/haneef/automatic_backup.py
   ```

---

### **8. Optional Enhancements**

* **Versioning:** Enable S3 bucket versioning to keep previous file versions.
* **Lifecycle Rules:** Automatically delete old versions after 90 days to stay within free-tier limits.
* **SNS Notifications:** Configure email alerts for each new backup.

---

## **How It Works**

1. Python script scans your local folder for files.
2. Each file is uploaded to S3 with the **current date** added to the filename.
3. If scheduled, this happens automatically every day.
4. Old backups can be managed with **versioning and lifecycle rules**.

---

## **Free Tier Safety Notes**

* Keep **total storage ≤ 5GB** to stay free.
* Avoid transitions to **Glacier** or other paid classes.
* Keep the number of requests low (< 2000 PUT requests/month).

---

## **Screenshots / Portfolio Use**

* Screenshot your S3 bucket with files labeled by date.
* Optional: screenshot Task Scheduler showing the scheduled task.

---

## **Conclusion**

This project is a simple, reliable, and free way to automatically back up your files to the cloud. It’s **fully automated**, demonstrates **AWS integration**, and is ideal for portfolios or learning cloud backup systems.

