---
title: "Week 5 Worklog"
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---


### Week 5 Objectives:

* Grasped and understood the technologies Route 53, CLI, DynamoDB, ElastiCache (Redis), and CloudFront.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        | Note			   												|
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------|
| 2   | - Create a public hosted zone and add a record pointing a domain to an IP address                                                                                                   		       | 10/06/2025 | 10/06/2025      |	<https://000010.awsstudygroup.com/>	  | Learn how Route 53, as a scalable DNS service, maps domain names to IP addresses.				   		| 
| 3   | - Install and configure the AWS CLI. Practice basic commands to list S3 buckets or describe EC2 instances                                              						       | 10/07/2025 | 10/07/2025      | <https://000011.awsstudygroup.com/>	  | I will understand that the AWS CLI is a tool for managing AWS services via the command line, enabling automation		|
| 4   | - Create a simple DynamoDB table. Use the console to add, update, and delete items 														       | 10/08/2025 | 10/08/2025      | <https://000060.awsstudygroup.com/> 	  | Learn that DynamoDB is a managed NoSQL database and how to perform basic CRUD operations				   	|
| 5   | - Launch a Redis cluster. Connect to it and practice basic SET and GET commands                            											       | 10/09/2025 | 10/09/2025      | <https://000061.awsstudygroup.com/> 	  | Lnderstand how ElastiCache (Redis) works as an in-memory caching service to improve application performance			| 
| 6   | - Create a CloudFront distribution for an S3 bucket containing a static website                                                                                     				       | 10/10/2025 | 10/10/2025      | <https://000094.awsstudygroup.com/> 	  | Learn that CloudFront is a CDN that speeds up content delivery by caching it at edge locations				|


### Week 5 Achievements:

* Gained practical experience with AWS core services across multiple categories
  * Networking & Content Delivery: Route 53 (DNS), CloudFront (CDN).
  * Database & Storage: DynamoDB (NoSQL), ElastiCache for Redis (In-memory caching).
  * Management Tools: AWS CLI (Command-line interface).

* Configured DNS management using Amazon Route 53
  * Created a public hosted zone and added an A record mapping a custom domain name to a public IP address.
  * Understood how Route 53 enables scalable and reliable domain name resolution for web applications.

* Installed and configured AWS CLI for command-line management
  * Set up Access Key, Secret Key, and Default Region.
  * Practiced commands such as listing S3 buckets and describing EC2 instances
  * Learned how the AWS CLI facilitates automation and scripting for managing AWS resources.

* Built a DynamoDB table to explore NoSQL concepts:
  * Created a table via AWS Management Console.
  * Practiced CRUD operations (Create, Read, Update, Delete) on data items.
  * Understood DynamoDB’s serverless and managed nature for handling structured data at scale.

* Deployed an ElastiCache (Redis) cluster to enhance performance:
  * Launched and connected to a Redis cluster.
  * Executed SET and GET commands to store and retrieve cached data.
  * Understood the role of in-memory caching in reducing database load and improving response time.

* Implemented CloudFront for content delivery optimization:
  * Created a CloudFront distribution for an S3-hosted static website.
  * Learned how caching at AWS edge locations accelerates global content delivery and enhances user experience.

* Developed a cohesive understanding of how AWS services interconnect:
  * Route 53 for DNS mapping, CloudFront for content delivery, and DynamoDB/Redis for data management.
  * Recognized how these services collectively support scalability, reliability, and performance in modern cloud architectures.
