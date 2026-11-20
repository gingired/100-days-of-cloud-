# Day 33: Create a Lambda Function

## Overview

In this lab, I created a dedicated IAM execution role (`lambda_execution_role`) for AWS Lambda and deployed a serverless Python function named **`xfusion-lambda`** configured to return HTTP status code `200` along with the response body `"Welcome to KKE AWS Labs!"`.

---

## Lab Requirements

* **Lambda Function Name:** `xfusion-lambda`
* **Runtime:** Python
* **IAM Execution Role:** `lambda_execution_role`
* **Architecture:** `x86_64`
* **Response Payload:** Status code `200` and body `Welcome to KKE AWS Labs!`

---

## Step-by-Step Implementation

### Step 1: Create the IAM Execution Role (`lambda_execution_role`)

* Navigated to the **IAM Console** and selected **Roles** from the left navigation pane.
* Clicked **Create role**, chose **AWS service** as the trusted entity type, and selected **Lambda** under use case before clicking **Next**.
* Selected the desired permissions policy on **Step 2** and clicked **Next**.
* Entered **`lambda_execution_role`** as the role name on **Step 3** and clicked **Create role**.

### Step 2: Create and Configure the Lambda Function (`xfusion-lambda`)

* Opened the **Functions** page in the **AWS Lambda Console** and clicked **Create function**.
* Selected **Author from scratch**, entered **`xfusion-lambda`** as the function name, and chose the **Python** runtime.
* Kept architecture set to **`x86_64`** and clicked **Create function**.
* Under **Permissions**, expanded **Change default execution role**, chose **Use an existing role**, and selected **`lambda_execution_role`**.

### Step 3: Write Code, Deploy, and Test

* Updated `lambda_function.py` in the **Code source** editor with the following handler:
```python
import json

def lambda_handler(event, context):
    # TODO implement
    return {
        'statusCode': 200,
        'body': json.dumps('Welcome to KKE AWS Labs!')
    }

```


* Clicked **Deploy** in the **Code source** section to publish the updated function code.
* Created a test event under the **Test** tab and clicked **Invoke** to run the function.

---

## Verification & Results

* Invoked `xfusion-lambda` using the console test event and inspected the execution output.
* Confirmed that the function returned `statusCode` `200` and `body` `"Welcome to KKE AWS Labs!"` without errors.

---

## Key Takeaways

* **Least-Privilege Execution Roles:** Assigning a dedicated IAM role (`lambda_execution_role`) scopes the function's AWS permissions cleanly without embedding static credentials in code.
* **Deploy Before Testing:** Changes made in the inline Lambda code editor must be deployed first so that subsequent test invocations execute the latest logic.
