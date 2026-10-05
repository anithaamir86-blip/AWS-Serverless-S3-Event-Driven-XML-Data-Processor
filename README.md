# AWS Serverless S3 Event-Driven XML Data Processor

A serverless data extraction pipeline deployed on AWS. This project demonstrates an event-driven architecture where XML file uploads to Amazon S3 automatically trigger an AWS Lambda function running Python (`boto3`) to parse structured XML data elements and stream formatted logs into Amazon CloudWatch.

This lab bridges traditional ETL and XML data parsing concepts with modern, serverless cloud architecture.

---

## Architecture Overview
[ Local XML File ] ---> [ S3 Bucket ] ---> (S3 Event Trigger) ---> [ AWS Lambda (Python 3.12) ] ---> [ CloudWatch Logs ]
### Key Components:
* **Amazon S3 (`xml-processor-bucket`)**: Acts as the landing storage zone for inbound XML documents. Configured with S3 Event Notifications targeting Object Created events (`s3:ObjectCreated:*`).
* **AWS Lambda (`ParseXMLDocument`)**: An event-driven serverless compute function written in Python using `urllib.parse` and `xml.etree.ElementTree` to retrieve and parse S3 objects on demand.
* **AWS IAM (`LambdaXMLProcessorRole`)**: Implements least-privilege access execution permissions with `AWSLambdaBasicExecutionRole` and `AmazonS3ReadOnlyAccess`.
* **Amazon CloudWatch**: Receives execution log streams and outputs extracted XML schema elements for operational visibility.

---

## Repository Structure

```text
├── README.md               # Project documentation
├── lambda_function.py      # Python script executed by AWS Lambda
├── sample.xml              # Sample XML file used for testing
└── screenshots/            # Console verification screenshots
    ├── s3-bucket.png
    ├── lambda-trigger.png
    └── cloudwatch-logs.png
Deployment & Setup Steps
Step 1: Create the Amazon S3 Bucket
Created a general-purpose S3 bucket with default server-side encryption (SSE-S3) and public access blocked.
Step 2: Configure IAM Roles & Permissions
Created an IAM execution role for Lambda (LambdaXMLProcessorRole).
Attached managed policies: AWSLambdaBasicExecutionRole and AmazonS3ReadOnlyAccess.
Step 3: Deploy the AWS Lambda Function
Created a Python 3.12 Lambda function using the custom IAM execution role.
Deployed core extraction logic using Python's built-in xml.etree.ElementTree parsing library and boto3 SDK.
Step 4: Configure S3 Event Notification Trigger
Added an S3 trigger on the Lambda function targeting s3:ObjectCreated:* events restricted to .xml suffixes.
Verification & Output
1. S3 Upload Verification
Uploaded sample.xml to the target S3 bucket:
2. Lambda S3 Event Trigger
Configured event mapping on the Lambda function console:
3. CloudWatch Execution Logs
Inspected CloudWatch log stream verifying automated retrieval, parsing, and console extraction output:
Tech Stack & AWS Services
Cloud Provider: Amazon Web Services (AWS)
Compute: AWS Lambda (Python 3.12, boto3)
Storage: Amazon S3
Monitoring & Security: Amazon CloudWatch Logs, AWS IAM
