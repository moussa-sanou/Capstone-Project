# AWS Capstone-Project
# Project Description
In this project, I had the opportunity to demonstrate my solution design skills.
I applied the architectural design principles in this project to:
- Deploy a PHP application running on an Amazon Elastic Compute Cloud (Amazon EC2) instance.
- Create a database instance that the PHP application can query.
- Create a MySQL database from an SQL dump file.
- Update application parameters in an AWS System Manager Parameter Store.
- Secure the application to prevent public access to backend systems.
# Project Details
![image](https://github.com/moussa-sanou/Capstone-Project/assets/58495791/ed9283c3-3f60-45d1-807b-e38d249cd34c)
# Task 1: Create a MySQL RDS database instance
# Step 1: Create Subnet Groups
- In RDS choose Subnet Groups
    # Subnet Group Details
  - Name: Social-DB-subnet
  - Description: Social-DB-subnet
  - VPC-Select: Example VPC
  - # Add Subnet
  - AZ-Select: us-east-1a (private subnet 1)
  - us-east-1b1b (private subnet 2)
  - Private subnet1 (10.0.2.0/23)
  - private subnet 2 (10.0.4.0/23)
# Step 2: Create Database
- Navigate RDS Service in AWS and create db instance
- RDS -> Database -> Create Database
    # Create database
  - Choose a database creation method: Standard create
  - Engine options: Select MySQL
  - Templates: Dev/Test
  - Availability and durability: Multi-AZ DB instance
  - # Settings
  - DB instance identifier: Example
  - # Credentials Settings:
  - Master username: admin
  - Master password: password
  - Confirm password: password
  - # Instance Configuration
  - DB instance class: Burstable classes (includes t classes)
  - t3.micro
  - # Connectivity
  - Virtual private cloud (VPC): Example VPC
  - VPC Security Group: Example-DB
  - # Database options
  - Initial database name: exampledb
  - Backup: uncheck it
  - Monitoring: Disable monitoring
  - Click Create database
# Task 2: Create Cloud9 Environment
- Navigate cloud9
- Create cloud environment
    # Step 1: Name environment
  - Name: capstone project
   # Step 2: Network settings
  - Network (VPC): Example VPC
  - Subnet: Public Subnet 2
  - Click Next
  - Create Environment
# Task 3: Install a LAMP web server on Amazon Linux2 cloud9 instance
- Enter command in cloud9 instance 
    # Step 1: Prepare the LAMP server
  - sudo yum update -y
  - sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
  - sudo yum install -y httpd mariadb-server
  - sudo systemctl start httpd
  - sudo systemctl enable httpd
  - sudo systemctl is-enabled httpd
   # Step 2: Download the project assets Copy Link from the capstone project
  - wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/ILT-TF-200-ACACAD-20-EN/capstone-project/Example.zip
  - ls
  - mkdir Example
  - sudo unzip Example.zip -d Example/
  - sudo cp Example/* /var/www/html/
  - Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
  - Check public IP of Cloud9 EC2 instance and paste into new tab(Now Webpage is not showing)
  - Choose Instances and select your instance.(Cloud9 created Instance-Start with aws-cloud9)
  - On the Security tab, view the inbound rules. Add HTTP protocol with 0.0.0.0/0 then again refresh your webpage it will shows the webpage
# Task 4: Import the Data into the database
- Enter command in cloud9 instance 
    # Step 1: Download Database
  - wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/ILT-TF-200-ACACAD-20-EN/capstone-project/Countrydatadump.sql
  - Go to RDS dashboard now your database instance is created
  - Copy the endpoint of DB
  - Go to Cloud9 service to access the machine
  - Database file is downloaded successfully
  - Go to Security Group select Example-DB and add inbound rule for MYSQL/Aurora and source to cloud9 instance then only we can able to import the database
  - Come back cloud9 instance
   # Step 2: Import the file
  - mysql -u admin -p exampledb --host <rds-endpoint> < Countrydatadump.sql
  - It asks password then give password(copy password from master password) then hit enter
  - Verify once again database is attached or not by using the following command
  - mysql -u admin -p --host <endpoint> 
  - MySQL [(none)]>  use exampledb;
  - MySQL [exampledb]> show tables;
  - MySQL [exampledb]> select*from countrydata_final;
  - MySQL [exampledb]> exit;
# Task 5: Configure Parameter values in AWS systems manager
- Navigate to AWS systems manager and create parameter
    # Step 1: Create Parameter
  - /example/endpoint   <rds-endpoint>
  - /example/username  admin
  - /example/password   password
  - /example/database   exampledb
   # Step 2: Modify IAM role to cloud9 instance
  - Go to cloud9 EC2 instance and attach IAM role-  Inventory-App-Role
  - Refresh the web page again
  - Now you can access the database successfully
# Task 6: Create AMI for Autoscaling(Cloud9 instance)
- AMI on cloud9 instance
    # Step 1: Take an AMI on cloud9 instance
  - Select cloud9 instance   -->   actions-->i   mages and templates-->   create image
  - Image Name-   CapstoneProjectAMI
  - Description-    AMI for CapstoneProject
  - Create Image
# Task 7: Create Load Balancer
- Open the Amazon EC2 Console
    # Step 1: Create Load Balancer
  - Go to EC2 console and select Load Balancer in new tab
  - Select Create Load Balancer
  - Select Application Load Balancer
  - Load balancer name- CapstoneProject-LB
  - Network mapping
  - VPC-Select Example VPC
  - Mappings-Select both us-east-1a below subnet choose Public subnet1  & us-east-1b below subnet
  - choose Public subnet2
    # Step 2: Security groups
  - Security groups-Select   ALBSG security group
  - Listeners and routing
  - Default action-Create Load Balancer
    # Step 3: Create Target Group
  - Specify group details
  - Choose a target type-instance
  - Target group name- CapstoneProject-TG
  - VPC-Ensure Example VPC is selected then click next
  - Register targets
  - 2 available Target is there so click Create Target Group
  - Come back to Load balance and refresh this Listener and routing, now we can see the created target group
  - Select CapstoneProject-TG
  - Then click Create Load Balancer
  - Copy the DNS Name
# Task 8: Create AutoScaling
- EC2 management console under Auto Scaling choose Auto Scaling Groups in new tab
    # Step 1: Create Auto Scaling group
  - Choose launch template or configuration
  - Name
  - Auto Scaling group name- praveen-ASG
  - Launch template 
  - Launch template-Choose Example-LT then modify the template
  - Change the AMI ID we created as CapstoneProjectAMI
  - Select Example-LT go to details   -->  Actions  -->  Modify Templates
  - Scroll down and on Launch Templates Contents choose our CapstoneProjectAMI ID then create it
  - Ensure the CapstoneProjectAMI ID is changed in our template then click next
    # Step 2: Launch Instance
  - Choose instance launch options
  - Network
  - Availability Zones and subnets-Select Private subnet1 & Private subnet2 then click next
    # Step 3: Configure advanced options
  - Load balancing - optional-Select Attach to an existing load balancer
  - Attach to an existing load balancer
  - Existing load balancer target groups-Select CapstoneProject-LB
  - Health checks - Select ELB(check mark) then click next
    # Step 4: Configure group size and scaling policies
  - Group size -Increase the group size
  - Desired capacity: 2
  - Minimum capacity: 2
  - Maximum capacity: 2
  - Click Next
  - Add notifications
  - Click Next
  - Add tags
  - Add name tag and value as sami-CapstoneProject and click next
  - Review  then click Create Auto Scaling group

       
      

      
  
    
    
    
    
     
    
    


