
``# ECS Project with Terraform

This repository contains the Terraform configuration to provision resources in AWS for your Flask application running on ECS. It sets up an ECS cluster with a task definition, load balancer, and autoscaling for your Flask containers.

## Prerequisites

Before you begin, make sure you have the following:

1. [Terraform](https://www.terraform.io/) installed on your local machine. We recommend using the latest stable version.

2. AWS CLI installed and configured with the necessary credentials and profiles. Ensure you have profiles for each AWS account you want to use.

3. Docker container image for your Flask application pushed to an Amazon ECR repository. Update the image URL in the `main.tf` file in the `aws_ecs_task_definition` resource to point to your image.

4. **Important**: CodeBuild Role with "AmazonEC2RegistryPowerUser" Policy

   To enable CodeBuild to pull Docker images from Amazon ECR, it requires appropriate permissions.
   For this project, the CodeBuild role must be attached with the "AmazonEC2RegistryPowerUser" policy. This policy grants the necessary permissions for CodeBuild builders to pull Docker images from the specified ECR repositories. Ensure that the CodeBuild role has this policy attached to avoid any issues during the build and deployment process.


## Flask Application

The Flask application for this project is located in the `src` directory:

- `src/app.py`: This file contains the Flask application code. It defines a simple route that displays the hostname and IP address of the container.

- `src/Dockerfile`: The Dockerfile used to build the Docker container for the Flask application. It sets up the application in a Python 3.10.0-alpine environment.

- `templates/index.html`: The HTML template used by the Flask application to display the hostname and IP address.


## CodePipeline

The CodePipeline automates the build and deployment process of your Flask application to Amazon ECR. It uses the following files in the repository:

- `buildspec.yml`: This file contains the build and deployment instructions for CodePipeline. It uses Docker to build the application's container image and pushes it to Amazon ECR.

- `imagedefinitions.json`: This file is automatically generated by the CodePipeline and contains the image URI of the Docker container in Amazon ECR.


## Getting Started

1. Clone this repository to your local machine:

``
git clone https://github.com/ArielDomchik/ECS-Terraform-CodePipeline-Deployment``
``cd ecs-project/terraform-configuration`` 

2.  Initialize Terraform and download the required providers in `terraform-configuration`:

`terraform init` 

3.  Review and adjust the `variables.tf` file to customize the configuration to your needs.
    
4.  Deploy the resources to AWS:

`terraform apply` 

6.  Confirm the execution by typing `yes` when prompted.

## Accessing the Application

Once the Terraform provisioning is complete, the Flask application will be accessible through the ALB endpoint. The application's route displays the hostname and IP address of the container.
* Terraform will output the load balancer url

## Customizing the Configuration

You can customize this Terraform configuration to fit your specific requirements:

-   Adjust the number of containers to run in the `desired_count` variable in `main.tf`.
    
-   Modify security group rules to suit your application's requirements.
    
-   Change the AWS region in the `provider "aws"` block in `main.tf` and other appropriate locations.
    

## Destroying Resources

When you're done testing and want to tear down the infrastructure, run:

`terraform destroy` 

Confirm the execution by typing `yes` when prompted.
