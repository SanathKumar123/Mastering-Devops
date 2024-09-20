# Deploying a Web Application on AWS S3 and EC2

This guide help to deploy web application on an AWS S3 bucket and an EC2 instance.

## Links to part 1 

http://sanathsbucketforassignment1.s3-website-us-east-1.amazonaws.com

## Link to part 2

Elastic IP - 54.210.51.171

## You can access part 2 from part 1

## PART 1

## Step 1: Create an S3 Bucket

1. Log in to your AWS Management Console at https://console.aws.amazon.com
2. Navigate to the S3 service.(Search for s3 in search box)
3. Click on the "Create bucket" button.
4. Choose a globally unique bucket name (my bucket name - sanathsbucketforassignment1 ) and select the region for bucket (I have chosen US East (N. Virginia)).
5. Under Permissions, uncheck "Block all public access" for public accessibility.
6. Accept default settings
7. Click "Create bucket" to finish.
8. Once the bucket is created, select the newly created bucket from the S3 dashboard.
9. Click on the "Properties" tab.
10. Scroll down to the "Static website hosting" and click on it.
11. Choose "Enable" (To use this bucket to host a website).
12. Enter the index document (about.html) and error document (error.html).
13. Click "Save" to apply the changes.


## Step 2: Upload Your Web Application to the S3 Bucket

1. Open the newly created S3 bucket.
2. Click on the "Upload" button.
3. Click on the "Add files" button and Select your web application files from your local machine.
4. Click "Upload" to upload your files to the bucket.

## Step 3: Configure Bucket Policy for Public Access 

1. Navigate to the bucket's properties.
2. Click on "Permissions" and then "Bucket Policy".
3. Add a policy:

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::sanathsbucketforassignment1/*"
        }
    ]
}

4. Save the policy.

## step 4: Test Website:

1. In the S3 console, select your bucket and go to "Properties".
2. Under "Static website hosting", copy the website endpoint (http://sanathsbucketforassignment1.s3-website-us-east-1.amazonaws.com).
3. Paste this endpoint into your web browser. You should see about page.

## PART 2

## Step 1: Launch an EC2 Instance

1. Navigate to the EC2 service.
2. Click on "Launch Instance" and Choose an appropriate Amazon Machine Image (AMI) like "Ubuntu Server 24.04 LTS".
3. Select an instance type like "t2.micro" (free tier eligible).
4. Choose a key pair or create a new one for SSH access.
5. Create a new security group or use an existing one.
6. Configure rules: SSH access from "Anywhere" (port 22), HTTP access from "Anywhere" (port 80), HTTPS access from "Anywhere" (port 443).
7. Leave remaining settings as default and click "Launch Instance".

## Step 2: Deploy Your Web Application on the EC2 Instance

1. Select EC2 instance and click on connect
2. Install and Configure a Web Server:
    sudo apt update
    sudo apt upgrade
3. Install Apache HTTP Server:
    sudo apt install apache2
    sudo systemctl start apache2
    sudo systemctl enable apache2
4. Copy your web application files from the S3 bucket to your EC2 instance
    Upload survey page to s3 bucket and grant public access
    wget https://sanathsbucketforassignment1.s3.amazonaws.com/survey.html
5. Move the downloaded file to the Apache document root:
    sudo mv survey.html /var/www/html/index.html

## Step 3: Accessing Your Deployed Web Application

1. Select the EC2 instance and copy the public IP address of your EC2 instance (Elastic IP - 54.210.51.171) and paste it in web browser, you can access the web page.

## Optional: Adding an Elastic IP to the EC2 Instance
1. Open the AWS Management Console, click the EC2 link, and display the page associated with your region.
2. Click the Elastic IPs link in the EC2 Dashboard.
3. Click Allocate New Address and choose VPC or EC2 from the drop-down list, depending on whether you're going to associate this IP with an instance in Amazon EC2-Virtual Private Cloud (VPC) or Amazon EC2-Classic, respectively. Click Yes, Allocate to confirm your choice.
4. Right-click the newly created Elastic IP and choose Associate Address.
5. Choose your desired EC2 instance from the drop-down list of running instances and click Associate.