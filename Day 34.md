# Day 34: Create a Lambda Function Using CLI

## Overview

In this lab, I created and deployed a serverless Python AWS Lambda function named **`datacenter-lambda-cli`** using the AWS Command Line Interface (CLI). After writing the handler logic and packaging it into a deployment archive (`function.zip`), I provisioned the function using the existing `lambda_execution_role`, verified its configuration, and invoked it to confirm a `200` status code with the response body `"Welcome to KKE AWS Labs!"`.

---

## Lab Requirements

* **Lambda Function Name:** `datacenter-lambda-cli`
* **Runtime:** Python (`python3.9`)
* **Handler:** `lambda_function.lambda_handler`
* **Deployment Package:** `function.zip` (containing `lambda_function.py`)
* **IAM Execution Role:** `lambda_execution_role`
* **Response Payload:** Status code `200` and body `"Welcome to KKE AWS Labs!"`

---

## Step-by-Step Implementation

### Step 1: Create the Python Script (`lambda_function.py`)

* Created `lambda_function.py` defining the `lambda_handler` function to return status code `200` and the greeting body:
```python
def lambda_handler(event, context):
    return {
        "statusCode": 200,
        "body": "Welcome to KKE AWS Labs!"
    }

```



### Step 2: Zip the Script

* Packaged `lambda_function.py` into a zip archive named `function.zip`:
```bash
zip function.zip lambda_function.py

```



### Step 3: Retrieve AWS Account ID

* Queried the caller identity via AWS STS to obtain the active AWS Account ID for the role ARN:
```bash
aws sts get-caller-identity

```



### Step 4: Create the Lambda Function Using AWS CLI

* Provisioned `datacenter-lambda-cli` with the `python3.9` runtime, `lambda_execution_role`, and `function.zip` deployment package:
```bash
aws lambda create-function \
  --function-name datacenter-lambda-cli \
  --runtime python3.9 \
  --role arn:aws:iam::<ACCOUNT_ID>:role/lambda_execution_role \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip

```



### Step 5: Verify and Invoke the Lambda Function

* Verified the function metadata and configuration details:
```bash
aws lambda get-function \
  --function-name datacenter-lambda-cli

```


* Invoked the function synchronously and wrote the response payload to `response.json`:
```bash
aws lambda invoke \
  --function-name datacenter-lambda-cli \
  response.json

```



---

## Verification & Results

* Confirmed via `aws lambda get-function` that `datacenter-lambda-cli` was created with the expected runtime, handler, and execution role.
* Inspected `response.json` after invoking the function and verified it returned:
```json
{
  "statusCode": 200,
  "body": "Welcome to KKE AWS Labs!"
}

```



---

## Key Takeaways

* **Repeatable CLI Provisioning:** Packaging code into a `.zip` bundle and running `aws lambda create-function` makes serverless deployments scriptable, repeatable, and easy to automate.
* **Handler & Role Alignment:** Ensuring `--handler` matches `<filename>.<function_name>` (`lambda_function.lambda_handler`) and `--role` points to a valid IAM role ARN prevents runtime execution errors.
