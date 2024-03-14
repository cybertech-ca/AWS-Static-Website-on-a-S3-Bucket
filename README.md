<p align="center"><b><big>Creating an Static Website using an AWS S3 Bucket</big></b></p>


This CloudFormation code provides a blueprint for automatically creating the infrastructure to host a static website on Amazon Web Services (AWS). Let's break down the resources it creates and configures

**Description:** The code includes a human-readable description: "Creates an S3 bucket configured for hosting a static website, and a Route 53 DNS record pointing to the bucket". This clarifies the code's purpose at a glance.

**Parameters:** The code defines three parameters:
    
      **DomainName:*** This parameter expects the user to provide the existing domain name they want to use for the website (e.g., https://www.cybertech-ca.com/).

      **FullDomainName:** This parameter allows the user to specify the complete website URL, including any subdomain (e.g., (https://www.cybertech-ca.com/) or [invalid URL removed]).
        
      **AcmCertificateArn (Optional):** This parameter is for an optional SSL/TLS certificate ARN (Amazon Resource Name) from ACM. If provided, the code configures CloudFront to use HTTPS encryption for secure communication.


**Outputs:** The code defines three outputs that provide valuable information after the stack is deployed:

    **BucketName:** This outputs the name of the S3 bucket created for website content storage.
    **CloudfrontEndpoint:** This outputs the domain name of the CloudFront distribution, which is the public endpoint visitors will use to access your website.
    **FullDomain:** This outputs the full website domain name as specified by the user in the FullDomainName parameter.

**S3 Bucket (WebsiteBucket):**

* This is the heart of your website's storage. Imagine it as a secure folder in the cloud where you'll upload all your website's files like HTML pages, CSS stylesheets, JavaScript code, images, and other assets.
* The code configures the bucket with "PublicRead" access, allowing anyone to access the website content publicly over the internet.
* It also specifies "index.html" as the default file to load when someone visits your website. This means if they navigate to your domain name without specifying a file, "index.html" will be displayed. Similarly, "404.html" is set as          the error document for situations like "page not found".

**S3 Bucket Policy (WebsiteBucketPolicy):**

This policy defines the access permissions for your S3 bucket. In this case, it allows the public (anyone on the internet) to use the s3:GetObject action, which essentially permits them to download objects (website files) from the bucket.

**CloudFront Distribution (WebsiteCloudfront):**

CloudFront acts as a middleman between your S3 bucket and your website visitors. When someone visits your website, CloudFront delivers the content from the closest server, significantly improving website loading speed for visitors in different regions.

  **key settings:** 

  **Origin:** Points to the S3 bucket where your website content is stored.

  **Default Root Object:** Similar to the S3 bucket configuration, this specifies "index.html" as the default file to serve when someone requests the root path of your website (e.g.,(https://www.cybertech-ca.com/).

  **Aliases:** This allows you to map your domain name (e.g., (https://www.cybertech-ca.com/) to the CloudFront distribution.

  **Default Cache Behavior:** Defines how CloudFront caches and delivers your website content. It allows GET and HEAD requests, compresses content for faster delivery, and redirects visitors to the HTTPS version of your website for improved     security (assuming an SSL certificate is configured).

  **Viewer Certificate (Optional):** This section allows you to integrate an SSL/TLS certificate from AWS Certificate Manager (ACM) to enable HTTPS encryption for secure communication between visitors and your website.
    CloudFront Origin Access Identity (cloudfrontoriginaccessidentity - Optional):

**Note:**
Other resources can be potential added. For example: An **OAI (Origin Access Identity)** can be used to grant specific users or applications controlled access to your S3 bucket from CloudFront while maintaining the public "read" access for the website content itself.

**Mappings:** It can also be added, and this section can be helpful in certain scenarios. It allows you to define mappings for values that might differ based on the AWS region you're deploying the stack in. For example, you could map region names to their corresponding S3 Hosted Zone IDs for website hosting.

**Route 53 Record Set Group (WebsiteDNSName):**

Route 53 is AWS's Domain Name System (DNS) service. It acts like a phonebook for the internet, translating your domain name (e.g., https://www.example.com/) into the IP address of the server hosting your website.
This code creates a record in Route 53 that points your domain name to the CloudFront distribution. This ensures that when someone enters your domain name in their browser, they're directed to the CloudFront servers hosting your website content.

<p align="center">**Additional Notes**</p>

This code snippet is a template and may require adjustments based on your specific needs.
Security best practices recommend using HTTPS encryption for websites. While not explicitly shown in this example, you can integrate an ACM certificate with the CloudFront distribution to enable HTTPS.
CloudFormation offers a powerful way to automate infrastructure provisioning on AWS. This example demonstrates its capabilities for deploying a static website.
