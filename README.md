# WordPress Scalable and Highly-Available Web Application

## Overview
This guide provides steps to deploy a highly available WordPress website on AWS Elastic Beanstalk with Amazon RDS for robust performance. By following these steps, you can create a scalable and reliable WordPress website that can handle high levels of traffic.

<img width="1440" alt="281100617-9538a2e1-9f23-4319-ab3b-b3fba8e623bb" src="https://github.com/r-ramos2/WordPress-Scalable-and-Highly-Available-Deployment/assets/90172092/78ab960d-0b15-41b4-adf3-7debd7a7b67c">

## AWS Architecture
<img width="1012" alt="wp-diagram" src="https://github.com/r-ramos2/WordPress-Scalable-and-Highly-Available-Deployment/assets/90172092/dff10108-842f-4baa-927b-c87a9aff3bda">

## WordPress Setup
1. Download and extract WordPress from the official WordPress website.
2. Compress all files inside the extracted folder.
3. Move the new `wordpress.zip` to the Desktop for upload.

## IAM Role Creation
1. Open IAM on a separate tab.
2. On the left menu, choose "Roles".
3. Click on "Create role" and name it `aws-elasticbeanstalk-service-role`.
4. Under "Trusted entity type", choose "AWS service".
5. Under "Use case", choose "Elastic Beanstalk".
6. For "Choose a use case for the specified service", select "Elastic Beanstalk", then click "Next".
7. Under permission policies, use `AWSElasticBeanstalkServiceRolePolicy`, then click "Next".
8. Under "Name, review, and create", click on "Create role".

Repeat the above steps to create another role named `aws-elasticbeanstalk-ec2-role` with the following changes:
1. In step 5, choose "EC2" for the "Use case".
2. In step 8, use `AmazonS3FullAccess` and `CloudWatchAgentServerPolicy` for the permission policies.

## Elastic Beanstalk Setup
1. Open Elastic Beanstalk.
2. On the left menu, choose "Applications".
3. Click on "Create application".
4. Choose a name for "Application name".
5. Click on "Create".
6. On the left menu, choose "Environments".
7. Click on "Create environment".
8. Under "Environment tier", choose "Web server environment".
9. Under "Application name", write a name of your choice (i.e. `wordpress`).
10. Under "Platform", choose "PHP".
11. Leave default settings.
12. Under "Application code", choose "Sample application" or "Upload your code".
13. If you upload your code, choose "Local file".
14. Then upload the `wordpress.zip` file you created.
15. Under "Version label", type `v1`.
16. Under "Presets", choose "Single instance" (e.g. free tier eligible), then click "Next".

## Service Access and VPC Settings
1. Under "Service access", choose "Use an existing service role".
2. Under "Existing service roles", choose `aws-elasticbeanstalk-service-role`.
3. Under "EC2 instance profile", choose `aws-elasticbeanstalk-ec2-role`, then click "Next".
4. Under "VPC", choose the default VPC.
5. Under "Instance subnets", choose two availability zones (i.e. `us-east-1a` and `us-east-1b`).
6. Under Choose database subnets, choose two availability zones (i.e. `us-east-1a` and `us-east-1b`).
7. Toggle the "enable database" button.
8. Under Database settings, choose Engine=`mysql`, instance class=`db.t3.micro` (e.g. newer and cheaper than db.t2.micro), Storage=`5`, `a username`, `a password`, Availability=`Low (one AZ)` (e.g. multi-AZ incurs a fee due to high-availability), Database deletion policy=`Delete`.
9. Click "Next".
10. Under "EC2 security groups", choose default.
11. Under "Fleet composition", choose "On-demand instance".
12. Under "instance types", choose "t3.micro", then click "Next".
13. Under "Health reporting", choose "Basic", then click "Next".
14. Under "Review", click ""Submit"".
15. Wait until the Elastic Beanstalk environment finishes launching.
16. Under "Environment overview", wait until Health is Green.
17. Under "Domain", click on the link.
18. When you see the WordPress page, select a language, then click "Continue".
19. Click on "Let's go!"

## Database Setup
1. Open RDS on a separate tab.
2. On the left menu, choose "Databases".
3. Choose the database you just created.
4. Under "Configuration", find DB name.
5. Under "Connectivity & security", find the Endpoint address.
6. Go back to the WordPress page.
7. Enter the following information: Database=`wordpress`, Username=`ebdb`, Password=`any 8-characters`, Database Host=`Endpoint address`.
8. Click "Submit".

## Clean Up
Empty and terminate Elastic Beanstalk, RDS, S3 (e.g. might need to delete bucket policy), and EC2 instances. To avoid unnecessary costs, please double-check that no underlying resources are still running.

## Conclusion
In this project, we demonstrated how to deploy a high-availability WordPress website with an external Amazon RDS database to Elastic Beanstalk. By following these steps, we have created a scalable and reliable WordPress website that can handle high levels of traffic. Feel free to message me with code improvement suggestions.

## Acknowledgment
This tutorial is adapted from the [AWS Elastic Beanstalk tutorial for deploying a high-availability WordPress website](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/php-hawordpress-tutorial.html) provided by Amazon Web Services. We extend our gratitude to AWS for providing this valuable resource, which served as the foundation for the "Scalable and Highly-Available WordPress Deployment" tutorial.
