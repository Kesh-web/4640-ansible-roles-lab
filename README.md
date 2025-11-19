# 4640-ansible-roles-lab
Kesh Alfred Leeanne 

# ACIT 4640 – Lab Week 11  
## Terraform + Ansible Roles Deployment

This lab provisions two AWS EC2 instances (Ubuntu frontend and Rocky Linux backend) using Terraform and configures both using Ansible with a dynamic AWS EC2 inventory. All configuration has been refactored into roles, and handlers/templates/files are placed inside each role following Ansible best practices.

## Prerequisites

Install required packages for Ansible and the AWS dynamic inventory plugin:

sudo apt update
sudo apt install -y python3-pip

# Install AWS Python SDK dependencies for ansible aws_ec2 plugin
sudo pip3 install boto3 botocore

# Verify installation
python3 --version
pip3 --version


## Setup

### 1. Clone Starter Code
git clone https://gitlab.com/cit_4640/4640-ansible-roles-lab.git
cd 4640-ansible-roles-lab

### 2. Create AWS SSH Key
ssh-keygen -t ed25519 -f ~/.ssh/aws

After we ran the import key script.


## Deploying Infrastructure with Terraform

After installing prerequisites and creating your AWS SSH key, run Terraform to create the two EC2 servers.

### Initialize Terraform
cd terraform
terraform init

### Review the plan
terraform plan

### Apply and deploy the infrastructure
terraform apply -auto-approve

Terraform will provision:
- Ubuntu EC2 instance (frontend server)
- Rocky Linux EC2 instance (backend server)

The output will include the public IP addresses used by Ansible during configuration.


### Create Ansible Roles
The two required roles are generated using `ansible-galaxy init` so the directory structure follows Ansible best practices.

Run these commands inside the `ansible` directory:

```cd ansible``` and then run these commands 
--
ansible-galaxy init --init-path roles frontend_server
--
ansible-galaxy init --init-path roles redis_server
--
This creates:

ansible/roles/frontend_server/
ansible/roles/redis_server/

Each role created by Ansible Galaxy includes the following directories:

- defaults/
- files/
- handlers/
- meta/
- tasks/
- templates/
- tests/
- vars/


## Refactoring Ansible Configuration

The roles (`frontend_server` and `redis_server`) were already generated earlier using Ansible Galaxy.  
The next step was to refactor the original `plays.yml` into the proper role structure.

### Moving Existing Configuration Into Roles

All frontend-related files from the starter project were moved into the `frontend_server` role:

- files/default.conf → roles/frontend_server/files/default.conf
- templates/index.html.j2 → roles/frontend_server/templates/index.html.j2

All related nginx tasks were added to:

roles/frontend_server/tasks/main.yml

A handler was added to reload nginx when templates or configs change:

roles/frontend_server/handlers/main.yml

Backend-related logic from `plays.yml` was moved into:

roles/redis_server/tasks/main.yml

### New Playbook

A new `playbook.yml` replaced the old `plays.yml` and assigns roles to their AWS EC2 groups discovered by the dynamic inventory plugin:

- hosts: frontend
  roles:
    - frontend_server

- hosts: backend
  roles:
    - redis_server

The old `plays.yml` was removed after the refactor.

### Running Ansible

From inside the `ansible` directory:

ansible-playbook -i inventory/aws_ec2.yml playbook.yml



<img width="613" height="314" alt="server-img" src="https://github.com/user-attachments/assets/2f58bdb7-ec6b-4fda-9a67-186e21786ca6" />


