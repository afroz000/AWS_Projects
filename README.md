# AWS_Projects

## Step 1: Set Up an S3 Bucket for Website Hosting
1.1. Create an S3 Bucket
Log in to your AWS Management Console.
Navigate to S3 and click Create bucket.
Enter a Bucket name (e.g., my-website-bucket) and select a region.
Under Object Ownership, make sure to disable "Block all public access".
Click Create bucket.
1.2. Upload Website Files
Open your newly created bucket.
Click Upload and add your website files (e.g., index.html, styles.css).
Make sure the index.html file is included as the entry point.
1.3. Enable Static Website Hosting
Go to the Properties tab of the bucket.
Scroll down to Static website hosting and click Edit.
Select Enable.
Specify the index document (e.g., index.html).
Optionally, add an error document (e.g., error.html).
Save changes.
1.4. Make Your Bucket Objects Public
Go to the Permissions tab of the bucket.

Click Bucket Policy and add a policy like the one below:

json
Copy
Edit
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-website-bucket/*"
    }
  ]
}
Replace my-website-bucket with your bucket name.

Save the policy.

Test public access by navigating to your S3 bucket endpoint, which will look like this:

php
Copy
Edit
http://<bucket-name>.s3-website-<region>.amazonaws.com
## Step 2: Set Up CloudFront for Content Delivery
2.1. Create a CloudFront Distribution
Navigate to CloudFront in the AWS Management Console.
Click Create Distribution.
Under Origin settings:
Origin Domain: Select your S3 bucket from the dropdown or manually enter the bucket URL (e.g., <bucket-name>.s3.amazonaws.com).
Origin Access Control (OAC): You can optionally set this up to secure access between CloudFront and your S3 bucket, but for simplicity, you can skip this for now.
Scroll to Default Cache Behavior:
Set Viewer Protocol Policy to "Redirect HTTP to HTTPS".
Scroll down and click Create Distribution.
2.2. Update S3 Bucket Policy for CloudFront
If you use OAC:

Go to your S3 bucket's Permissions tab.
Update the bucket policy to allow CloudFront to access objects.
2.3. Wait for CloudFront Deployment
CloudFront may take 10–15 minutes to deploy. Once complete, you’ll receive a CloudFront Domain Name (e.g., d1234567abcdef.cloudfront.net).

## Step 3: Test Your Website
Open your CloudFront Domain Name in a browser.
Your website should now be live, served through CloudFront.
## Step 4: (Optional) Configure a Custom Domain
Purchase a domain from Route 53 or another domain registrar.
Create a Route 53 Hosted Zone for your domain.
Add an Alias Record in Route 53 to point your domain to the CloudFront distribution.
## Step 5: (Optional) Add HTTPS
CloudFront automatically provides HTTPS for your distribution. If using a custom domain:

Request a free SSL/TLS Certificate from AWS Certificate Manager (ACM).
Attach the certificate to your CloudFront distribution.
## Step 6: Clean Up Resources (If Testing Only)
If this is a learning project, don’t forget to delete your S3 bucket and CloudFront distribution to avoid incurring costs.
*------------*

## What Are Buckets?
Think of an S3 bucket as a container or folder that holds objects (your data). It’s the top-level structure in S3 that organizes and stores your data.

Buckets are created by you, the user, and you can store an unlimited number of objects in them.
Each bucket must have a unique name globally across AWS because it’s part of the public namespace (like a domain name).
Buckets help you group related data, set access permissions, and manage your stored objects efficiently.
## What Are Objects?
An object is the actual piece of data that you upload to S3. This can be any type of file—images, videos, text files, application data, backups, etc.

Every object consists of:
Key: The unique identifier (or name) of the object within the bucket (e.g., file1.jpg or folder/file2.txt).
Data: The actual content of the object (e.g., the binary data of a file).
Metadata: Information about the object, such as its size, content type, or custom tags.
For example:

Bucket Name: my-website-assets
Object Key: images/logo.png
Object Data: The binary data of the logo image file.
Relationship Between Buckets and Objects
Buckets are like containers or directories, while objects are the files stored inside these containers.
For example, if you’re storing website assets:
Bucket: my-website-assets
Objects:
images/logo.png
css/styles.css
index.html

Bucket Limits: Each AWS account can have 100 buckets by default (can be increased upon request).
You can store unlimited objects and data in a bucket.

