# Lab 177 — [JAWS] Challenge: AWS Lambda Exercise

## 1. Lab Overview

This challenge demonstrates how to build an event-driven, serverless application using AWS Lambda, Amazon S3, and Amazon Simple Notification Service (SNS).

The application automatically counts the number of words in a text file whenever it is uploaded to an S3 bucket. The result is published to an SNS topic and delivered to a subscribed email address.

### Objectives

- Create an AWS Lambda function using Python.
- Configure an S3 bucket to trigger Lambda when a text file is uploaded.
- Retrieve and process files stored in Amazon S3.
- Count the number of words in a text file.
- Create an SNS topic and configure an email subscription.
- Send the word count through an email notification.
- Test the complete event-driven workflow.

### AWS Services Used

| Service | Purpose |
|---|---|
| AWS Lambda | Executes the Python word-count function |
| Amazon S3 | Stores uploaded text files and generates events |
| Amazon SNS | Publishes word-count notifications |
| AWS IAM | Provides permissions through the lab-provided execution role |
| Amazon CloudWatch | Stores Lambda execution logs |
| Python and Boto3 | Implement the processing logic and AWS service integration |

**AWS Region:** US West (Oregon) — `us-west-2`

---

## 2. Application Architecture

The application follows an event-driven architecture in which uploading a file automatically starts the processing workflow.

```text
                User
                  |
                  | Upload .txt file
                  v
           Amazon S3 Bucket
                  |
                  | ObjectCreated Event
                  v
           AWS Lambda Function
               (Python)
                  |
                  | 1. Read the S3 object
                  | 2. Count the words
                  | 3. Format the result
                  v
             Amazon SNS Topic
                  |
                  | Email Notification
                  v
           Confirmed Email Subscriber
```

### Workflow

1. A user uploads a text file to the configured S3 bucket.
2. Amazon S3 generates an object-created event.
3. The event invokes the Lambda function.
4. Lambda extracts the bucket name and object key.
5. The function retrieves and reads the uploaded file.
6. Python counts the whitespace-separated words.
7. Lambda publishes the result to an SNS topic.
8. SNS delivers the notification to the confirmed email subscriber.

---

## 3. Step-by-Step Implementation

### Step 1: Launch the AWS Academy Lab

1. Open the AWS Academy / Vocareum environment.
2. Start `177-[JAWS]-Lab — [Challenge] AWS Lambda Exercise`.
3. Wait until the lab status displays **Ready**.
4. Open the AWS Management Console through the lab environment.
5. Select the **US West (Oregon)** Region (`us-west-2`).

All resources must be created in the same Region so that the S3 event, Lambda function, and SNS notification workflow can be configured consistently.

### Step 2: Create an Amazon SNS Topic

Amazon SNS is used to distribute the word-count result through email.

1. Open the AWS Management Console.
2. Search for **Simple Notification Service (SNS)**.
3. Navigate to **Topics**.
4. Click **Create topic**.
5. Select the **Standard** topic type.
6. Enter the topic name:

   `WordCountTopic`

7. Keep the appropriate default settings.
8. Click **Create topic**.
9. Open the newly created topic.
10. Copy its **Topic ARN** for later configuration.

The Topic ARN uniquely identifies the SNS topic to which Lambda will publish its results.

### Step 3: Create an SNS Email Subscription

1. Open the `WordCountTopic` topic.
2. Click **Create subscription**.
3. Select the following settings:

   | Field | Value |
   |---|---|
   | Topic ARN | ARN of `WordCountTopic` |
   | Protocol | Email |
   | Endpoint | Your email address |

4. Click **Create subscription**.
5. Open the email inbox associated with the subscription.
6. Find the SNS confirmation message.
7. Click the confirmation link.

The subscription must be confirmed before SNS can deliver email notifications to that address.

### Step 4: Create an Amazon S3 Bucket

Amazon S3 stores the input files that Lambda will process.

1. Search for **Amazon S3** in the AWS Console.
2. Open **Buckets**.
3. Click **Create bucket**.
4. Enter a globally unique bucket name, such as:

   `wordcount-lab-unique-suffix`

5. Select the Region:

   `US West (Oregon) — us-west-2`

6. Keep the default security settings, including Block Public Access.
7. Click **Create bucket**.
8. Open the bucket and verify that it exists.

Use your actual bucket name in the subsequent configuration steps.

### Step 5: Create the AWS Lambda Function

Lambda executes the Python code whenever a supported file is uploaded.

1. Search for **AWS Lambda** in the AWS Console.
2. Navigate to **Functions**.
3. Click **Create function**.
4. Select **Author from scratch**.
5. Configure the function:

   | Setting | Value |
   |---|---|
   | Function name | `WordCountFunction` |
   | Runtime | Python 3.12 or another available Python runtime |
   | Architecture | `x86_64` |
   | Permissions | Use an existing role |
   | Existing role | `LambdaAccessRole` |

6. Select the existing `LambdaAccessRole` provided by the lab.
7. Click **Create function**.

**Important:** The lab explicitly requires the existing role because creating a new IAM role is not permitted. The role supplies the permissions required by the function, including access to S3, SNS, and CloudWatch Logs as specified by the lab.

### Step 6: Implement the Python Word-Count Function

1. Open `WordCountFunction`.
2. Select the **Code** tab.
3. Open `lambda_function.py`.
4. Replace the starter code with the following implementation.

```python
import boto3
import os
import urllib.parse

s3 = boto3.client("s3")
sns = boto3.client("sns")


def lambda_handler(event, context):
    topic_arn = os.environ["SNS_TOPIC_ARN"]

    for record in event["Records"]:

        # Extract the S3 bucket and uploaded object key
        bucket = record["s3"]["bucket"]["name"]
        key = urllib.parse.unquote_plus(
            record["s3"]["object"]["key"]
        )

        # Retrieve the uploaded text file
        response = s3.get_object(
            Bucket=bucket,
            Key=key
        )

        # Read the file and count whitespace-separated words
        content = response["Body"].read().decode("utf-8")
        word_count = len(content.split())

        # Extract the original filename
        filename = key.rsplit("/", 1)[-1]

        # Format the notification message
        message = (
            f"The word count in the {filename} file "
            f"is {word_count}."
        )

        # Publish the result through Amazon SNS
        sns.publish(
            TopicArn=topic_arn,
            Subject="Word Count Result",
            Message=message
        )

    return {
        "statusCode": 200,
        "body": "Word count sent"
    }
```

5. Click **Deploy** to save the updated function.

#### Explanation of the Code

**A. Initialize AWS clients**

```python
s3 = boto3.client("s3")
sns = boto3.client("sns")
```

Boto3 creates clients for interacting with Amazon S3 and Amazon SNS.

**B. Read the event information**

```python
bucket = record["s3"]["bucket"]["name"]
key = urllib.parse.unquote_plus(
    record["s3"]["object"]["key"]
)
```

The S3 event contains the bucket name and object key. URL decoding handles encoded characters in filenames.

**C. Retrieve the file**

```python
response = s3.get_object(
    Bucket=bucket,
    Key=key
)
```

The S3 `GetObject` API retrieves the uploaded file.

**D. Count the words**

```python
content = response["Body"].read().decode("utf-8")
word_count = len(content.split())
```

The file contents are decoded into a Python string. The `split()` method separates the text into whitespace-delimited tokens, and `len()` counts them.

**E. Publish the result**

```python
sns.publish(
    TopicArn=topic_arn,
    Subject="Word Count Result",
    Message=message
)
```

SNS publishes the formatted result using the required email subject.

### Step 7: Configure the Lambda Environment Variable

The function needs the SNS topic ARN to know where to send its notifications.

1. Open `WordCountFunction`.
2. Navigate to **Configuration → Environment variables**.
3. Click **Edit**.
4. Choose **Add environment variable**.
5. Enter:

   | Field | Value |
   |---|---|
   | Key | `SNS_TOPIC_ARN` |
   | Value | The ARN copied from `WordCountTopic` |

6. Click **Save**.

The function reads this value using:

```python
topic_arn = os.environ["SNS_TOPIC_ARN"]
```

This approach separates configuration from application code.

### Step 8: Configure the S3 Trigger

The S3 trigger automatically invokes Lambda when a file is uploaded.

1. Open `WordCountFunction`.
2. In **Function overview**, click **Add trigger**.
3. Select **S3** as the trigger source.
4. Select the bucket created in Step 4.
5. Choose **All object create events** as the event type.
6. Optionally configure the suffix filter as `.txt`.
7. Acknowledge the recursive invocation warning if it appears.
8. Click **Add**.

The trigger connects S3 object-created events to the Lambda function.

**Important:** Do not write output files into the same triggering bucket. Doing so can create recursive Lambda invocations.

### Step 9: Create a Sample Text File

Create a file named `sample.txt` with the following content:

```text
AWS Lambda counts words.
S3 triggers the function.
SNS sends an email.
```

The file contains 11 whitespace-separated words.

### Step 10: Upload the File to S3

1. Open **Amazon S3**.
2. Select your configured bucket.
3. Click **Upload**.
4. Choose **Add files**.
5. Select `sample.txt`.
6. Click **Upload**.
7. Verify that the file appears in the bucket.

Uploading the file generates the S3 event that invokes Lambda.

### Step 11: Verify the Email Notification

Open the email inbox used for the SNS subscription.

The expected email subject is:

`Word Count Result`

The expected message body is:

```text
The word count in the sample.txt file is 11.
```

Repeat the upload with additional text files containing different numbers of words to verify the processing behavior.

### Step 12: Monitor Lambda Execution

If the email does not arrive:

1. Open **AWS Lambda → WordCountFunction**.
2. Select **Monitor**.
3. Click **View CloudWatch logs**.
4. Open the latest log stream.
5. Inspect the execution logs for errors.

Check the following common issues:

- The SNS email subscription has not been confirmed.
- The `SNS_TOPIC_ARN` environment variable is missing or incorrect.
- The S3 trigger references the wrong bucket.
- The Lambda function lacks the necessary execution-role permissions.
- The uploaded file does not match the configured suffix filter.
- The file contains text that cannot be decoded as UTF-8.

### Step 13: Capture Evidence and End the Lab

After successful testing:

1. Capture a screenshot of the Lambda function and its code.
2. Keep a copy of one successful word-count notification email.
3. Forward the test email and screenshot to the instructor as required.
4. Return to the lab instructions.
5. Click **End Lab**.
6. Click **Yes** to confirm.
7. Close the lab termination panel when appropriate.

---

## 4. Expected Results

| Test | Expected result |
|---|---|
| SNS topic creation | `WordCountTopic` is created |
| Email subscription | Subscription is confirmed |
| Lambda deployment | `WordCountFunction` is deployed |
| S3 trigger | Object-created events invoke Lambda |
| Sample file upload | Lambda reads the uploaded file |
| Word-count calculation | Whitespace-separated words are counted |
| SNS notification | Email is published with subject `Word Count Result` |
| Email body | Contains the filename and calculated word count |

## 5. Important Concepts Learned

### Event-Driven Architecture

Instead of manually invoking the function, an S3 event automatically triggers the processing workflow.

### Serverless Computing

AWS Lambda executes code without requiring the user to provision or maintain a server.

### Object Storage

Amazon S3 stores files as objects identified by bucket names and object keys.

### Messaging and Notifications

Amazon SNS decouples the processing function from the email delivery mechanism.

### IAM Execution Roles

Lambda uses its execution role to access permitted AWS services without embedding credentials in the source code.

### Environment Variables

Environment variables make it possible to configure resource identifiers without hardcoding them into the function.

### Monitoring and Debugging

CloudWatch Logs provides information about Lambda execution and helps identify failures.

## 6. Troubleshooting Reference

| Problem | What to check |
|---|---|
| SNS email not received | Confirm the subscription and inspect spam or junk folders |
| Lambda not invoked | Verify the S3 trigger, bucket selection, and event type |
| `AccessDenied` from S3 | Verify the Lambda execution role's S3 permissions |
| `AccessDenied` from SNS | Verify the role's SNS publishing permissions |
| Missing environment variable | Check the spelling of `SNS_TOPIC_ARN` |
| Function execution failure | Inspect the latest CloudWatch log stream |
| Unexpected word count | Check the file contents and whitespace-separated counting behavior |

## 7. Conclusion

This challenge demonstrates how AWS services can be combined to automate file processing using an event-driven serverless architecture. Amazon S3 acts as the event source, AWS Lambda performs the word-count operation, and Amazon SNS delivers the result through email.

The lab provides practical experience with Python-based Lambda functions, S3 event notifications, SNS subscriptions, IAM execution roles, environment variables, and CloudWatch monitoring.

---

## Lab Information

- **Lab ID:** `177-[JAWS]`
- **Lab Title:** `[Challenge] AWS Lambda Exercise`
- **AWS Region:** `us-west-2` — US West (Oregon)
- **Runtime:** Python
- **Primary Services:** AWS Lambda, Amazon S3, Amazon SNS, AWS IAM, Amazon CloudWatch
- **Repository suggestion:** `aws-lambda-word-count-lab`
