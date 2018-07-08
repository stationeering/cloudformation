# CloudFormation for stationeering.com

CloudFormation used for the AWS infrastructure around stationeering.com, provided for reference for anyone who may be interested. Contains no credentials or otherwise security consious data.

## Deploying

* Ensure you have `awscli` installed.
* Using the AWS Console, deploy the `cloudformation-user.json` stack as `cloudformation-user`. 
* Using the AWS Console, deploy the `deploy-user.json` stack as `deploy-user`. 
* In IAM, find cloudformation user and mint new API credentials.
* Add credentials and default region to `~/.aws/credentials`.
* Create main DNS stack with MX records for mail.
```
AWS_PROFILE=cloudformation aws cloudformation create-stack --stack-name dns --template-body file://templates/dns.json
```
* Create certificate stack in **us-east-1** regardless of other stacks regions, may hang while domain validation is performed.
```
AWS_DEFAULT_REGION=us-east-1 AWS_PROFILE=cloudformation aws cloudformation create-stack --stack-name certificates --template-body file://templates/certificates.json
```
* Update `cloudfront.json` with new certificate ARNs.
* Create remaining cloudformation stacks via CLI:
```
AWS_PROFILE=cloudformation aws cloudformation create-stack --stack-name s3-storage --template-body file://templates/s3-storage.json
AWS_PROFILE=cloudformation aws cloudformation create-stack --stack-name cloudfront --template-body file://templates/cloudfront.json
```

## Updating

* Execute a line as needed.
```
AWS_DEFAULT_REGION=us-east-1 AWS_PROFILE=cloudformation aws cloudformation update-stack --stack-name certificates --template-body file://templates/certificates.json
AWS_PROFILE=cloudformation aws cloudformation update-stack --stack-name dns --template-body file://templates/dns.json
AWS_PROFILE=cloudformation aws cloudformation update-stack --stack-name s3-storage --template-body file://templates/s3-storage.json
AWS_PROFILE=cloudformation aws cloudformation update-stack --stack-name cloudfront --template-body file://templates/cloudfront.json
```