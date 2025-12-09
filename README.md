# AWS Serverless Student Management Application

This project demonstrates a fully serverless student management system built using **AWS Lambda**, **API Gateway**, **DynamoDB**, and **S3 Static Website Hosting**. The solution provides APIs to **insert** and **retrieve** student data from DynamoDB and a simple HTML frontend hosted on Amazon S3.

---

## 🚀 Architecture Overview

```
HTML/JavaScript (S3)
        |
    API Gateway (REST)
     /             \
Lambda(GET)     Lambda(POST)
        |             |
     DynamoDB (studentData)
```

---

## 🗃️ DynamoDB Setup

1. Go to **AWS Console → DynamoDB**
2. Click **Create Table**
3. Configure table:

   * Table Name: `studentData`
   * Partition Key: `studentId` (String)
4. Leave default settings
5. Click **Create Table**

---

## 🔐 IAM Role for Lambda

1. Go to **IAM → Roles → Create role**
2. Select:

   * Trusted entity: **AWS Service**
   * Use Case: **Lambda**
3. Attach permissions:

   * `AmazonDynamoDBFullAccess`
   * `AWSLambdaBasicExecutionRole`
4. Role name: `Lambda-DynamoDB-Role`
5. Create Role

---

## 🧠 Lambda Functions

### 1️⃣ GET Student Data

1. Go to **Lambda → Create Function**
2. Select:

   * Author from scratch
   * Name: `getStudent`
   * Runtime: **Python 3.12**
   * Architecture: `x86_64`
   * Permissions: Use existing role → `Lambda-DynamoDB-Role`
3. Create Function
4. Paste the code from `getStudent.py`
5. Deploy

**Test Event Example:**

```json
{
  "studentId": "101"
}
```

---

### 2️⃣ Insert Student Data

1. Go to **Lambda → Create Function**
2. Select:

   * Author from scratch
   * Name: `insertStudentData`
   * Runtime: **Python 3.12**
   * Architecture: `x86_64`
   * Permissions: Use existing role → `Lambda-DynamoDB-Role`
3. Create Function
4. Paste the code from `insertStudentData.py`
5. Deploy

**Test Event Example:**

```json
{
  "studentId": "101",
  "name": "John",
  "age": "20",
  "college": "XYZ College"
}
```

---

## 🌐 API Gateway Setup

1. Go to **API Gateway → Create API**
2. Choose **REST API → Build**
3. Name: `student`
4. Endpoint Type: **Edge Optimized**
5. Create API

---

### ➤ Create GET Method

1. Select **Resources**
2. Click **Actions → Create Method**
3. Choose **GET**
4. Integration Type: **Lambda**
5. Select `getStudent` function
6. Save & Test

---

### ➤ Create POST Method

1. Select **Resources → Actions → Create Method**
2. Choose **POST**
3. Integration Type: **Lambda**
4. Select `insertStudentData` function
5. Save & Test

---

### ⚙️ Enable CORS

1. Select **Resource /**
2. Click **Actions → Enable CORS**
3. Apply for GET and POST

---

### 🚢 Deploy API

1. **Actions → Deploy API**
2. Stage: `prod`
3. Copy **Invoke URL**

Example:

```
https://<api-id>.execute-api.<region>.amazonaws.com/prod
```

---

## 🖥️ Frontend Setup (S3)

1. Go to **S3 → Create Bucket**
2. Disable **Block Public Access**
3. Upload files:

   * `index.html`
   * `script.js`
4. Go to **Properties → Static Website Hosting**
5. Enable & set index: `index.html`
6. Add Bucket Policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::<bucket-name>/*"
    }
  ]
}
```

---

## 📌 Usage

1. Visit the S3 Static Website URL
2. Enter student details
3. Click **Submit** → Data stored in DynamoDB
4. Enter Student ID and click **Get Student** → Fetches from DB

---

---

## 🛠️ Future Enhancements

* API Key + Usage Plans
* AWS Cognito Authentication
* Lambda Layers
* CloudWatch Dashboard
