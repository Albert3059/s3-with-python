
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

Feel free to customize this page further to suit your needs!
