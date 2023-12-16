# S3and-lambda
This Repo is to perform lambda fucntion using boto 3 created using Blueprint. Create a folder in the bucket called as ‘upload’ when a file is uploaded in this bucket, rename this file and put that in the another folder called as ‘renamed’. The rename file should have a prefix ‘rename_‘. The upload folder gets deleted once teh file is renamed an dmoved to renamed folder 

#This is completed using lambda function Subhra-lambda-Fn and Storage S3 account name-Subhras3. theh below puthon code has to be written on the CODE tab of teh labda function . After writing the below Code click on the Deploy to save the chnages 
import json
import boto3

s3 = boto3.client('s3')

def lambda_handler(event, context):
    for record in event['Records']:
        if record['eventName'] == 'ObjectCreated:Put':
            bucket = record['s3']['bucket']['name']
            key = record['s3']['object']['key']
            
            # Check if the file is in the 'upload' folder
            if 'upload/' in key:
                # Extract the file name and its extension
                filename = key.split('/')[-1]
                file_extension = filename.split('.')[-1]
                
                # Rename the file by adding a prefix 'rename_'
                renamed_filename = 'rename_' + filename
                
                # Copy the file from the 'upload' folder to the 'renamed' folder
                copy_source = {
                    'Bucket': bucket,
                    'Key': key
                }
                s3.copy(copy_source, bucket, 'renamed/' + renamed_filename)
                
                # Remove the original file from the 'upload' folder
                s3.delete_object(Bucket=bucket, Key=key)
******************************************************************************************************************************************************************************
The Secnd lambda function tab is TEST. Type teh below command and edit the bucket name with yur own bucket name provided all the access has been granted to the Lambda fucntion to access data in teh S3 bucket . once done Test an dsave teh file if no errors

To check the File and program if it fucntions as expected uplpad a file to the S3 bucket or create a folder an dlet it upload and see it renamed with the Renanmed suffix and moved to the renamed folder 

{
  "Records": [
    {
      "eventVersion": "2.0",
      "eventSource": "aws:s3",
      "awsRegion": "us-east-1",
      "eventTime": "1970-01-01T00:00:00.000Z",
      "eventName": "ObjectCreated:Put",
      "userIdentity": {
        "principalId": "EXAMPLE"
      },
      "requestParameters": {
        "sourceIPAddress": "127.0.0.1"
      },
      "responseElements": {
        "x-amz-request-id": "EXAMPLE123456789",
        "x-amz-id-2": "EXAMPLE123/5678abcdefghijklambdaisawesome/mnopqrstuvwxyzABCDEFGH"
      },
      "s3": {
        "s3SchemaVersion": "1.0",
        "configurationId": "testConfigRule",
        "bucket": {
          "name": "subhras3",
          "ownerIdentity": {
            "principalId": "EXAMPLE"
          },
          "arn": "arn:aws:s3:::subhras3"
        },
        "object": {
          "key": "harappacul.txt",
          "size": 1024,
          "eTag": "0123456789abcdef0123456789abcdef",
          "sequencer": "0A1B2C3D4E5F678901"
        }
      }
    }
  ]
}

