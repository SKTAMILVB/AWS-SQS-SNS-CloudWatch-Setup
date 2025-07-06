**📦 AWS-SQS-SNS-CloudWatch-Setup**

This guide demonstrates how to create an SQS queue, subscribe it to an SNS topic, and set up CloudWatch for monitoring and alerts. It's helpful for building decoupled, event-driven architectures on AWS.

**✅ Prerequisites**
AWS Account
IAM user with permissions for SQS, SNS, CloudWatch
AWS CLI configured or access to AWS Console

**1️⃣ Step 1: Create an SNS Topic**
Using AWS Console:
1.Go to Amazon SNS.
2.Click Topics > Create topic.
3.Select Standard.
4.Provide a name, e.g., MySNSTopic.
5.Click Create topic.
**Using AWS CLI:
aws sns create-topic --name MySNSTopic**

**2️⃣ Step 2: Create an SQS Queue**
Using AWS Console:
1.Go to Amazon SQS.
2.Click Create Queue.
3.Choose Standard Queue.
4.Name it, e.g., MySQSQueue.
5.Leave default settings and create it.
**Using AWS CLI:
**aws sqs create-queue --queue-name MySQSQueue****

**3️⃣ Step 3: Subscribe SQS Queue to SNS Topic**
Steps:
1.Get the ARN of both:
2.SNS Topic: aws sns list-topics
3.SQS Queue: aws sqs get-queue-attributes
4.Allow SNS to send messages to SQS by updating the SQS access policy.
![image](https://github.com/user-attachments/assets/582ee29a-ce0b-4473-948b-6944b3d26aef)
Attach this policy under SQS > Access policy section (or via CLI set-queue-attributes).

Subscribe SQS to SNS:
![image](https://github.com/user-attachments/assets/4e2aa105-3780-47a8-a718-76bfefb43dde)
  
**4️⃣ Step 4: Publish a Test Message to SNS**
![image](https://github.com/user-attachments/assets/64e60ff6-e951-4fe8-803a-301735106faf)
Check SQS queue to confirm message delivery.

**5️⃣ Step 5: Set Up CloudWatch Monitoring & Alarms**
Monitor SQS Metrics:
1.Go to CloudWatch > Metrics > SQS.
2.Select the queue (ApproximateNumberOfMessagesVisible, NumberOfMessagesSent, etc.).
3.Create an Alarm:
       Example: Alert if messages > 10 for 5 minutes.
Monitor SNS Metrics:
1.Go to CloudWatch > Metrics > SNS.
2.Check NumberOfMessagesPublished, NumberOfNotificationsFailed, etc.
3.Set an alarm for failures if needed.

**6️⃣ Step 6: Set CloudWatch Alarm Notifications via SNS**
1.Create a new SNS topic for alarms (e.g., CloudWatchAlertsTopic).
2.Add your email as a subscription.
3.Attach this topic as the notification target when creating alarms.

**📁 Folder Structure (Optional for Repo)**
![image](https://github.com/user-attachments/assets/b5782d1a-b0e1-412c-aed9-20fbed3592f4)

**🔚 Conclusion**
You’ve now set up:
1.An SNS Topic
2.An SQS Queue
3.Subscription between SNS → SQS
4.CloudWatch metrics and alarms
This setup is useful for:
1.Decoupling microservices
2.Event-based architecture
3.Monitoring queues & handling failures
