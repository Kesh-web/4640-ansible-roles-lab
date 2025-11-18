# 4640-ansible-roles-lab
Kesh Alfred Leeanne 

# ACIT 4640 – Lab Week 11  
## Terraform + Ansible Roles Deployment

This lab provisions two AWS EC2 instances (Ubuntu frontend and Rocky Linux backend) using Terraform and configures both using Ansible with a dynamic AWS EC2 inventory. All configuration has been refactored into roles, and handlers/templates/files are placed inside each role following Ansible best practices.

## Setup

### 1. Clone Starter Code
git clone https://gitlab.com/cit_4640/4640-ansible-roles-lab.git
cd 4640-ansible-roles-lab

### 2. Create AWS SSH Key
ssh-keygen -t ed25519 -f ~/.ssh/aws

### 3. Add SSH Key to AWS (using provided script)
./scripts/add-aws-key.sh ~/.ssh/aws.pub

## Terraform Deployment

### 1. Initialize Terraform
cd terraform
terraform init

### 2. Apply Infrastructure
terraform plan
terraform apply -auto-approve

Terraform will create:
- Ubuntu EC2 instance (frontend)
- Rocky Linux EC2 instance (backend)

The outputs include the public IPs used by Ansible.

## Ansible Structure

ansible/
- ansible.cfg  
- playbook.yml (refactored from plays.yml)  
- inventory/aws_ec2.yml  
- roles/  
  - frontend/ (nginx + HTML deployment)  
  - backend/ (Rocky Linux configuration)  

Each role contains:
- tasks/  
- handlers/ (reload nginx when templates change)  
- templates/  
- files/  
- vars/  

## Running Ansible

### Test Dynamic AWS Inventory
ansible-inventory -i inventory/aws_ec2.yml --graph

### Run the Playbook
ansible-playbook -i inventory/aws_ec2.yml playbook.yml

## Deliverables Included

- README.md  
- server-img.jpg (screenshot of HTML served by Ubuntu frontend)  
- .gitignore  
- terraform/main.tf  
- terraform/provider.tf  
- terraform/modules/web-server/{main.tf, outputs.tf, variables.tf}  
- ansible/ansible.cfg  
- ansible/playbook.yml  
- ansible/inventory/aws_ec2.yml  
- ansible/roles/frontend  
- ansible/roles/backend  

## Completion Summary

- plays.yml refactored into playbook.yml  
- Two Ansible roles created (frontend, backend)  
- Handlers implemented  
- Files/templates moved into roles  
- Terraform provisions two EC2 servers  
- Dynamic AWS EC2 inventory used  
- README contains all commands  
- Screenshot added  

Done.
