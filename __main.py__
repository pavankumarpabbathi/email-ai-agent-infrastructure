import pulumi
import pulumi_aws as aws
import pulumi_awsx as awsx ##Pulumi for Crosswalk
import json


b = aws.s3.Bucket("firstbucket",
    opts = pulumi.ResourceOptions(retain_on_delete=True),
    bucket="my-tf-test-187025654051-bucket",
    force_destroy=True,
    tags={
        "Name": "My bucket",
        "Environment": "Dev",
    })

##In Pulumi Sequence[str] = List of string

lambda_role_args = {
    "assume_role_policy": json.dumps({
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Principal": {
                    "Service": "lambda.amazonaws.com"
                },
                "Action": "sts:AssumeRole"
            }
        ]
    }),
    "description": "Email Sender Lambda Compute Enviroment IAM Role",
    "force_detach_policies": True,
    "managed_policy_arns": [
        "arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole",
        "arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess"
    ],
    "name": "email-sender-lambda-role",
    "tags": {
        "Environment": "Dev",
        "Owner": "DevOps"
    }
}

lambda_iam_role = aws.iam.Role("lambda_role",
    **lambda_role_args,
    opts = pulumi.ResourceOptions(depends_on=[b], protect=False)
)


ecr_repo_args = {
    "force_delete": True,
    "name": "email-sender-agent-repo",
    "image_scanning_configuration": {
        "scan_on_push": True
    },
    "tags": {
        "Environment": "Dev",
        "Owner": "DevOps"
    }
}

pulumi.export("bucket_name", b.id)
pulumi.export("role_arn", lambda_iam_role.arn)
pulumi.export("role_name", lambda_iam_role.id)
