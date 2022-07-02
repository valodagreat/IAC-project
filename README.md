
# IAC-Project

IAC Project is a project that automates the deployment of my web app and web server. **AWS CloudFormation** was the IAC tool used to automate the deployment. I deployed my index.html using **AWS S3 bucket and Cloudfront**, while I deployed my server using **AWS EC2 instance** and **Ansible** as the configuration management tool for the server.

## Setting up the Environment

* Set up AWS by running this command
```bash
aws configure
```
This would prompt you to add your **AWS Access Key ID** and **AWS Secret Access Key**, you'll also be prompted to add your **region** and **output format**

* Create an S3 bucket and deploy the index.html
```bash
# Create S3 bucket using cloudformation
aws cloudformation deploy --template-file bucket.yml --stack-name "Name" --parameter-overrides MyBucketName="BucketName" 

# Wait for bucket to be created successfully and
# Upload index.html file to S3 bucket
# Assuming the bucket name is BucketName
aws s3api put-object --bucket BucketName --key index.html --body index.html
```

* Deploy S3 bucket to Cloudfront
```bash
aws cloudformation deploy --template-file cloudfront.yml --stack-name "Name2" --parameter-overrides MyBucketName="BucketName"
```

You should have your index.html running on cloudfront

* Create EC2 instance for the server
```bash
aws cloudformation deploy --template-file template.yml --stack-name "Name3" --parameter-overrides AMItoUse="ami" KeyName="keyname.pem" InstanceType="t3.small"
```

* Edit the Inbound access to the security group of your instance by opening port 3000

* Install Ansible by visiting [Download Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

* Add the Public IP of your instance to the Inventory file

* Run ansible playbook by specifying key file here or define it in your ansible.cfg file
```bash
ansible-playbook -i inventory.txt main.yml --key-file "~/.ssh/mykey.pem"
```
