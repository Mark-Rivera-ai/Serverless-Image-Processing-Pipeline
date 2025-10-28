# Serverless Image Processing Pipeline (In Progress)
Serverless image processing pipeline leveraging AWS Lambda, S3, and API Gateway to automate ingestion, resizing, and metadata tagging of uploaded images.

**Status:** Core AWS infrastructure and function architecture designed; deployment automation in progress.

*Purpose 

- This project was created to apply and reinforce the foundational AWS concepts learned while preparing for and passing the AWS Cloud Practitioner certification.

-----------------------

* Below is the Architecture for this project:

User Upload (HTTP POST)
        │
        ▼
Amazon API Gateway
        │
        ▼
AWS Lambda (Ingest Function)
  - Validates upload
  - Saves to S3 (Raw Images)
        │
        ▼
S3 Event Trigger (ObjectCreated)
        │
        ▼
AWS Lambda (Processor Function)
  - Resizes image
  - Extracts metadata
  - Writes results to S3 (Processed) and DynamoDB
        │
        ▼
CloudWatch Logs and Metrics

------------------------
* Core Components

API Gateway
Provides a simple REST endpoint for uploads. Could later be replaced with a presigned URL workflow.

AWS Lambda (Ingest Function)
Receives uploads, validates file type, and stores images in the raw S3 bucket.

AWS Lambda (Processor Function)
Triggered by S3 events. Uses the Pillow (Python) or Sharp (Node.js) library to create resized copies and gather metadata. Stores processed results and metadata.

Amazon S3
Holds both the original and processed images. The raw bucket acts as input, and a second bucket or folder holds processed images.

DynamoDB
Stores metadata such as image name, dimensions, and timestamps for easy retrieval.

CloudWatch
Collects logs and metrics from both Lambda functions.

------------------------

* Processing Flow

1. A user uploads an image via API Gateway.

2. The Ingest Lambda stores it in the Raw S3 bucket.

3. S3 triggers the Processor Lambda.

4. The Processor resizes the image and writes metadata.

5. Results are saved to the Processed S3 location and DynamoDB.

6. CloudWatch tracks execution and errors.

* Planned Enhancements

- Use SQS for queued, large-batch image processing.

- Add AWS Rekognition integration for automatic tagging.

- Deploy using a CI/CD pipeline with AWS SAM or the AWS CDK.

- Add an optional dashboard (React + Amplify) for browsing results.

---------------------------------

* Security

- All AWS resources configured with least-privilege IAM roles.

- S3 buckets encrypted at rest and configured for private access only.

- Option for presigned URLs to control upload access.

- CloudWatch alerts for failures or throttling.

----------------------------------

* Estimated Timeframe

- Day 1 (4–6 hrs)	Create two S3 buckets, write Ingest Lambda, connect via API Gateway	1 day

- Day 2 (4–6 hrs)	Add Processor Lambda for resizing + metadata extraction, enable S3 trigger, and test end-to-end flow	1 day

- Optional	Add DynamoDB + CloudWatch dashboards	+½ day

-----------------------------------

* Expected Outcome

Once complete, the pipeline will automatically process uploaded images, create resized copies, generate metadata, and store both the images and metadata without manual intervention. It will be ready for extension into more advanced workflows (Rekognition tagging, queue processing, or a simple web viewer).
