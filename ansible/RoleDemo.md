Ansible Roles Demo using AWS EC2

This demo explains how to use Ansible Roles to install and run Apache Web Server on an AWS EC2 instance using best practices.

1. Demo Objective

Understand Ansible Roles

Automate Apache installation on EC2

Verify output using a browser

Learn real-world Ansible workflow

2. Architecture

Browser → EC2 (Apache Web Server)

Ansible Control Node → EC2 (SSH via private IP)

3. Prerequisites

AWS Account

2 EC2 instances in the same VPC

Ansible Control Node

Managed Node (Web Server)

Amazon Linux OS

Security Group rules:

SSH (22) allowed

HTTP (80) allowed

PEM key available on Ansible server

4. Setup Ansible Control Node

Install Ansible on control node:

sudo yum install ansible -y
ansible --version

5. Configure Default Inventory

Edit default inventory file:

sudo vi /etc/ansible/hosts


Add EC2 private IP:

[web]
172.31.1.132 ansible_user=ec2-user ansible_ssh_private_key_file=/root/Demo.pem


Explanation:

ansible_user → remote EC2 login user

.pem path → key stored on Ansible server

6. Test Connectivity
ansible web -m ping


Expected output:

ping: pong


This confirms SSH and inventory are correct.

7. Create Ansible Role
cd ~
ansible-galaxy init roles/apache


Role structure created:

roles/apache/
├── tasks/main.yml
├── handlers/main.yml
├── vars/main.yml

8. Define Role Tasks

Edit tasks file:

vi roles/apache/tasks/main.yml

- name: Install Apache
  yum:
    name: httpd
    state: present
  notify: start apache

9. Define Handler

Edit handlers file:

vi roles/apache/handlers/main.yml

- name: start apache
  service:
    name: httpd
    state: started

10. Create Main Playbook

Create site.yml:

vi site.yml

- hosts: web
  become: yes
  roles:
    - apache


become: yes allows Ansible to run tasks with sudo.

11. Run the Playbook
ansible-playbook site.yml


Expected result:

Apache installed

Apache service started

No failures

12. Verify in Browser

Open in browser:

http://<EC2-PUBLIC-IP>


You should see:

It works!


🎉 Demo successful

13. Key Teaching Points

Roles make automation reusable

Handlers run only when changes occur

Inventory defines target systems

become enables privilege escalation

Same role works for many servers

14. Common Issues & Fixes

SSH Permission Denied

Wrong PEM key

Wrong ansible_user

Apache Install Failed

Missing become: yes

Ping Works but Playbook Fails

Check sudo permissions on EC2
