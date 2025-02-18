Flask Web Application with Docker and EKS Deployment
### •	Step 1: Create a Flask Web Application
 Start by creating a basic Flask web application. Flask is a lightweight Python web framework that allows you to build web applications with ease.
### •	Step 2: Create a Dockerfile
A Dockerfile is a script used by Docker to build an image. Here's how to create a Dockerfile to containerize the Flask application.
### •	Step 3: Create a requirements.txt File
In the requirements.txt file, list the dependencies needed to run the Flask app, including Flask itself.
### •	Step 4: Preparing Your Environment
Before you can build your Docker image and deploy to ECR and EKS, make sure you have the following tools installed:
Docker: For building and running container images.
AWS CLI: For interacting with AWS services.
kubectl: The command line tool for Kubernetes.
eksctl: A tool for creating EKS clusters.
Make sure you have an AWS account set up with the appropriate IAM roles for ECR and EKS access.

### •	Step 5: Build and Push Docker Image to Amazon ECR
1. **Build the Docker Image:** First, build the Docker image using the docker build command:
**`docker build -t eks-flask-web-app .`**
2. **Tag the Docker Image:** Tag the image with your Amazon ECR repository URI:
**`docker tag my-flask-app:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-flask-app:latest`**
3. **Login to ECR:** Use the AWS CLI to authenticate Docker to your Amazon ECR registry:
**`aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
`**
4. Push the Docker Image to ECR: Finally, push the image to Amazon ECR:
**` docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-flask-app:latest `**

### •	Step 6: Step 6: Create Amazon ECR Repository
Before pushing your Docker image to ECR, you need to create an ECR repository:

In the AWS Management Console, go to ECR.
Click Create repository.
Name the repository (e.g.,** my-flask-app**) and create it.
After creating the repository, follow **Step 5** to push the image.

### •	Step 7: Setup Amazon EKS Cluster Requirements
You need to install and configure the following tools:

** eksctl: Install it via Homebrew or download it directly from GitHub.
** kubectl: This is the Kubernetes CLI used to interact with the EKS cluster.
** AWS CLI: Make sure your AWS CLI is configured with your IAM credentials.
### •	Step 8: Creating an Amazon EKS cluster
### •	Step 9: Create a Node Group
### •	Step 10: Create a Kubernetes Deployment
### •	Step 11: Test The Application
### •	Step 12: Cleanup
