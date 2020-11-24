# iamRoleManager
A simple role manager for creating IAM Roles on AWS. This is a part of a large project. This project allows you to create IAM role by providing:

1. Role name
2. Trust Policy Document
3. Then you can provide policy document

# Prerequisites
  
Python 3.6+
pip 9.0.1 (atleast)
Boto3 Python package 
AWS Credentials (APP KEY and APP KEY SECRET) stored in credentials file

# Operations allowed

This library allows you to:
  1. Create IAM Role
  2. Attach IAM Policy to the created role
  
# Sample initialization script:

    if __name__ == "__main__":
    irm = iamRoleManager()

    role_name = "transcoder_role"
    policy_name = "transcoder_role_policy"
    role_description = "Role for creating pipeline for transcoding pipeline"
    trust_document = {
        "Version": "2012-10-17",
        "Statement": [
            {
            "Effect": "Allow",
            "Principal": {
                "Service": "elastictranscoder.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
            }
        ]
    }

    role_policy = {"Version":"2008-10-17","Statement":[{"Sid":"1","Effect":"Allow","Action":["s3:Put*","s3:ListBucket","s3:*MultipartUpload*","s3:Get*"],"Resource":"*"},{"Sid":"2","Effect":"Allow","Action":"sns:Publish","Resource":"*"},{"Sid":"3","Effect":"Deny","Action":["s3:*Delete*","s3:*Policy*","sns:*Remove*","sns:*Delete*","sns:*Permission*"],"Resource":"*"}]}

    resp = irm.create_iam_role(role_name, role_description, trust_document)
    print( f"Response Create Iam Role: \n{resp} \n")

    resp = irm.attach_iam_policy(role_name, policy_name, role_policy)
    print( f"Response Attach Iam Policy: \n{resp} \n")
