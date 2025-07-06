**📦 AWS-SQS-SNS-CloudWatch-Setup**
This guide demonstrates how to create an SQS queue, subscribe it to an SNS topic, and set up CloudWatch for monitoring and alerts. It's helpful for building decoupled, event-driven architectures on AWS.

**✅ Prerequisites**
AWS Account

IAM user with permissions for SQS, SNS, CloudWatch

AWS CLI configured or access to AWS Console

**1️⃣ Step 1: Create an SNS Topic**
Using AWS Console:
Go to Amazon SNS.

Click Topics > Create topic.

Select Standard.

Provide a name, e.g., MySNSTopic.

Click Create topic.

Using AWS CLI:
bash
Copy
Edit
aws sns create-topic --name MySNSTopic
**2️⃣ Step 2: Create an SQS Queue**
Using AWS Console:
Go to Amazon SQS.

Click Create Queue.

Choose Standard Queue.

Name it, e.g., MySQSQueue.

Leave default settings and create it.

Using AWS CLI:
bash
Copy
Edit
aws sqs create-queue --queue-name MySQSQueue
**3️⃣ Step 3: Subscribe SQS Queue to SNS Topic**
**Steps:
Get the ARN of both:

SNS Topic: aws sns list-topics

SQS Queue: aws sqs get-queue-attributes

Allow SNS to send messages to SQS by updating the SQS access policy.

json
Copy
Edit
{
  "Version": "2012-10-17",
  "Id": "sns-sqs-policy",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "SQS:SendMessage",
      "Resource": "arn:aws:sqs:REGION:ACCOUNT_ID:MySQSQueue",
      "Condition": {
        "ArnEquals": {
          "aws:SourceArn": "arn:aws:sns:REGION:ACCOUNT_ID:MySNSTopic"
        }
      }
    }
  ]
}**
Attach this policy under SQS > Access policy section (or via CLI set-queue-attributes).

Subscribe SQS to SNS:

bash
Copy
Edit
aws sns subscribe \
  --topic-arn arn:aws:sns:REGION:ACCOUNT_ID:MySNSTopic \
  --protocol sqs \
  --notification-endpoint arn:aws:sqs:REGION:ACCOUNT_ID:MySQSQueue
**4️⃣ Step 4: Publish a Test Message to SNS**
bash
Copy
Edit
aws sns publish \
  --topic-arn arn:aws:sns:REGION:ACCOUNT_ID:MySNSTopic \
  --message "Test message from SNS to SQS"
Check SQS queue to confirm message delivery.

**5️⃣ Step 5: Set Up CloudWatch Monitoring & Alarms**
Monitor SQS Metrics:
Go to CloudWatch > Metrics > SQS.

Select the queue (ApproximateNumberOfMessagesVisible, NumberOfMessagesSent, etc.).

Create an Alarm:

Example: Alert if messages > 10 for 5 minutes.

Monitor SNS Metrics:
Go to CloudWatch > Metrics > SNS.

Check NumberOfMessagesPublished, NumberOfNotificationsFailed, etc.

Set an alarm for failures if needed.

**6️⃣ Step 6: Set CloudWatch Alarm Notifications via SNS**
Create a new SNS topic for alarms (e.g., CloudWatchAlertsTopic).

Add your email as a subscription.

Attach this topic as the notification target when creating alarms.

**📁 Folder Structure (Optional for Repo)**
pgsql
Copy
Edit
aws-sqs-sns-cloudwatch-setup/
├── README.md
├── policies/
│   └── sns-sqs-policy.json
├── scripts/
│   ├── create-sns.sh
│   ├── create-sqs.sh
│   └── subscribe.sh

**🔚 Conclusion**
You’ve now set up:

An SNS Topic

An SQS Queue

Subscription between SNS → SQS

CloudWatch metrics and alarms

This setup is useful for:

Decoupling microservices

Event-based architecture

Monitoring queues & handling failures
