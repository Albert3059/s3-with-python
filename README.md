
---

# AWS S3 Bucket Creation and Deletion Python Scripts

This repository contains Python scripts to create and delete an S3 bucket on AWS. The scripts use the `boto3` library to interact with AWS services.

## Prerequisites

1. **AWS Account**: You need an AWS account with appropriate permissions to create and delete S3 buckets.
2. **AWS CLI Configuration**: Ensure you have configured your AWS credentials using the AWS CLI or by setting up environment variables.
   - Install the AWS CLI: [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)
   - Configure AWS credentials:
     ```bash
     aws configure
     ```
3. **Python Environment**: Ensure Python 3.x is installed.
4. **Boto3 Library**: Install the `boto3` library using pip:
   ```bash
   pip install boto3
   ```

---

## Script 1: Create S3 Bucket and Upload a File

This script creates an S3 bucket and uploads a file to it.

### Steps to Run the Script

1. Open the `create_bucket.py` script and update the following variables:
   - `BUCKET_NAME`: Replace `"my-unique-bucket-mk"` with your desired bucket name.
   - `REGION`: Replace `"us-east-1"` with your desired AWS region.
   - `FILE_PATH`: Replace `r"C:\Users\username\Pictures\Camera Roll\****.jpg"` with the path to the file you want to upload.
   - `S3_KEY`: Replace `"*****.jpg"` with the desired filename in S3.
3. Run the script:
   ```bash
   python create_bucket.py
   ```
4. If successful, the script will:
   - Create the S3 bucket.
   - Upload the specified file to the bucket.

### Script Code

```python
import boto3
from botocore.exceptions import NoCredentialsError, PartialCredentialsError, ClientError

# AWS S3 Configuration
BUCKET_NAME = "my-unique-bucket-mk"
REGION = "us-east-1"
FILE_PATH = r"C:\Users\username\Pictures\Camera Roll\****.jpg"
S3_KEY = "****.jpg"  # The filename in S3

def create_s3_bucket():
    """Create an S3 bucket in the specified region."""
    s3 = boto3.client("s3", region_name=REGION)
    
    try:
        if REGION == "us-east-1":
            s3.create_bucket(Bucket=BUCKET_NAME)
        else:
            s3.create_bucket(
                Bucket=BUCKET_NAME,
                CreateBucketConfiguration={"LocationConstraint": REGION},
            )
        print(f"✅ Bucket '{BUCKET_NAME}' created successfully.")
    except ClientError as e:
        print(f"⚠️ Error creating bucket: {e}")

def upload_file_to_s3():
    """Upload a file to the S3 bucket."""
    s3 = boto3.client("s3")
    
    try:
        s3.upload_file(FILE_PATH, BUCKET_NAME, S3_KEY)
        print(f"✅ File '{FILE_PATH}' uploaded to '{BUCKET_NAME}' as '{S3_KEY}'.")
    except FileNotFoundError:
        print("⚠️ Error: The file was not found.")
    except NoCredentialsError:
        print("⚠️ Error: AWS credentials not found.")
    except PartialCredentialsError:
        print("⚠️ Error: Incomplete AWS credentials.")
    except ClientError as e:
        print(f"⚠️ AWS Error: {e}")

if __name__ == "__main__":
    create_s3_bucket()
    upload_file_to_s3()
```

---

## Script 2: Delete S3 Bucket

This script deletes an S3 bucket and all its contents.

### Steps to Run the Script

1. Open the `delete_bucket.py` script and update the following variable:
   - `BUCKET_NAME`: Replace `"bucket-2-created"` with the name of the bucket you want to delete.
2. Run the script:
   ```bash
   python delete_bucket.py
   ```
3. If successful, the script will:
   - Empty the bucket (delete all objects and versions).
   - Delete the bucket.

### Script Code

```python
import boto3
from botocore.exceptions import NoCredentialsError, PartialCredentialsError, ClientError

# AWS S3 Configuration
BUCKET_NAME = "bucket-2-created"

def empty_s3_bucket():
    """Delete all objects and versions in an S3 bucket."""
    s3 = boto3.resource("s3")
    bucket = s3.Bucket(BUCKET_NAME)

    try:
        bucket.objects.all().delete()  # Delete all objects
        if bucket.object_versions.all():
            bucket.object_versions.all().delete()  # Delete all versioned objects
        print(f"✅ Emptied bucket '{BUCKET_NAME}'")
    except ClientError as e:
        print(f"⚠️ Error emptying bucket: {e}")

def delete_s3_bucket():
    """Delete the S3 bucket after emptying its contents."""
    s3 = boto3.client("s3")

    try:
        s3.delete_bucket(Bucket=BUCKET_NAME)
        print(f"✅ Bucket '{BUCKET_NAME}' deleted successfully.")
    except ClientError as e:
        print(f"⚠️ Error deleting bucket: {e}")

if __name__ == "__main__":
    try:
        empty_s3_bucket()  # First, delete all objects
        delete_s3_bucket()  # Then, delete the bucket
    except NoCredentialsError:
        print("⚠️ Error: AWS credentials not found.")
    except PartialCredentialsError:
        print("⚠️ Error: Incomplete AWS credentials.")
    except Exception as e:
        print(f"⚠️ Unexpected error: {e}")
```

---

## Notes

- **Bucket Naming**: S3 bucket names must be globally unique.
- **Error Handling**: The scripts include error handling for common issues like missing credentials or file not found.

---


---

# **📌 3. Running the Python Scripts in VS Code**
### ✅ **Step 5: Open Project in VS Code**
1. Open **Visual Studio Code**.
2. Click **File > Open Folder**.
3. Select the `aws-s3-python` folder.

### ✅ **Step 6: Install Dependencies**
In VS Code, open the **terminal**  and run:
```sh
pip install boto3
```

### ✅ **Step 7: Run the Scripts**
To create a bucket and upload a file, run:
```sh
python create_s3_bucket.py
```
To delete the bucket and its contents, run:
```sh
python delete_s3_bucket.py
```

---

# **📌 4. Push Code to GitHub**
### ✅ **Step 8: Add and Commit Files**
After adding scripts, push them to GitHub:
```sh
# Stage all files
git add .

# Commit changes
git commit -m "Added S3 bucket scripts and README"

# Push to GitHub
git push origin main
```

---

# **📌 5. Verify on GitHub**
1. Go to your GitHub repository.
2. You should see all your files uploaded!

---
# **Advantages of using script to create AWS services**

1.**Automation & Efficiency**
- Scripts allow you to automate the provisioning of AWS resources, saving time compared to manual setup via the AWS Console.
Repetitive tasks (e.g., creating multiple S3 buckets, launching EC2 instances) can be handled automatically.

✅ Example: A Python script using Boto3 can create 10 S3 buckets in different regions in seconds.

2.**Consistency & Repeatability**
- Ensures that AWS resources are created in a standardized and repeatable way, avoiding human errors.
You can store scripts in version control (Git) to track changes and maintain consistency.

✅ Example: A Terraform script can define an infrastructure blueprint that multiple teams can use to deploy identical environments.

3.**Scalability & Flexibility**
- Easily scale services up or down by modifying a script instead of manually adjusting resources.

Can dynamically adjust configurations (e.g., EC2 instance sizes, database scaling).

✅ Example: A Lambda function can trigger a script to scale up an Auto Scaling Group when traffic increases.

4.**Cost Optimization**
- Helps reduce costs by ensuring resources are provisioned only when needed and automatically deleted when not in use.
Scheduled scripts can terminate unused resources to prevent unnecessary charges.

✅ Example: A cron job can run a Python script that stops EC2 instances at night to save costs.

5.**Infrastructure as Code (IaC) Integration**
- Works with IaC tools like Terraform, AWS CloudFormation, and AWS CDK to define infrastructure in a declarative way.
Enables infrastructure versioning and rollback capabilities.

✅ Example: A CloudFormation template can deploy an entire AWS environment, including S3, EC2, and RDS, in minutes.

6.**Security & Access Control**
- Helps enforce security best practices by applying proper IAM roles, encryption, and permissions automatically.
Reduces the risk of misconfigurations compared to manual setup.

✅ Example: A script can enforce strict S3 bucket policies, ensuring no public access.

7.**Integration with CI/CD & DevOps**
- AWS scripting is crucial for CI/CD pipelines, allowing seamless deployments and infrastructure updates.
Works well with Jenkins, GitHub Actions, AWS CodePipeline, etc.

✅ Example: A script in a CI/CD pipeline can automatically deploy a web application to AWS whenever code is pushed.

8.**Disaster Recovery & Backups**
-Automates backups, snapshots, and failover mechanisms for disaster recovery.
Ensures rapid recovery with minimal manual intervention.

✅ Example: A script can take daily snapshots of an RDS database and store them in S3.

**Conclusion: Why Use Scripts?**

✅ Faster than manual setup

✅ More reliable & consistent

✅ Reduces costs through automation

✅ Improves security & compliance

✅ Enables DevOps, CI/CD, & scalability

---

Feel free to customize this page further to suit your needs!
