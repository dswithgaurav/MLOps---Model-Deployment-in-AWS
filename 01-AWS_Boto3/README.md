# AWS Automation with Boto3 – EC2 and S3

### Prerequisites

* Python 3.7+
* AWS Account
* AWS CLI configured with credentials (`aws configure`)
* IAM user with proper EC2 and S3 permissions
* Install required packages:

```bash
pip install boto3
```

### Setup

Make sure your AWS credentials are configured:

```bash
aws configure
```

This will set up `~/.aws/credentials` and `~/.aws/config`.



## EC2 Operations

##### Create EC2 Instance

```python
import boto3

ec2 = boto3.resource('ec2')

instances = ec2.create_instances(
    ImageId='ami-0abcdef1234567890',  # Use a valid AMI ID in your region
    MinCount=1,
    MaxCount=1,
    InstanceType='t2.micro',
    KeyName='my-key-pair'  # Make sure this key exists
)
print(f"Launched EC2 Instance ID: {instances[0].id}")
```

##### List EC2 Instances

```python
for instance in ec2.instances.all():
    print(f'ID: {instance.id}, State: {instance.state["Name"]}, Type: {instance.instance_type}')
```

##### Stop an Instance

```python
ec2.Instance('i-0123456789abcdef0').stop()
```

##### Terminate an Instance

```python
ec2.Instance('i-0123456789abcdef0').terminate()
```



## S3 Operations

##### Create a Bucket

```python
import boto3

s3 = boto3.client('s3')

bucket_name = 'my-unique-bucket-name-123456'
s3.create_bucket(
    Bucket=bucket_name,
    CreateBucketConfiguration={'LocationConstraint': 'us-west-2'}
)
print(f'Bucket {bucket_name} created')
```

##### Upload a File

```python
s3.upload_file('local_file.txt', bucket_name, 'uploaded_file.txt')
```

##### List Buckets

```python
response = s3.list_buckets()
for bucket in response['Buckets']:
    print(f'Bucket Name: {bucket["Name"]}')
```

##### Download a File

```python
s3.download_file(bucket_name, 'uploaded_file.txt', 'downloaded_file.txt')
```

##### Delete a File

```python
s3.delete_object(Bucket=bucket_name, Key='uploaded_file.txt')
```



### Notes

* Use `boto3.client('ec2')` for low-level access and `boto3.resource('ec2')` for object-oriented access.
* Make sure bucket names are globally unique.
* EC2 AMI IDs are region-specific.
