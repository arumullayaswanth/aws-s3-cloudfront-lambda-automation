
# End-to-End Static Website Hosting on AWS S3 with CloudFront CDN and Automated Cache Invalidation via Lambda



## ✅ Overview

This project demonstrates the complete process of hosting a static website on Amazon S3 and distributing it via CloudFront, including an automated cache invalidation mechanism using AWS Lambda.

---

## 📂 1. Static Website Hosting on Amazon S3

### Step 1: Create S3 Bucket
- Go to **Amazon S3 > Buckets > Create bucket**
- General configuration:
  - **Region:** US East (N. Virginia) `us-east-1`
  - **Bucket name:** `aluru.site`
  - **Object Ownership:** ACLs enabled
- Permissions:
  - Uncheck **Block all public access**
- Enable **Bucket Versioning**
- Click **Create bucket**

### Step 2: Upload Static Website Files
- Open the bucket `aluru.site`
- Upload your project files (e.g., `index.html`, `style.css`, etc.)

### Step 3: Enable Static Website Hosting
- Go to **Properties > Static website hosting > Edit**
- Enable static website hosting:
  - Hosting type: `Host a static website`
  - Index document: `index.html`
- Save changes

### Step 4: Make Files Public
- Select all uploaded files
- Actions > **Make public using ACL**

---

## 🌐 2. Configure Amazon CloudFront

### Step 1: Create a Distribution
- Go to **CloudFront > Create Distribution**
- Origin settings:
  - **Origin domain:** `aluru.site.s3.us-east-1.amazonaws.com`
  - Click **Use website endpoint**
  - **Name:** `aluru.site.s3-website-us-east-1.amazonaws.com`
  - Cache key and origin requests

### Step 2: Create Custom Cache Policy
- Cache key and origin requests:
  - Click **Create cache policy**
  - Name: `prod`
  - TTL Settings:
    - Default TTL: `86400`
    - Max TTL: `86400`
    - Min TTL: `86400`
- Save policy and assign it

### Step 3: Complete Distribution Settings
- Web Application Firewall (WAF): `select Do not enable security protections`
- Click **Create distribution**

### Step 4: Access via CloudFront
- After the distribution is deployed, copy the **Distribution domain name** (e.g., `https://d16rlawyioyp8z.cloudfront.net`)
- Paste in browser to access the site
- paste in google now you can access the application

---

## ⚠️ 3. Manual vs Automated Cache Invalidation

**note:** developers are push update code in s3 so immediately this should be reflect in to cash a otherwise still old code they will access so immediately there should be update so you need to create invalidation so invalidation manually we have to do whenever code will push by the developers we have to do invalidation (this a manually process)


### Manual Invalidation
- Navigate to:
  - **CloudFront > Distributions > Your Distribution > Invalidations**
- Create invalidation:
  - **Object path:** `/*`
- Click **Create invalidation**

**note:**can we automate this process instead of doing manually every time (for example developers push the code every two days once we have to do  manually every time right otherwise old code only )

> 📝 This must be done every time developers push updates to S3 to avoid serving outdated content from cache.

---

## 🤖 4. we can Automate Cache Invalidation with AWS Lambda

### Step 1: Create IAM Role for Lambda
- Go to **IAM > Roles > Create role**
- Trusted entity: `AWS Service`
- Use case: `Lambda`
- Attach permissions:
  - `AWSLambdaBasicExecutionRole`
  - `CloudFrontFullAccess` *(or custom policy for cache invalidation)*
- Name the role: `lambda-admin`
- Create role

### Step 2: Create Lambda Function
- Go to **Lambda > Create function**
- Choose:
  - **Author from scratch**
  - Function name: `test`
  - Runtime: `Python 3.x`
- Execution Role:
  - Choose **Use existing role**
  - Select: `lambda-admin`

### Step 3: Add Lambda Code

Paste the following code:

```python
import boto3
import json
from datetime import datetime

# Hardcoded CloudFront Distribution ID
CLOUDFRONT_DISTRIBUTION_ID = "E2FSSVNKWHFUV2"  #Replace give your cloudfront id

# Initialize CloudFront client
cloudfront_client = boto3.client("cloudfront")

def lambda_handler(event, context):
    try:
        # Create an invalidation request
        response = cloudfront_client.create_invalidation(
            DistributionId=CLOUDFRONT_DISTRIBUTION_ID,
            InvalidationBatch={
                "Paths": {
                    "Quantity": 1,
                    "Items": ["/*"]  # Invalidate all files
                },
                "CallerReference": str(datetime.utcnow().timestamp())  # Unique reference
            }
        )

        # Convert response to JSON-serializable format
        invalidation_id = response["Invalidation"]["Id"]
        create_time = response["Invalidation"]["CreateTime"].isoformat()  # Convert datetime to string

        return {
            "statusCode": 200,
            "body": json.dumps({
                "message": "Invalidation request sent successfully",
                "InvalidationId": invalidation_id,
                "CreateTime": create_time
            })
        }

    except Exception as e:
        print(f"Error creating invalidation: {str(e)}")
        return {
            "statusCode": 500,
            "body": json.dumps({
                "error": "Failed to create invalidation",
                "message": str(e)
            })
        }

```

> 🔁 Replace `CLOUDFRONT_DISTRIBUTION_ID ` with your actual CloudFront distribution ID.

# deploy the code

### Step 4: Add S3 Trigger to Lambda
- Go to Lambda > Your Function > **Add Trigger**
- Configuration:
  - **Trigger type:** S3
  - Bucket: `aluru.site`
  - Suffix: `index.html`
- Acknowledge warning and click **Add**

**Note:**  when the new changes will come the code lambda will tack care until on that your cache we take.
**Note:** whenever new changes will come lambda will take care if no change will be there automatically old change will be ser form the cache

---

## ✅ Outcome

- Every time new content is pushed to the S3 bucket, the Lambda function will automatically invalidate the CloudFront cache.
- Users always get the latest version of the site without manual intervention.

---

## 🧾 Project Summary for Resume

**AWS Cloud Project – Static Website Hosting with Automation**

- ✅ Designed and deployed a static website using Amazon S3 with public access configuration and versioning for reliable content management.
- 🚀 Integrated AWS CloudFront as a CDN to improve global content delivery performance, configuring custom cache policies for optimized TTL settings.
- 🔁 Automated CloudFront cache invalidation using AWS Lambda and IAM roles to ensure real-time content updates upon every code deployment to S3.
- 🔐 Implemented secure and scalable AWS architecture by managing permissions via IAM, reducing manual intervention and improving deployment efficiency.

---

