# AWS Capstone-Project
# Project Description
In this project, I had the opportunity to demonstrate my solution design skills.
I applied the architectural design principles in this project to:
- Deploy a PHP application that runs on an Amazon Elastic Compute Cloud (Amazon EC2) instance.
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
   # Step 2: Download the project assets: Copy Link from the capstone project
  - wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/ILT-TF-200-ACACAD-20-EN/capstone-project/Example.zip
  - ls
  - mkdir Example
  - sudo unzip Example.zip -d Example/
  - sudo cp Example/* /var/www/html/
  - Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/
  - Check public IP of Cloud9 EC2 instance and paste into new tab(Now Webpage is not showing)
  - Choose Instances and select your instance.(Cloud9 created Instance-Start with aws-cloud9)
  - On the Security tab, view the inbound rules. Add HTTP protocol with 0.0.0.0/0 then again refresh your webpage it will shows the webpage
# Task 3: 
- Open terminal
-     # st
       
      

      
  
    
    
    
    
     
    
    


