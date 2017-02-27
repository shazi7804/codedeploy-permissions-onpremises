# CodeDeploy generate permissions with On-Premises
This example provides all the permissions to build Codedeploy on-premises

## Note
In this example you will create the following permissions：

- A S3 bucket

- A IAM USER for Travis CI

    - attach policy: AWSCodeDeployDeployerAccess

    - user policy
    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "Stmt1487528506000",
                "Effect": "Allow",
                "Action": [
                    "s3:*"
                ],
                "Resource": [
                    "arn:aws:s3:::codedeploy-*"
                ]
            }
        ]
    }
    ```

- A IAM USER for On-Premises (defualt: codedeploy-onpremises)

    - user policy

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "sts:AssumeRole"
                ],
                "Resource": "arn:aws:iam::account:role/Role-onpremises-*"
            }
        ]
    }
    ```

- A IAM Role for On-Premises (default: Role-onpremises)

    - trust role 

        ```json
        {
          "Version": "2012-10-17",
          "Statement": [
            {
              "Effect": "Allow",
              "Principal": {
                "AWS": "arn:aws:iam::account:root"
              },
              "Action": "sts:AssumeRole"
            }
          ]
        }
        ```

    - role policy

        ```json
        {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Sid": "Stmt1487527978000",
                    "Effect": "Allow",
                    "Action": [
                        "s3:GetObject",
                        "s3:GetObjectVersion",
                        "s3:ListBucket"
                    ],
                    "Resource": [
                        "arn:aws:s3:::codedeploy-*"
                    ]
                }
            ]
        }
        ```
- A IAM Role for CodeDeploy service (default: Role-CodeDeploy)

    - attach policy: AWSCodeDeployRole

- CodeDeploy application (default: $projectname)

    - Deployment config: CodeDeployDefault.AllAtOnce (default)
    - Deployment Group: dev, stage, prod

- Register On Premises at for CodeDeploy
    
    - default register dev, stage instance

## Auto Install

    $ chmod +x install
    $ ./install

## Config

### Customize

- ProjectName = Set your project name.
- OnPremises_Server = Register On Premises instance name

### Global
- Region = AWS region

### S3
- BucketName = Set you S3 bucket name

### IAM group

- IAMGROUP = Set IAM group name

## Other 
You can without auto install.

### Create S3 bucket.

    $ ./s3bucket

### Create IAM user(TravisCI) to provide Travis CI deploy.

    $ ./iamuser-travis

### Create IAM users(OnPremises) to provide On-Premises generate sts session.

    $ ./iamuser-onpremises

### Create IAM Role to provide On-Premises access S3, CodeDeploy.

    $ ./iamrole-onpremises

### Create IAM Role to provide CodeDeploy service.

    $ ./iamrole-codedeploy

### Create CodeDeploy application, deployment_group

    $ ./codedeploy    

### Register On Premises instance for CodeDeploy

    $ ./register-onpremises
