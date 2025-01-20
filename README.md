# 30 Days DevOps Challenge - NBA Game Day Notifications / Sports Alerts System

## **Project Overview**
This project is an alert system that sends real-time NBA game day score notifications to subscribed users via SMS/Email. The system provides sports fans with up-to-date game information. The project demonstrates cloud computing principles and efficient notification mechanisms. It leverages:
- **Amazon SNS**
- **AWS IAM Roles and Policies**
- **Code as a Service**
- **AWS Lambda** 
- **Python Development**
- **Event Driven Notifications**
- **Amazon EventBridge**
- **SportsData IO NBA API** 

## **Features**
- Fetches live NBA game scores using an external API.
- Sends formatted score updates to subscribers via SMS/Email using Amazon SNS.
- Scheduled automation for regular updates using Amazon EventBridge.
- Designed with security in mind, following the principle of least privilege for IAM roles.


## **Prerequisites**
- Free account with subscription and API Key at [SportsData IO](https://sportsdata.io/)
- Personal AWS account with basic understanding of AWS and Python


## **Technical Architecture**
![Gameday notification system diagram](https://github.com/joesghub/gameday-notifications/blob/main/screenshots/gameday%20notification%20system%20diagram.png?raw=true)


## **Technologies**
- **Cloud Provider**: AWS
- **Core Services**: SNS, Lambda, EventBridge
- **External API**: NBA Game API (SportsData.io)
- **Programming Language**: Python 3.x
- **IAM Security**:
  - Least privilege policies for Lambda, SNS, and EventBridge.


## **Project Structure**
```
game-day-notifications/
├── src/
│   ├── gd_notifications.py          # Main Lambda function code
├── policies/
│   ├── gb_sns_policy.json           # SNS publishing permissions
│   ├── gd_eventbridge_policy.json   # EventBridge to Lambda permissions
│   └── gd_lambda_policy.json        # Lambda execution role permissions
├── .gitignore
└── README.md                        # Project documentation
```

## **Setup Instructions**
This journey outlines the step-by-step process of creating a cloud-based notification system using AWS services, including SNS, Lambda, and EventBridge. From cloning the initial repository to setting up automation, the project demonstrates a structured approach to integrating external APIs, securing resources with IAM policies, and leveraging AWS to deliver real-time notifications. It demonstrates a practical implementation of serverless architecture, integration of external APIs, and cloud DevOps principles, with opportunities for future scalability, customization, and resilience.

**Impact from a Cloud DevOps Perspective**
- **Scalability**: The use of AWS Lambda ensures that the system scales seamlessly based on the volume of notifications, avoiding over-provisioning or resource wastage.
- **Automation**: By integrating EventBridge, critical processes such as notification scheduling and execution are fully automated, reducing manual intervention and potential for error.
- **Security and Best Practices**: Following the principle of least privilege with IAM policies ensures secure access to resources, mitigating risks in a cloud environment.
- **Cost Efficiency**: The serverless architecture leverages pay-as-you-go pricing, keeping operational costs low while maintaining high availability and reliability.
- **Resilience and Monitoring**: Using AWS CloudWatch Logs enables proactive error detection and performance monitoring, ensuring a robust and dependable notification system.

### **Clone the Repository**
```bash
git clone https://github.com/ifeanyiro9/game-day-notifications.git
cd game-day-notifications
```

### **Create an SNS Topic**
1. Open the AWS Management Console.
2. Navigate to the SNS service.
3. Click Create Topic and select Standard as the topic type.
4. Name the topic (e.g., gd_topic) and note the ARN.
5. Click Create Topic.
![Creating a topic](https://github.com/joesghub/gameday-notifications/blob/main/screenshots/1.%20creating%20a%20topic.png?raw=true)

### **Add Subscriptions to the SNS Topic**
1. After creating the topic, click on the topic name from the list.
2. Navigate to the Subscriptions tab and click Create subscription.
3. Select a Protocol:
- For Email:
  - Choose Email.
  - Enter a valid email address.
- For SMS (phone number):
  - Choose SMS.
  - Enter a valid phone number in international format (e.g., +1234567890).

4. Click Create Subscription.
![Creating a subscription](https://github.com/joesghub/gameday-notifications/blob/main/screenshots/2.%20creating%20a%20subscription.png?raw=true)
5. If you added an Email subscription:
- Check the inbox of the provided email address.
- Confirm the subscription by clicking the confirmation link in the email.
6. For SMS, the subscription will be immediately active after creation.
![Confirm subscription](https://github.com/joesghub/gameday-notifications/blob/main/screenshots/2a.%20confirm%20subscription.png?raw=true)

### **Create the SNS Publish Policy**
1. Open the IAM service in the AWS Management Console.
2. Navigate to Policies → Create Policy.
3. Click JSON and paste the JSON policy from gd_sns_policy.json file
4. Replace REGION and ACCOUNT_ID with your AWS region and account ID.
5. Click Next: Tags (you can skip adding tags).
6. Click Next: Review.
7. Enter a name for the policy (e.g., gd_sns_policy).
8. Review and click Create Policy.
![Creating policy for lambda to publish to sns](https://github.com/joesghub/gameday-notifications/blob/main/screenshots/3.%20creating%20policy%20for%20lambda%20to%20publish%20to%20sns.png?raw=true)

### **Create an IAM Role for Lambda**
1. Open the IAM service in the AWS Management Console.
2. Click Roles → Create Role.
3. Select AWS Service and choose Lambda.
4. Attach the following policies:
- SNS Publish Policy (gd_sns_policy) (created in the previous step).
- Lambda Basic Execution Role (AWSLambdaBasicExecutionRole) (an AWS managed policy).
5. Click Next: Tags (you can skip adding tags).
6. Click Next: Review.
7. Enter a name for the role (e.g., gd_role).
8. Review and click Create Role.
9. Copy and save the ARN of the role for use in the Lambda function.
![Creating role for lambda and attaching policy](https://github.com/joesghub/gameday-notifications/blob/main/screenshots/4.%20creating%20role%20for%20lambda%20and%20attaching%20policy.png?raw=true)

### **Deploy the Lambda Function**
1. Open the AWS Management Console and navigate to the Lambda service.
2. Click Create Function.
3. Select Author from Scratch.
4. Enter a function name (e.g., gd_notifications).
5. Choose Python 3.x as the runtime.
6. Assign the IAM role created earlier (gd_role) to the function.
![Building lambda function and attaching created role with policies](https://github.com/joesghub/gameday-notifications/blob/main/screenshots/5.%20building%20lambda%20function%20and%20attaching%20created%20role%20with%20policies.png?raw=true)
7. Under the Function Code section:
- Copy the content of the src/gd_notifications.py file from the repository.
- Paste it into the inline code editor.
8. Under the Environment Variables section, add the following:
- NBA_API_KEY: your NBA API key.
- SNS_TOPIC_ARN: the ARN of the SNS topic created earlier.
9. Click Create Function.
![Inserting environment variable into function python code](https://github.com/joesghub/gameday-notifications/blob/main/screenshots/6.%20inserting%20environment%20variable%20into%20function%20python%20code.png?raw=true)



### **Test the System**
1. Open the Lambda function in the AWS Management Console.
2. Create a test event to simulate execution.
3. Run the function and check CloudWatch Logs for errors.
4. Verify that SMS notifications are sent to the subscribed users.
![Testing lambda function](https://github.com/joesghub/gameday-notifications/blob/main/screenshots/7.%20testing%20lambda%20function.png?raw=true)

### **Set Up Automation with Eventbridge**
1. Navigate to the Eventbridge service in the AWS Management Console.
2. Go to Rules → Create Rule.
3. Select Event Source: Schedule.
4. Set the cron schedule for when you want updates (e.g., hourly).
5. Under Targets, select the Lambda function (gd_notifications) and save the rule.
![Scheduling gameday notifications](https://github.com/joesghub/gameday-notifications/blob/main/screenshots/9.%20scheduling%20gameday%20notifications.png?raw=true)


## **What We Learned**
1. Designing a notification system with AWS SNS and Lambda.
2. Securing AWS services with least privilege IAM policies.
3. Automating workflows using EventBridge.
4. Integrating external APIs into cloud-based workflows.
![Successful gameday notification](https://github.com/joesghub/gameday-notifications/blob/main/screenshots/8.%20successful%20gameday%20notification.png?raw=true)


## **Future Enhancements**
1. **Enhanced API Integration**: Expand the notification system to include more APIs, enabling notifications for additional sports leagues or other event categories.
2. **Multi-Region Deployment**: Improve system resilience and reduce latency by deploying the Lambda function and SNS topics across multiple AWS regions.
3. **User Management and Preferences**: Build a frontend or backend user management system where subscribers can set preferences for notification channels (email, SMS, or push) and notification frequency.
4. **Data Insights**: Integrate AWS Athena or QuickSight to analyze notification metrics, such as delivery rates and user engagement, providing actionable insights.
5. **CI/CD Pipelines**: Implement CI/CD workflows using tools like Jenkins, CodePipeline, or GitHub Actions to automate testing, deployment, and updates.
6. **Advanced Security**: Leverage AWS Secrets Manager for securely storing and managing sensitive environment variables like API keys.
7. **Dynamic Event Handling**: Use dynamic triggers in EventBridge to schedule events based on real-time data, such as game schedules fetched from APIs.
