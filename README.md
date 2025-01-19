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


## What is S3?
Amazon S3 (Simple Storage Service) is a cloud storage service provided by AWS that allows you to store, manage, and retrieve any amount of data (files, objects) over the internet. It's designed for scalability, durability, and accessibility, making it an essential service in cloud computing.
Key Features of S3
Object Storage:

S3 stores data as objects in a flat namespace (no hierarchical file system like traditional storage).
Each object consists of:
Data (your actual file content).
Metadata (information about the file, like its size, type, and other custom attributes).
A unique key (filename or identifier).
Buckets:

Data is stored in containers called buckets.
Each bucket has a unique name across all AWS regions.
Buckets can be configured for various purposes, such as static website hosting or data archiving.
Scalability and Durability:

S3 scales automatically to handle growing storage needs.
S3 is designed to provide 99.999999999% durability (11 nines), meaning your data is highly secure and protected from loss.
Access Control:

Fine-grained access policies using IAM roles, bucket policies, and Access Control Lists (ACLs).
Public or private access can be configured at the bucket or object level.
Cost-Efficiency:

Pay only for what you use (storage, retrieval, and data transfer).
Different storage classes allow cost optimization based on access patterns (e.g., S3 Standard, S3 Intelligent-Tiering, S3 Glacier).
Static Website Hosting:

S3 can serve static websites (HTML, CSS, JavaScript) by enabling the "Static Website Hosting" option.
This makes S3 ideal for hosting lightweight websites or content delivery.
Integration:

Works seamlessly with other AWS services, like CloudFront, Lambda, EC2, Athena, and Redshift.
Data Transfer:

Supports secure data transfer using SSL/TLS.
Allows uploading/downloading of data via the AWS Management Console, AWS CLI, SDKs, or APIs.
Versioning:

S3 allows you to keep multiple versions of the same object, which helps in accidental deletion or overwriting.
Common Use Cases
Static Website Hosting:

Serve HTML, CSS, JavaScript, and media files (images, videos) directly from S3.
Backup and Restore:

Use S3 for storing backups of data, databases, or systems.
Data Archiving:

Store infrequently accessed data in cost-effective storage classes like S3 Glacier.
Media Storage:

Host and deliver large media files (e.g., images, videos, audio).
Big Data Analytics:

Store raw and processed datasets for analysis with tools like AWS Athena, Redshift, or Spark.
Disaster Recovery:

Store critical data across multiple AWS regions for high availability and disaster recovery.
Application Data Storage:

Use S3 to store logs, application assets, or user-uploaded files in web or mobile applications.
Benefits of S3
Highly Available: S3 ensures your data is accessible from anywhere in the world with low latency.
Secure: Advanced encryption, access controls, and monitoring to protect your data.
Cost-Effective: Choose from multiple storage classes based on your needs.
Easy Integration: Works with almost every other AWS service and third-party tools.
Developer-Friendly: Access your data programmatically via the S3 API, CLI, or SDKs.

## What is CloudFront?
Amazon CloudFront is a Content Delivery Network (CDN) service provided by AWS that speeds up the delivery of your web content (e.g., HTML, CSS, JavaScript, images, videos, APIs) to users by caching it at locations (called edge locations) close to them. This reduces latency, improves load times, and enhances the user experience.

Here’s a breakdown of CloudFront and how it works:

Key Features of CloudFront
Global Edge Network:

CloudFront has a vast network of edge locations worldwide.
Content is cached at these edge locations, so when a user requests data, it is served from the nearest edge location instead of the original server (called the origin).
Origin:

The origin is the source of the content CloudFront delivers.
Common origins include:
Amazon S3 (e.g., for static files like HTML, CSS, and images).
An HTTP server (e.g., running on EC2 or on-premises).
Elastic Load Balancers (ELBs) for dynamic websites.
Caching:

CloudFront caches content based on its Time to Live (TTL), which is defined in the cache settings.
Cached content reduces load on the origin server since subsequent requests are served from the cache.
HTTPS Support:

CloudFront provides SSL/TLS encryption for secure delivery (HTTPS).
It supports custom SSL certificates for delivering content over a custom domain.
Cost-Efficient:

By caching content at edge locations, CloudFront reduces data transfer costs from your origin server (e.g., S3 or EC2).
You only pay for the data served and requests made to CloudFront.
Dynamic Content Support:

Although CloudFront is optimized for static content, it can also accelerate dynamic content delivery using features like Lambda@Edge or by integrating with APIs.
DDoS Protection:

Built-in integration with AWS Shield provides protection against Distributed Denial of Service (DDoS) attacks.
Customizable Behavior:

You can configure CloudFront behavior for different paths (e.g., cache images differently than JavaScript files).
Use Lambda@Edge to execute custom logic at edge locations, such as modifying HTTP headers or serving different versions of content.
How CloudFront Works
Content is Originated:

You define an origin (e.g., an S3 bucket, EC2 instance, or on-premises server) that stores the original content.
First Request:

When a user requests content, CloudFront retrieves it from the origin server and stores it (caches it) at the closest edge location.
Subsequent Requests:

The cached content is served directly from the edge location to other users nearby, reducing latency and load on the origin server.
Cache Expiry:

Cached content is refreshed after the TTL expires or if you manually invalidate it.
Why Use CloudFront?
Low Latency:

Content is delivered from the edge location closest to the user, reducing latency.
High Performance:

CloudFront speeds up websites and applications by caching frequently accessed content.
Global Coverage:

With edge locations worldwide, CloudFront ensures a consistent and fast experience for users regardless of their location.
Cost Savings:

Reduces the load on your origin server and saves on data transfer costs from the origin.
Security:

Supports HTTPS and integrates with AWS Shield for DDoS protection.
Use Cases for CloudFront
Static Website Hosting:

Distribute static files like HTML, CSS, JavaScript, and images stored in S3.
Dynamic Content Acceleration:

Speed up APIs, dynamic websites, or database-driven applications.
Streaming Media:

Deliver video or audio content with minimal buffering.
Software Distribution:

Efficiently distribute large files like software installers or updates.
Secure Content Delivery:

Protect sensitive content using signed URLs or cookies.
Analogy for CloudFront
Imagine you run a pizza shop, and all pizzas are made in one central location (your origin server, e.g., S3). If customers all over the world order pizza, delivering from the central shop to distant locations would take time (high latency).

To solve this:

You open local branches (CloudFront edge locations) that pre-make popular pizzas (cached content).
When someone orders a pizza, they get it from the nearest branch instead of waiting for it to come from the main shop.
This improves delivery speed and reduces the load on your main shop.

## What do S3 and CloudFront do?
What S3 Does:
It acts as storage for your website files, such as HTML, CSS, JavaScript, images, and videos.
S3 is where your actual content is stored.
When static website hosting is enabled, S3 can directly serve these files via a public endpoint (e.g., http://bucket-name.s3-website-region.amazonaws.com).
Analogy:
Think of S3 as a "file server" or "repository" for your website assets. It holds all the content you want to display on your site.
It’s not a backend in the sense of having logic, databases, or APIs—it’s just file storage.
CloudFront: The Content Delivery Network (CDN)
What CloudFront Does:

CloudFront is a CDN (Content Delivery Network) that sits in front of S3.
Its job is to distribute your content globally, caching it at multiple edge locations closer to your users.
This improves performance, reduces latency, and allows for secure delivery (e.g., HTTPS).
Why Use CloudFront with S3?

If you access your website directly from the S3 bucket, all requests go to the S3 region where your bucket is hosted.
With CloudFront, cached copies of your website files are stored at edge locations around the world, reducing the load on your S3 bucket and making your site faster for users everywhere.
Analogy:

CloudFront is like a "delivery driver" that picks up files from S3 (or another origin) and delivers them to users quickly and efficiently.
How They Work Together
S3 stores your static website content (HTML, CSS, JS, etc.).
CloudFront fetches the content from S3, caches it at edge locations, and serves it to your users.
The user accesses your website via CloudFront’s domain (e.g., d123456abcdef.cloudfront.net), not directly via the S3 bucket URL.
So, What About Frontend and Backend?
CloudFront and S3 aren’t "frontend" or "backend" in the traditional web app sense:

Frontend refers to what the user sees (e.g., a React app, HTML files).
Backend refers to the server-side logic, APIs, and database handling.
In this case, your S3 bucket stores the static frontend files (e.g., HTML, CSS, and JavaScript). There’s no actual backend logic unless you integrate with services like AWS Lambda for dynamic content.

Why This Setup Works for Static Websites
Static websites don’t require backend logic since all the content is prebuilt and stored as files.
CloudFront makes your static website highly performant and globally accessible.

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

