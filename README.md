# 🛡️ Real-Time Data Leak Prevention and Remediation with AWS Lambda, Macie, and EventBridge

## 🔍 Introduction  
Sensitive data leaks are one of the most dangerous and expensive risks for any business. This project is a **serverless automation system** that detects sensitive data exposure in real-time and automatically remediates it using AWS Lambda.

Built as part of the **AWS Lambda Hackathon**, this project uses Amazon Macie to detect PII (Personally Identifiable Information) in an S3 bucket, then triggers a Lambda function via EventBridge to isolate and secure the data, followed by an alert to security engineers via SNS and CloudWatch to log event and for compliance purposese.

---

## 🧠 Problem Solved  
Organizations often rely on manual monitoring or delayed response mechanisms when handling data leaks. This project:
- Detects PII exposure instantly.
- Automates remediation.
- Eliminates human error in initial response.
- Sends real-time alerts to incident response teams.

---

## 🧰 AWS Services Used

| Service              | Purpose                                                      |
|----------------------|--------------------------------------------------------------|
| **AWS Lambda**       | Core logic to remediate detected data leak.                  |
| **Amazon Macie**     | Detects sensitive data (e.g., PII) in S3 buckets.            |
| **Amazon EventBridge** | Triggers Lambda based on Macie alerts.                    |
| **Amazon SNS**       | Sends notification to incident response team.                |
| **Amazon S3**        | Stores both vulnerable and secure files.                     |
| **CloudWatch Logs** | For debugging and monitoring Lambda executions and compliance audit. |

---

## 🏗️ Architecture Overview  
![Architecture Diagram](https://github.com/AibinuolaDamilola/aws-dlp-lambda-automation/blob/main/diagrams/arc.png)

---

**Workflow:**
1. A file is uploaded to a monitored (vulnerable) S3 bucket.
2. Macie scans the file and detects sensitive content.
3. EventBridge captures the Macie finding and triggers a Lambda function.
4. The Lambda function:
   - Moves the file to a secure S3 bucket.
   - Deletes the original file from the vulnerable bucket.
   - Sends an SNS alert to the security team.
   - Log the event

---

## ⚙️ How It Works

1. **Macie Detection**: Scans new files in a bucket and generates alerts when it detects sensitive information like names, addresses, emails, medical records, credit cards etc.  
2. **EventBridge Rule**: Listens for Macie findings and forwards them to a Lambda trigger.  
3. **Lambda Function** (`index.py`):
   - Parses the Macie finding.
   - Retrieves the affected object from S3.
   - Moves the file to a pre-defined secure bucket.
   - Deletes the original object.
   - Publishes a security alert to an SNS topic.  
4. **SNS Notification**: Sends a clear, formatted alert to the incident response team.
5. **CloudWatch Log**: Log the events for complaince and monitoring 

---

## 📄 File Descriptions

| File/Folder              | Description                                         |
|--------------------------|-----------------------------------------------------|
| `lambda/index.py`        | Main Lambda code handling detection & response      |
| `diagrams/architecture.png` | System architecture diagram                    |
| `eventbridge-config.json` | Sample EventBridge rule for Macie alerts         |
| `sns-template.txt`       | Example SNS message body sent during alert         |
| `README.md`              | This file – full documentation and overview         |
| `requirements.txt`       | Dependencies for Lambda function (usually `boto3`)  |

---

##  How to Reproduce

### Prerequisites
- AWS Account with access to:
  - S3
  - Macie
  - Lambda
  - SNS
  - EventBridge
- Basic knowledge of IAM Roles and Permissions

### Steps

1. **Create Two S3 Buckets**:
   - One for incoming/vulnerable files
   - One secure (encrypted) bucket for quarantined files

2. **Enable Amazon Macie**  
   Set Macie to scan your "vulnerable" bucket.

3. **Set up SNS Topic**  
   Create a topic and subscribe your email to receive alerts.

4. **Deploy Lambda Function**  
   Upload `index.py` and attach permissions for:
   - S3 read/write
   - SNS publish
   - Logging to CloudWatch

5. **Create EventBridge Rule**  
   Use `eventbridge-config.json` or manually configure a rule for Macie findings.

6. **Upload PII File to Bucket**  
   Upload a test file with mock PII to trigger the flow.

---

## 🎥 Demo Video  
 [Watch on YouTube](https://youtu.be/hZDTvldsadY?si=QaNLJ5WsC3YQURDw)

- Demonstrates real-time detection of PII  
- Shows automated remediation in S3  
- Displays SNS alert sent to email  

---

## 📌 Important Notes
- You must enable Macie in your selected region.  
- Macie incurs cost after free tier; monitor usage.  
- Make sure Lambda has proper IAM role with:
  - `s3:*`, `sns:Publish`, `logs:*`

---

## 🛠 Project Walkthrough
I’ve written a detailed walkthrough of this project on Medium — covering how to connect Amazon S3, Macie, EventBridge, Lambda, and SNS to automate sensitive data detection and alerting.
📖 Read it here: <a href="https://medium.com/@evolvewithdami/how-i-built-an-automated-sensitive-data-alerting-system-on-aws-s3-macie-lambda-sns-1f5027594eab">How I Built an Automated Sensitive Data Alerting System with AWS</a>

---

## Business Impact & Results
This setup gave the startup:
- Prevents real-time exposure of sensitive data
- Keeps audit logs for compliance
- And enforces encryption and deletion instantly
- This reduced manual audits, cut response time to under 2 minutes, and added a layer of HIPAA readiness.

---

## 🤝 Contributing  
PRs and ideas are welcome. Fork the repo, submit your changes, and help expand the project

---

## 📜 License  
MIT License. Free to use and adapt with credit.
