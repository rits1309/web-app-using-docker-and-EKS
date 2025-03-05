# Deploying a Basic Web Application on Amazon EKS

Here’s how you can explain each step in a README.md file for a project involving Flask, Docker, and deployment on AWS using Amazon EKS and ECR:
Flask Web Application with Docker and EKS Deployment
This project demonstrates how to build and deploy a simple Flask web application using Docker, Amazon Elastic Container Registry (ECR), and Amazon Elastic Kubernetes Service (EKS).

## Step 1: Create a Flask Web Application
Start by creating a basic Flask web application. Flask is a lightweight Python web framework that allows you to build web applications with ease. 
Here's a minimal example of a Flask app:
### app.py:
```
from flask import Flask
app = Flask(__name__)
@app.route('/')
def home():
    return "Hello, World!"
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=80)
```
This is a simple "Hello, World!" application to test our Docker container and Kubernetes setup later.
## Step 2: Create a Dockerfile
A Dockerfile is a script used by Docker to build an image. Here's how to create a Dockerfile to containerize the Flask application.
### Dockerfile:
```
FROM python:3.9-slim 
WORKDIR /app
COPY . /app
RUN pip install --no-cache-dir -r requirements.txt
EXPOSE 80
CMD ["python", "app.py"]
```
### This Dockerfile will:
    Start with a slim Python image.
    Copy the current directory files into the Docker image.
    Install the required dependencies.
    Expose port 80 (as Flask runs on port 80).
    Define the command to run the Flask app.
## Step 3: Create a requirements.txt File
In the requirements.txt file, list the dependencies needed to run the Flask app, including Flask itself.
## requirements.txt:
```
Flask==2.1.0
```
This will install Flask when building the Docker image.
## Step 4: Preparing Your Environment
Before you can build your Docker image and deploy to ECR and EKS, make sure you have the following tools installed:
- **Docker**: For building and running container images.
- **AWS CLI**: For interacting with AWS services.
- **kubectl**: The command line tool for Kubernetes.
- **eksctl**: A tool for creating EKS clusters.
Make sure you have an AWS account set up with the appropriate IAM roles for ECR and EKS access.
## Step 5: Build and Push Docker Image to Amazon ECR
- **Build the Docker Image**: First, build the Docker image using the docker build command:
```
docker build -t my-eks-web-app .
```
- **Run Image:** Run the newly built image.
```
  docker run -p 5000:5000 my-eks-web-app
```
- Running Docker locally, point your browser to ```http://Server's public IP:5000/```
- **Tag the Docker Image**: Tag the image with your Amazon ECR repository URI:
```
docker tag my-flask-app:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-flask-app:latest
```
- Create an ECR repository to store your Docker images.
Make note of the repository URI.
- **Login to ECR**: Use the AWS CLI to authenticate Docker to your Amazon ECR registry:
```
aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
```
- **Push the Docker Image to ECR**: Finally, push the image to Amazon ECR:
```
docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-flask-app:latest
```
## Step 6: Create Amazon ECR Repository
Before pushing your Docker image to ECR, you need to create an ECR repository:
- In the AWS Management Console, go to ECR.
- Click Create repository.
- Name the repository (e.g., ```my-flask-app```) and create it.
- After creating the repository, follow Step 5 to push the image.
## Step 7: Setup Amazon EKS Cluster Requirements
You need to install and configure the following tools:
eksctl: Install it via Homebrew or download it directly from GitHub.
kubectl: This is the Kubernetes CLI used to interact with the EKS cluster.
AWS CLI: Make sure your AWS CLI is configured with your IAM credentials.
## Step 8: Creating an Amazon EKS Cluster
To create an EKS cluster, run the following eksctl command:
```eksctl create cluster --name my-flask-cluster --region <region> --nodegroup-name my-node-group --node-type t3.micro --nodes 3```
This command will create an EKS cluster with 3 t3.micro nodes in the specified region.

## Step 9: Create a Node Group
When you create an EKS cluster, you may also need to create a node group to run your applications. You can add this as part of the cluster creation process using eksctl or manually from the AWS console.

## Step 10: Create a Kubernetes Deployment
- Create a Deployment YAML file (deployment.yaml):
- Deploy to EKS: Run the following command to create the deployment in your EKS cluster:
```eksctl create cluster --name my-flask-cluster --region <region> --nodegroup-name my-node-group --node-type t3.micro --nodes 3 ```
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-app
        image: <aws_account_id>.dkr.ecr.<region>.amazonaws.com/my-flask-app:latest
        ports:
        - containerPort: 80
```
Save it and run ```kubectl apply -f deployment.yaml```
## Step 11: Test The Application
Once the deployment is successful, expose the Flask app via a Kubernetes service:
- Create a Service YAML file (service.yaml): ``` kubectl apply -f service.yaml ```
```
apiVersion: v1
kind: Service
metadata:
  name: flask-app-service
spec:
  selector:
    app: flask-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```
Apply the Service:
### Access the application: Use the external IP provided by the LoadBalancer to access the Flask app in the browser.
## Step 12: Cleanup
After testing your application, you can clean up the resources to avoid unnecessary charges:
Delete the EKS cluster:
Delete the ECR repository (if no longer needed):
## Conclusion
This guide covers the steps to build a simple Flask web application, containerize it with Docker, deploy it to AWS using Amazon ECR and EKS, and test the deployment. By following the steps above, you can learn how to integrate Docker and Kubernetes with AWS for scalable web application deployments.


