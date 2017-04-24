# DevOps exercise AWS 04: Infrastructure-As-Code

## Introduction

AWS resources and infrastructure should always be created using a configuration management system such as [Cloudformation](https://aws.amazon.com/cloudformation/) or [Terraform](https://www.terraform.io/). This ensures all created AWS resources are tracked and reinstallable.  It also encourages sharing and collaboration and can help an infrastructure recover from a disaster.

## Exercise

1. Install one AWS EC2 Linux instance using either `AWS Cloudformation` or `Terraform`.
2. The instance must allow Internet traffic from any IP to ports `80` and `443` only.
3. The instance must allow login via SSH, but only from a definable IP or IP range (*in CIDR notation*) and must require an AWS `Key Pair`.
4. **Inputs:** The configuration code should accept two parameters:
  * The IP or IP range (*in CIDR notation*) for allowing SSH to the instance
  * The AWS Region to install the resources in (Dev's notes: Cloudformation doesn't support selecting regions from inside a template. Resources are always created in the region where you are using Cloudformation from, so for instance if you want your resources to be created in Canada you have to access Cloudformation through the following URL: https://ca-central-1.console.aws.amazon.com/cloudformation)
5. **Outputs:** The configuration code should output the Public IP of the created EC2 instance
6. Document how to create and how to destroy the AWS resources.

## Acceptance Criteria

* The solution is completely installable using the chosen configuration management system 
(*the key pair may be created manually and referenced by name in the configuration code*).
* The configuration code requires parameters `AWS Region` and `IP or IP Range` (no hard-coded values)
* The configuration code outputs the Public IP of the created EC2 instance
* All resources can be cleaned up (destroyed) with the chosen configuration system.

## Bonus Points!

Pre-install `docker` software on the instance. When `docker run -d -p80:80 -p443:443 httpd` is run manually on the instance, the [Apache docker image](https://hub.docker.com/_/httpd/) will run and the default Web page should be viewable under the instance's `Public IP` address.

## Guidelines/Hints

* Create the EC2 instance with type `t2.micro` or `t2.nano`
* Login via SSH should use an `AWS Key Pair`. Due to limitations of the configuration tools, the `Key Pair` may be created manually in the AWS Web Console and referenced by name in the configuration code.
* Use your own private AWS account to test the installation (EC2 instance qualifies for free-tier)
* Use an official **Amazon Linux AMI** owned by Amazon.
* You can either install resources without specifying a specific VPC/subnet **OR** install resources in a new VPC/subnet created with the configuration management system

## How to contribute your solution:

1. Fork the repo
2. Commit everything that you do in your fork
3. Give us a link to your fork
4. Provide meaningful comments with your commits

# Solution

I used Cloudformation for my solution, the resulting template is the WeltTestInfrastructure.template file in the repo root.

## How to use

1. Log in to AWS
2. Create a key pair named 'WeltKP' (you can do this by navigating to the EC2 service and select the 'Key Pair' option under the 'Network & Security' section. Also note that key pairs are region based, so it has to be created in the same region where you want to create the stack.)
3. Navigate to the Cloudformation service
4. Select the region where you want your resources to be created
5. Select the 'Create Stack' option
6. Select the upload template option, and upload the WeltTestInfrastructure.template file, then click next
7. Choose a name for your stack, and define a valid CIDR address for SSH access
8. On the options page click next
9. Review your template and click create

After completing the above steps AWS needs a couple of minutes to create all your resources. Note that even after you're able to log in to your EC2 instance, Docker may need some time to finish installation.