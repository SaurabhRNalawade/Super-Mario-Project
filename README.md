# ${\color {blue} \textbf {Super-Mario-Project}}$

**Project Workflow**
 
**Step 1 - Login and basics setup create the EC2 instance and select the instance type "t2.mediam" and increase the volume size upto 30 GB.**

**Step 2 - Then go to EC2 IAM and create the role with AWSFMAdminFullAccess and EKS cluster permissions and attach the IAM role.**

**Step 3 - Connect the instance and Setup Docker ,Terraform ,aws cli , and Kubectl.**

**Step 4 - Building Infrastructure Using terraform.**

**Step 5 - Creation of deployment and service for EKS.**

### Step 1 → Setup Tools
````
sudo apt update -y
````
Setup Docker:
````
sudo apt install docker.io -y
sudo systemctl start docker
sudo usermod -aG docker ubuntu
newgrp docker
docker --version
````
Setup Terraform:
````
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common

wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null

gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint

echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list
````
````
sudo apt update
sudo apt-get install terraform
terraform --version
````
Setup AWS CLI:
````
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip 
unzip awscliv2.zip
sudo ./aws/install
aws --version
````
Install kubectl:
- Download the latest release with the command:
````
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
````
- Validate the binary
````
 curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
````
- Validate the kubectl binary against the checksum file:
````
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
````
Install kubectl:
````
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
````
- Note: If you do not have root access on the target system, you can still install kubectl to the ~/.local/bin directory:
````
chmod +x kubectl
mkdir -p ~/.local/bin
mv ./kubectl ~/.local/bin/kubectl
````
````
kubectl version --client
````

## Step 2 - IAM Role for EC2
create role: role
![image](https://github.com/user-attachments/assets/ec593499-0caa-49c3-b209-fd062cefae25)

## Step 3 - Attach IAM role with your EC2 
go to EC2 click on actions → security → modify IAM role option

administrator access
eks
![image](https://github.com/user-attachments/assets/dc9b88e5-1c6b-45fb-b12a-a6e6abc44665)
![image](https://github.com/user-attachments/assets/4e63948b-5ddb-425c-b198-ad3c8ad38f4e)

## Step 4 - Building Infrastructure Using terraform
Install GIT:
````
sudo apt install git -y
git clone https://github.com/abhipraydhoble/Project-Super-Mario.git
cd Project-Super-Mario
cd EKS-TF
vim backend.tf
````
![image](https://github.com/user-attachments/assets/39f2f2a7-fa59-4eb1-a204-5391062f3e01)

Create Infra:
````
terraform init
terraform plan
terraform apply --auto-approve
````
````
aws eks update-kubeconfig --name EKS_CLOUD --region ap-southeast-1 --profile eks
````
## Step 5 - Creation of deployment and service for EKS
change the directory where deployment and service files are stored use the command →
````
cd ..
````
create the deployment
````
kubectl apply -f deployment.yaml
````
Now create the service
````
kubectl apply -f service.yaml
kubectl get all
kubectl get svc mario-service
````
copy the load balancer ingress and paste it on browser and your game is running

![image](https://github.com/user-attachments/assets/3e6584f7-9b16-45fa-9301-7abfd232892e)

Final Output: Enjoy The Game 🎮

![image](https://github.com/user-attachments/assets/6712966d-dbbc-460c-802b-9bdaca458db6)

Delete Infra
````
 terraform destroy -auto-approve
````
