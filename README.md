# Amazon-s3-with-spring-boot

we'll learn how to interact with the Amazon S3 (Simple Storage Service) storage system programmatically, from Java.

Remember that S3 has a very simple structure – each bucket can store any number of objects which can be accessed using either a SOAP interface or an REST-style API.

Going forward, we'll use the AWS SDK for Java to create, list, and delete S3 buckets. We'll also upload, list, download, copy, move, rename and delete objects within these buckets.

## Maven Dependencies
Before we get started, we need to declare Spring Cloud Starter AWS dependency in our project:

```javasript
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-aws</artifactId>
    <version>2.2.6.RELEASE</version>
</dependency>
```
To view the latest version, check Maven Central.

## Prerequisites
To use AWS SDK, we'll need a few things:

AWS Account: we need an Amazon Web Services account. If you still don't have any, go ahead and create an account
AWS Security Credentials: These are our access keys that allow us to make programmatic calls to AWS API actions. We can get these credentials in two ways, either by using AWS root account credentials from access keys section of Security Credentials page or by using IAM user credentials from IAM console
Choosing AWS Region: We have to select an AWS region(s) where we want to store our Amazon S3 data. Keep in mind that S3 storage prices vary by region. For more details, head over to the official documentation. For this tutorial, we'll use US East (Ohio) (region us-east-2)
## Creating Client Connection
First, we need to create a client connection to access Amazon S3 web service. We'll use AmazonS3 interface for this purpose:
```javasript
AWSCredentials credentials = new BasicAWSCredentials(
  "<AWS accesskey>", 
  "<AWS secretkey>"
);
```
And then configure the client:
```javasript
AmazonS3 s3client = AmazonS3ClientBuilder
  .standard()
  .withCredentials(new AWSStaticCredentialsProvider(credentials))
  .withRegion(Regions.US_EAST_2)
  .build();
```
## Amazon S3 Bucket Operations <br>
### Creating a Bucket
It's important to note that the bucket namespace is shared by all users of the system. So our bucket name must be unique across all existing bucket names in Amazon S3 (we'll find out how to check that in just a moment).


Moreover, as specified in the official documentation, the Bucket names must comply with the following requirements:

names should not contain underscores
names should be between 3 and 63 characters long
names should not end with a dash
names cannot contain adjacent periods
names cannot contain dashes next to periods (e.g., “my-.bucket.com” and “my.-bucket” are invalid)
names cannot contain uppercase characters
Let's create a bucket:

```javasript
String bucketName = "baeldung-bucket";

if(s3client.doesBucketExist(bucketName)) {
    LOG.info("Bucket name is not available."
      + " Try again with a different Bucket name.");
    return;
}
s3client.createBucket(bucketName);
```
