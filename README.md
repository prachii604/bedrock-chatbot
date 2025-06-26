## 🔎 Introduction

I created a fully serverless AI chatbot using Amazon Bedrock’s Titan Text G1 – Express model. The setup includes connecting the model to AWS Lambda for backend processing, exposing it via Amazon API Gateway with proper CORS configuration, and deploying a sleek HTML/JavaScript frontend through Amazon S3 static website hosting.

### Here's how I built it step by step:

1. Configured Amazon Bedrock and enabled access to the Titan model.
2. Created a Lambda function to handle user input and generate responses from the model.
3. Set up API Gateway to expose the Lambda function as a REST endpoint with CORS enabled.
4. Developed a responsive frontend using HTML, CSS, and JavaScript.
5. Hosted the frontend on Amazon S3 as a static website for public access.

---

## 🔎 Overview

* 🔍 Users interact with the chatbot either through a static frontend hosted on Amazon S3 or Amplify Hosting or by calling the API directly.
* 🔍 Their messages are sent as HTTPS requests to Amazon API Gateway, which securely forwards them to an AWS Lambda function.
* 🔍 The Lambda function processes the input and uses an IAM Role to securely call Amazon Bedrock, invoking the Titan model to generate a chatbot response.
* 🔍 That response flows back through Lambda and API Gateway to the user.

### 🛠️ Tech Stack:

* Amazon Bedrock (Titan Text G1 - Express)
* AWS Lambda
* Amazon API Gateway
* Amazon S3 (Static Hosting)
* JavaScript + HTML + CSS

---

## ➞ Step 1 - Set Up Amazon Bedrock Access

* Make sure your AWS account has Bedrock access (Bedrock is GA now but some regions might differ).
* Go to the AWS Console → Amazon Bedrock
* Request access to `amazon.titan-text-express-v1` or choose any model based on your needs.
* Once approved, you're ready to build.

## ➞ Step 2 - Create an IAM Role for Lambda

* Go to IAM → Roles → Create role
* Trusted entity type: **AWS service**
* Use case: **Lambda**
* Permissions: `AmazonBedrockFullAccess`, `CloudWatchLogsFullAccess`
* Role name: `chatBotLambdaRole`
* Review and create the role

## ➞ Step 3 - Create the Lambda Function

* Go to AWS Lambda → Create function
* Author from scratch
* Function name: `chatbotlambda`
* Runtime: Python 3.12
* Architecture: x86\_64
* Create the function
* Paste your Lambda code into `lambda_function.py`

## ➞ Step 4 - Set Up API Gateway

* Go to API Gateway → REST API → Build
* Create a new API named `chatbot-api`
* Create a Resource: `/chat`
* Enable **CORS**
* Create Method: `POST`
* Integration type: Lambda function
* Enable **Lambda proxy integration**
* Region: `us-east-1`, select `chatbotlambda`
* Deploy API to a new stage named `dev`

### Test your API using Postman or curl.

> ⚠️ Once deployed, you'll get an **invoke URL** to use as your API endpoint.

## ➞ Step 5 - Build the Frontend Chat UI

* Open VS Code or your preferred code editor
* Create `index.html`
* Paste your HTML/CSS/JavaScript code
* Replace the placeholder URL with your actual API Gateway endpoint:

  ```
  https://your-api-id.execute-api.us-east-1.amazonaws.com/chat
  ```

## ➞ Step 6 - Deploy Frontend to S3 Static Website

* Go to Amazon S3 → Create Bucket
* Choose General Purpose bucket
* Bucket name: `chatbotprachita`
* Disable "Block all public access"
* Upload `index.html`
* Enable static website hosting in **Properties**

  * Set index document as `index.html`
* Set the Bucket Policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::chatbotprachita/*"
    }
  ]
}
```

* Visit the **Object URL** of `index.html` to access your chatbot live.

---

## 🏆 Result

You now have a fully functioning, real-time AI chatbot with a stylish frontend hosted on S3, powered by Amazon Bedrock and seamlessly integrated using AWS Lambda and API Gateway!
