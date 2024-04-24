  # Automating the Stopping and Starting of Virtual servers (EC2)

   ![Enrollment](Images/Virtualserver.png)

  # Description:

  This project automates the starting and stopping of EC2 instances using AWS Lambda functions triggered by events from Amazon EventBridge. The automation helps in optimizing costs and resource utilization by ensuring that EC2 instances are only running when necessary.
  #

# Overview :

**The automation involves the following components:**

**2 EC2 Instances:** The project assumes the presence of two EC2 instances that need to be started and stopped based on specific events.

**IAM Policy:** An IAM policy is created to grant the necessary permissions for the Lambda functions to interact with EC2 instances.

**IAM Role:** An IAM role is assigned to the Lambda functions, allowing them to assume the necessary permissions defined in the IAM policy.

**2 Lambda Functions:** Two Lambda functions are created to handle the start and stop actions for the EC2 instances.

**EventBridge:** EventBridge is configured with rules to capture events that trigger the Lambda functions. These events could include scheduled times for instance shutdowns or custom events indicating changes in workload demand.

**CloudWatch:** CloudWatch is used for monitoring and logging the execution of Lambda functions and EventBridge rules, providing insights into the automation process.


# Benefits:

**Cost Savings:** Automation ensures that EC2 instances are only running when necessary, reducing idle time and minimizing costs associated with unused compute resources. By leveraging automation to start and stop instances based on demand, you can optimize spending and maximize cost efficiency within your AWS environment.

**Resource Optimization:** By automating the start and stop actions for EC2 instances, you can optimize resource utilization and avoid overprovisioning. This ensures that computing resources are allocated efficiently, leading to improved performance and scalability of your applications while minimizing waste.

**Operational Efficiency:** Automating routine tasks such as starting and stopping EC2 instances streamlines operations, reduces manual intervention, and frees up valuable time for IT teams to focus on more strategic initiatives. This improves overall operational efficiency and allows teams to allocate resources more effectively.

**Scalability and Flexibility:** Automation enables your infrastructure to scale dynamically in response to changing workload demands. By automatically adjusting the number of running   instances based on traffic patterns or scheduled events, you can ensure that your system remains responsive and resilient to fluctuations in demand.

**Reliability and Consistency:** Automation helps maintain a consistent and reliable environment by reducing the risk of human error and ensuring that tasks are executed according to predefined rules and schedules. This leads to improved system reliability, increased uptime, and enhanced user experience.



# What I Learned :

**Through this project, I gained valuable insights into several key aspects of AWS infrastructure automation:**

__AWS Lambda:__ I learned how to create and deploy Lambda functions to execute specific tasks within my AWS environment. 

  This includes understanding how to write Lambda code, configure triggers, and manage function execution.

**Amazon EventBridge:** I explored the capabilities of EventBridge in orchestrating automation workflows by capturing and processing events from various AWS services. 

  This involved setting up rules to trigger actions based on specific events, such as scheduled times.

**IAM Policies and Roles:** I developed a deeper understanding of AWS Identity and Access Management (IAM), particularly in creating policies and roles to manage permissions for Lambda   functions. 

  This included defining granular permissions to ensure secure access to EC2 instances.

**EC2 Instance Management:** I learned practical techniques for managing EC2 instances more efficiently, including automating start and stop actions based on workload requirements. 

  This helped optimize resource utilization and reduce costs by only running instances when needed.

**Monitoring and Logging:** I explored the importance of monitoring and logging in AWS automation workflows using CloudWatch. 

  This included setting up logging for Lambda function executions and EventBridge rule triggers to track performance and troubleshoot issues effectively.

# Steps:

# 1. Create EC2 Instances: 

  Ensure that you have at least two EC2 instances that you want to manage using this automation.

 # ![Enrollment](Images/EC2.png)

# 2. IAM Policy:

  Create an IAM policy granting permissions for starting and stopping EC2 instances. 
  
  Example policy statements can be found in the iam-policy.json file.

  On IAM, Navigate to Policy and choose create Policy

  Specify Permisiions, choose JSON and replace with Policy provided.

  ![Enrollment](Images/IAM-Policy.png)

  Review and Create. Providing Policy name.
  
  # ![Enrollment](Images/IAM-Policy2.png)

# 3. IAM Role: 

  Create an IAM role and attach the IAM policy created in step 2. 
  
  This role will be assumed by the Lambda functions to execute the necessary actions.

  On IAM, Navigate to Role and choose create Role

  Choose Aws services as trusted entity, Choose Lambda for Use case then next
  
  ![Enrollment](Images/role1.png)

  Add permission and choose the Policy created ealier then Next
  
  ![Enrollment](Images/role2.png)

  Enter a Role Name, Review and Create role
  
 # ![Enrollment](Images/role3.png)

# 4. Lambda Functions: 

  Open the Lambda console, and then choose Create function.
  
  Choose Author from scratch.
  
  Under Basic information, enter the following information:
  
  For Function name, enter a name that describes the function, such as "StopEC2Instances".
  
  For Runtime, choose Python 3.9.
  
  Under Permissions, expand Change default execution role.
  
  Under Execution role, choose Use an existing role.
  
  Under Existing role, choose the IAM role.
  
  Choose Create function.

 # ![Enrollment](Images/Lambda1.png)

  On the Code tab, under Code source, paste the following code into the editor pane of the code editor on the lambda_function tab. 
  
  This code stops the instances that you identify:

  Replace us-west-1 with the AWS Region that your instances are in. 
  
  Replace InstanceIds with the IDs of the instances that you want to stop and start.

  Choose Deploy.
  
  ![Enrollment](Images/Lambda2.png)

  On the Configuration tab, choose General configuration, and then choose Edit.
  
  Set Timeout to 10 seconds, and then choose Save.
  
  ![Enrollment](Images/Lambda3.png)

  Repeat steps to create another function. Complete the following steps so that this function starts your instances:
  
  Enter a different Function name. For example, "StartEC2Instances".
  
  Paste the following code into the editor pane of the code editor on the lambda_function tab:

  # Test Function:
  
  Open the Lambda console, and then choose Functions.
  
  Choose one of the functions(StopEC2Instance).
  
  Choose the Code tab.
  
  In the Code source section, choose Test.
  
  In the Configure test event dialog box, choose Create new test event.
  
  Enter an Event name. Then, choose Save.

  ![Enrollment](Images/Lambda4.png)

  Choose Test to run the function.
  
  ![Enrollment](Images/Lambda5.png)

  Instances Stopping
  
  ![Enrollment](Images/Lambda6.png)

  Repeat steps for the other function(StartEC2Instance).
  
 # ![Enrollment](Images/Lambda7.png)

# 5. EventBridge Rules: 

  Open the EventBridge console.
  
  Select Create rule.
  
  Enter a name for your rule, such as "StopEC2Instances". 
  
  For Rule type, choose Schedule, and then choose Continue in EventBridge Scheduler.
  
 # ![Enrollment](Images/rule1.png)

  Under Schedule pattern, for Occurrence, choose Recurring schedule.
  
  For Schedule type Cron-based schedule, enter an expression that tells Lambda when to stop your instance.
  
  ![Enrollment](Images/rule2.png)

  In Select targets, choose Lambda function from the Target dropdown list.
  
  For Function, choose the function that stops your instances.
  
  Choose Skip to review and create, and then choose Create.
  
  ![Enrollment](Images/rule3.png)
  
  ![Enrollment](Images/rule4.png)
  
  ![Enrollment](Images/rule5.png)

  Repeat steps create a rule to start your instances. Complete the following steps as for StopEC2Instance:
  
  Enter a name for your rule, such as "StartEC2Instances".
  
  ![Enrollment](Images/rule6.png)

  Event Schedule Create.
  
 # ![Enrollment](Images/rule7.png)
    
 

# 6. CloudWatch Logs: 

  Monitor the execution of Lambda functions and EventBridge rules using CloudWatch logs. 
  
  Ensure that appropriate logging is enabled to capture relevant information.

   ![Enrollment](Images/log1.png)
   
  ![Enrollment](Images/log2.png)
  
#  ![Enrollment](Images/log3.png)

# Usage :

  Once the setup is complete, the automation will automatically start and stop the EC2 instances based on the configured EventBridge rules. Monitor the CloudWatch logs for any issues or   unexpected behavior.

  Refer : https://repost.aws/knowledge-center/start-stop-lambda-eventbridge
