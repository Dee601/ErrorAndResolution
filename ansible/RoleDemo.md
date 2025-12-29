# Ansible Roles Demo using AWS EC2

This demo explains how to use **Ansible Roles** to install and run **Apache Web Server**
on an AWS EC2 instance using **best practices**.

---

## 1. Demo Objective

- Understand **Ansible Roles**
- Automate Apache installation on EC2
- Verify output using a browser
- Learn real-world Ansible workflow

---

## 2. Architecture

Browser → EC2 (Apache)  
Ansible Control Node → EC2 (via SSH, private IP)

---

## 3. Prerequisites

- AWS Account
- 2 EC2 instances in **same VPC**
  - 1 → Ansible Control Node
  - 1 → Managed Node (Web Server)
- Amazon Linux OS
- Security Group:
  - SSH (22) allowed
  - HTTP (80) allowed
- PEM key available on Ansible server

---

## 4. Setup Ansible Control Node

Login to control node and install Ansible:

```bash
sudo yum install ansible -y
ansible --version
```
5. Configure Default Inventory

Edit default inventory file:
```bash
sudo vi /etc/ansible/hosts
Add EC2 private IP (same VPC):

[web]
172.31.1.132 ansible_user=ec2-user ansible_ssh_private_key_file=/root/Demo.pem
```

👉 ansible_user = remote EC2 user
👉 .pem path = local Ansible server path

6. Test Connectivity

Run ping module:
```bash
ansible web -m ping


Expected output:

ping: pong
```
If this works → SSH + inventory is correct ✅

7. Create Ansible Role

Go to working directory:
```bash
cd ~


Create role structure:
```bash
ansible-galaxy init roles/apache

```
Role structure:

roles/apache/
├── tasks/main.yml
├── handlers/main.yml
├── vars/main.yml

8. Define Role Tasks

Edit tasks file:
```bash
vi roles/apache/tasks/main.yml

- name: Install Apache
  yum:
    name: httpd
    state: present
  notify: start apache
```
9. Define Handler

Edit handlers file:
```bash
vi roles/apache/handlers/main.yml

- name: start apache
  service:
    name: httpd
    state: started
```
10. Create Main Playbook

Create site.yml:
```bash
vi site.yml

- hosts: web
  become: yes
  roles:
    - apache

```
👉 become: yes is required for sudo/root tasks

11. Run the Playbook

Execute:
```bash
ansible-playbook site.yml
```

Expected result:

Apache installed

Apache service started

No failures

12. Verify in Browser

Open browser:

http://<EC2-PUBLIC-IP>


You should see:

It works!


🎉 Demo successful

13. Key Teaching Points

Role = reusable automation unit

Handlers run only when changes happen

Inventory defines target systems

become enables privilege escalation

Same role can be reused for 100s of servers

14. Common Issues & Fixes
SSH Permission Denied

Wrong PEM key

Wrong ansible_user

Apache Install Failed

Missing become: yes

Ping Works but Playbook Fails

Check sudo permissions on EC2
