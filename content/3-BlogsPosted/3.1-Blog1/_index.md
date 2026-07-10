---
title: "Blog 1"
date: 2026-06-16
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# Automating Medical Record Digitization with Amazon Bedrock Data Automation and AWS HealthLake

As part of AWS Vietnam Community Day 2026, I had the opportunity to learn about a highly practical and timely technology solution: applying Generative AI to automate the medical document digitization process at an enterprise scale.

Currently, a major challenge at many hospitals and healthcare facilities is storing and processing a massive volume of medical records in traditional paper formats, scans, or unstructured PDF files. The traditional manual data entry process is not only time-consuming and labor-intensive but also prone to high error rates, making it difficult to search, synchronize, and analyze clinical data for treatment purposes.

The solution presented in this architectural blueprint is a tight integration between the extraction power of Amazon Bedrock Data Automation (BDA) and the standardized data management capabilities of AWS HealthLake, creating a comprehensive end-to-end automated processing pipeline.


## Architecture Guidance

The solution architecture diagram describes an automated, serverless, and scalable pipeline that reduces manual workload and minimizes errors for healthcare organizations. The detailed operational workflow consists of sequential steps illustrated through the system diagram below.
**The solution architecture is now as follows:**

![Figure 1](/images/3-BlogsTranslated/3.1-Blog1/ARCHBLOG-1364-1.png)

> *Hình 1. Kiến trúc đầu-cuối thể hiện pipeline hướng sự kiện, từ lúc upload PDF đến khi lưu trữ dữ liệu tuân thủ FHIR.*


## Technology Choices and Communication Scope

| Communication scope                       | Technologies / patterns to consider                                                        |
| ----------------------------------------- | ------------------------------------------------------------------------------------------ |
| Triggering automated data flow within the pipeline              | Amazon S3 Event Notification, S3 Event Trigger                               |
| Logic processing and intermediate data routing | AWS Lambda (BDA Job Trigger, FHIR Processor) |
| Intelligent data extraction and standardization                          | Amazon Bedrock Data Automation, AWS HealthLake                                      |
| Monitoring, logging, and centralized system management | Amazon CloudWatch for logs (Centralized logging) |


## The Pub/Sub Hub

Dividing processing boundaries between components ensures decoupled system operations and increases resilience:
- Each service (S3, Lambda, Bedrock, HealthLake) focuses solely on resolving its specific, dedicated task.
- Connections between components are executed asynchronously based on events triggered from the Amazon S3 storage layer.  
- Completely eliminating direct synchronous connections prevents system bottlenecks when there is a sudden surge in uploaded records.*


## Core Microservice

Provides the foundational data processing layer and intelligent extraction tier, including: 
- **Amazon S3** (input PDF) acting as the landing zone for raw incoming data
- **Amazon Bedrock Data Automation combined with BDA Blueprints** (JSON templates) to extract clinical information. 
- **Amazon S3** (Extracted Medical Data) storing intermediate post-analysis data.

> Only direct write access for raw data into the input S3 bucket is permitted; the entire subsequent processing and standardization workflow occurs indirectly via AWS Lambda → ensuring data integrity.

## Front Door Microservice

- Provides the dedicated AWS HealthLake service to store medical data according to the international FHIR R4 standard structure.  
- Synchronizes raw data through the automated transformation process of the task-generating Lambda function (FHIR Processor and Job Creator). 
- Completely eliminates isolated data stores (Data Silos), allowing client applications to easily query via FHIR APIs.  

## New Features in the Solution

### 1. Intelligent Document Analysis with Generative AI
Instead of manual data entry, the system uses Amazon Bedrock Data Automation combined with structural Blueprints to completely automate the conversion of raw document content into highly accurate structured data.

### 2. Deep Healthcare Data Standardization
Synchronizing data into the AWS HealthLake repository based on the FHIR R4 standard breaks down the data silo barriers frequently encountered in the healthcare industry, helping medical organizations explore, search, and analyze data more effectively.

This is a clear demonstration that Generative AI is not just for content generation, but can actively support real-world data processing in critical sectors like healthcare.

### Original Blog Post Link:
https://aws.amazon.com/blogs/machine-learning/automate-medical-record-digitization-with-amazon-bedrock-data-automation-and-aws-healthlake/