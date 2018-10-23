# Amazon Simple Storage Service (S3)

The Amazon Simple Storage Service (S3) support is implemented by using the [AmazonS3Client](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/S3/TS3Client.html) class provided in the Aws SDK for .NET.

## UrlFormat

Below is the expected format for Amazon S3 files in the Zephyr.FileSystem.

|Type|Format|Example
|----|------|-------
|File|s3://[bucket]/[path]/[file]|s3://mybucket/dir001/subdir001/myfile.txt
|Directory|s3://[bucket]/[path]/|s3://mybucket/dir001/subdir001/

**Note**: All directories must end in a slash.  Click [here](utilities.md#directory-vs-file-url) for more details.

## Client

A client is a generic term for an object any given implementation needs to sucessfully complete its tasks.   This is usually completely containted within the implemenation classes themselves, exposed through their constructors, but the Utilities classes, which parse URL's and return the cooresponding implementation objects will need these clients as well.

The Amazon Simple Storage Service (S3) implementations require a client.  The AwsClient class provides a wrapper around the [AmazonS3Client](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/S3/TS3Client.html) provided in the Aws SDK for .NET.

### Region Endpoints

Regions provided must match the region in which the bucket was created.  This can be found in the S3 console as shown below :

![Amazon S3 Console](../img/filesystem-s3console-region.png)

The exception to this rule is if you use the "US East (N. Virginia)" region to create your client.  This will allow access to all regions.

### Credentials

Credentials can be provide explicitly into the classes, or can be pulled from the user's AWS credentails and config files.  If neither of those exist, it will look for environment-based variables representing the current user and their credentials.

See [Configuring AWS Credentails](https://docs.aws.amazon.com/sdk-for-net/v3/developer-guide/net-dg-config-creds.html) for details and other way to establish Amazon credentails to the AwsClient.

### Encryption

Encryption of the objects put into a bucket can be specified at the client level.  By default, the objects will be placed into the bucket using the value specified on the client.

#### Server Side Encryption Method (SSEMethod)

The "SSEMethod" property on the AwsClient class specifies which server side encryption method should be used when putting objects into an S3 bucket.  The availible values are :

* **AWS256** : Use AES 256 server side encryption.
* **AWSKMS** : Use AWS Key Management Service for server side encryption.

### Storage Class 

The "StorageClass" property on the AwsClient class specifies which tier of storage should be used when putting objects into an S3 Bucket.  The available values are : 

* **Standard** : The default storage class for S3.  Durability 99.999999999%; Availability 99.99% over a given year.
* **ReducedRedundancy** : Provides the same availability as standard, but at a lower durability.  Durability 99.99%; Availability 99.99% over a given year.
* **Glacier** : Used for objects stored in Amazon Glacier (objects that are for archival purpose and where GET operations are rare).  Durability 99.999999999%
* **StandardInfrequentAccess** : Used for objects that are long-lived and less frequently acccessed, like backups and older data.  Durability 99.999999999%; Availability 99.9% over a given year.
* **OneZoneInfrequentAccess** : Similar to StandardInfrequentAccess, but only stores data within one Availability Zone in a given region.  Durability 99.999999999%; Availability 99% over a given year.
