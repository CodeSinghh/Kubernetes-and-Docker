### Step 1: Login and Basics Setup

1. **Login to AWS Account as Root User**:
   - Open your web browser and navigate to the [AWS Management Console](https://aws.amazon.com/console/).
   - Enter your credentials to log in to your AWS account as the root user.

2. **Launch an EC2 Instance**:
   - Navigate to the [EC2 Dashboard](https://console.aws.amazon.com/ec2/).
   - Click on "Launch Instance" to begin the instance creation process.
   - Choose an Amazon Machine Image (AMI), e.g., Ubuntu.
   - Select an appropriate instance type, e.g., t2.micro.
   - Configure the instance settings, including network, subnet, security group, and key pair.
   - Make sure to allow HTTPS and HTTP traffic and select or create a key pair for SSH access.

3. **Connect to the EC2 Instance**:
   - After launching the EC2 instance, click on "Connect" at the top right corner of the EC2 Dashboard.
   - Follow the instructions provided to connect to your EC2 instance using SSH.
   - Use the downloaded key pair (.pem file) to authenticate the SSH connection.

4. **Execute Commands on EC2 Instance**:
   - Once connected to the EC2 instance via SSH, execute the following commands:
     - Switch to root user: `sudo su`
     - Update package repositories: `apt update -y`
### Step-by-Step Guide to Setup Docker, Terraform, AWS CLI, and kubectl

Here's the step-by-step guide to set up Docker, Terraform, AWS CLI, and kubectl on your system:

### Setup Docker

1. **Install Docker**:
   - Run the following command to install Docker:
     ```
     sudo apt install docker.io
     ```

2. **Add User to Docker Group**:
   - Add your username to the Docker group to allow non-root users to run Docker commands:
     ```
     sudo usermod -aG docker $USER
     ```

3. **Apply Group Changes**:
   - Apply the changes to the current session:
     ```
     newgrp docker
     ```

### Setup Terraform

1. **Install Dependencies**:
   - Install wget to download Terraform:
     ```
     sudo apt install wget -y
     ```

2. **Download and Install Terraform**:
   - Download and install the HashiCorp GPG key:
     ```
     wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
     ```
   - Add the HashiCorp official repository:
     ```
     echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
     ```
   - Update package lists and install Terraform:
     ```
     sudo apt update && sudo apt install terraform -y
     ```

### Setup AWS CLI

1. **Install AWS CLI**:
   - Download the AWS CLI installation package:
     ```
     curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
     ```
   - Install unzip:
     ```
     sudo apt-get install unzip -y
     ```
   - Unzip the installation package:
     ```
     unzip awscliv2.zip
     ```
   - Run the installation script:
     ```
     sudo ./aws/install
     ```

### Setup kubectl

1. **Install kubectl**:
   - Install curl if not already installed:
     ```
     sudo apt install curl -y
     ```
   - Download the kubectl binary:
     ```
     curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
     ```
   - Move the binary to the appropriate directory and set permissions:
     ```
     sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
     ```

By following these steps, you'll have Docker, Terraform, AWS CLI, and kubectl set up on your system, ready to use for your development and deployment needs.

### Step 4: Attach IAM Role to Your EC2 Instance

1. **Go to EC2 Section**:
   - Navigate to the EC2 section in the [AWS Management Console](https://console.aws.amazon.com/ec2/).

2. **Modify IAM Role**:
   - Select the EC2 instance to which you want to attach the IAM role.
   - Click on "Actions" and then select "Security" from the dropdown menu.
   - Choose the "Modify IAM role" option.

3. **Choose Role**:
   - In the Modify IAM role window, choose the IAM role from the dropdown menu that you want to attach to the EC2 instance.

4. **Update IAM Role**:
   - After selecting the IAM role, click on the "Update IAM role" button to attach the role to the EC2 instance.

### Why We Need IAM Role

IAM (Identity and Access Management) roles are crucial in managing access to AWS resources securely. Here's why we need IAM roles:

1. **Granular Access Control**:
   - IAM roles allow you to define fine-grained permissions for accessing AWS resources. You can specify what actions a user, service, or resource can perform on AWS services.

2. **Least Privilege Principle**:
   - IAM roles adhere to the principle of least privilege, which means granting only the permissions necessary to perform specific tasks. This minimizes the risk of unauthorized access or misuse of resources.

3. **Temporary Credentials**:
   - IAM roles can provide temporary security credentials that are automatically rotated and managed by AWS. These credentials are short-lived and reduce the risk of credential compromise.

4. **Seamless Integration**:
   - IAM roles seamlessly integrate with various AWS services, allowing EC2 instances, Lambda functions, and other resources to access AWS services securely without the need for long-term access keys.

5. **Secure Access to AWS Services**:
   - By attaching an IAM role to an EC2 instance, you ensure that the instance has the necessary permissions to access other AWS services securely, such as S3, DynamoDB, or RDS, based on the defined policies.

By leveraging IAM roles, you can effectively manage access to your AWS resources while maintaining security and compliance standards within your organization.

### Step 5: Building Infrastructure Using Terraform

1. **Clone GitHub Repository**:
   - Create a directory and navigate into it:
     ```
     mkdir webapp
     cd webapp
     ```
   - Clone the GitHub repository:
     ```
     git clone https://github.com/Aakibgithuber/Deployment-of-super-Mario-on-Kubernetes-using-terraform.git
     cd Deployment-of-super-Mario-on-Kubernetes-using-terraform/EKS-TF
     ```

2. **Edit Backend Configuration**:
   - Edit the `backend.tf` file using a text editor like Vim:
     ```
     vim backend.tf
     ```
   - Make sure to provide your bucket and region name in the `backend.tf` file to configure the Terraform backend. This enables Terraform to store state files in an S3 bucket, ensuring state consistency and collaboration among team members.

3. **Initialize Terraform**:
   - Run the following command to initialize Terraform:
     ```
     terraform init
     ```

4. **Validate Terraform Configuration**:
   - Validate the Terraform configuration to catch any syntax errors or mistakes:
     ```
     terraform validate
     ```

5. **Plan Infrastructure Changes**:
   - Generate and review an execution plan to see what changes will be made to the infrastructure:
     ```
     terraform plan
     ```

6. **Apply Infrastructure Changes**:
   - Apply the changes to create or modify the infrastructure:
     ```
     terraform apply --auto-approve
     ```
   - The `--auto-approve` flag skips the confirmation prompt, allowing Terraform to proceed with the changes automatically.

7. **Wait for Completion**:
   - Depending on the complexity of the infrastructure, the `terraform apply` command may take 5 to 10 minutes to complete. Allow it to finish execution.

8. **Update EKS Configuration**:
   - Update the configuration of the Amazon EKS cluster to configure `kubectl` to connect to it:
     ```
     aws eks update-kubeconfig --name EKS_CLOUD --region us-east-1
     ```
   - Replace `EKS_CLOUD` with the name of your EKS cluster and `us-east-1` with the appropriate AWS region where your cluster is deployed.

By following these steps, you'll be able to build your infrastructure using Terraform and configure it for deployment on Amazon EKS. Ensure that you provide the necessary configurations and permissions as mentioned in the notes to enable smooth execution of Terraform commands.

### Step 6: Creation of Deployment and Service for EKS

1. **Change Directory**:
   - Navigate back to the directory where deployment and service files are stored:
     ```
     cd ..
     ```

2. **Create Deployment**:
   - Apply the deployment configuration defined in `deployment.yaml` file:
     ```
     kubectl apply -f deployment.yaml
     ```

3. **Create Service**:
   - Apply the service configuration defined in `service.yaml` file:
     ```
     kubectl apply -f service.yaml
     ```

4. **View Resources**:
   - View all resources created in the Kubernetes cluster:
     ```
     kubectl get all
     ```

5. **Get Load Balancer Ingress**:
   - Retrieve the Load Balancer Ingress details for accessing the application:
     ```
     kubectl describe service mario-service
     ```

6. **Play and Enjoy**:
   - Copy the Load Balancer Ingress URL and paste it into a web browser to access and enjoy the game.

### Step 7: Destroy All Infrastructure

1. **Delete Deployment and Service**:
   - Delete the deployment and service to stop the application:
     ```
     kubectl delete service mario-service
     kubectl delete deployment mario-deployment
     ```

2. **Destroy EKS Cluster**:
   - Navigate to the directory containing the Terraform configuration for EKS:
     ```
     cd EKS-TF
     ```
   - Destroy the infrastructure using Terraform:
     ```
     terraform destroy --auto-approve
     ```

3. **Terminate EC2 Instance**:
   - After a few minutes, once the EKS cluster and associated resources are destroyed, go to the EC2 Dashboard.
   - Terminate the EC2 instance created earlier.

By following these steps, you'll be able to create and destroy the necessary resources for deploying your application on Amazon EKS efficiently. Ensure to terminate all resources to avoid incurring unnecessary charges on your AWS account.


