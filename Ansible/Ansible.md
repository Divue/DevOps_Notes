# =====================================================================
# ANSIBLE — COMPLETE FUNDAMENTALS + HANDS-ON LAB (INTERVIEW READY)
# =====================================================================

# =====================================================================
# PART 1 — FUNDAMENTALS (CLEAR CONCEPTS)
# =====================================================================

-----------------------------------------------------------------------
1. WHY ANSIBLE EXISTS
-----------------------------------------------------------------------

Problem:
Managing 10 servers manually is possible.
Managing 100+ servers manually is not scalable.

Each server needs:
- OS updates
- Security patches
- Install packages (git, nginx, docker)
- Create users
- Start services
- Deploy apps

Manual SSH into each server:
- Slow
- Error-prone
- Not consistent

Solution:
Configuration Management

Definition:
Configuration Management = keeping systems in a defined desired state automatically.

Tools:
- Puppet
- Chef
- Ansible
- SaltStack

-----------------------------------------------------------------------
2. WHAT IS ANSIBLE
-----------------------------------------------------------------------

Ansible is:
- Configuration management tool
- Automation tool
- Orchestration tool

Main idea:
Control many servers from one machine.

You write instructions once.
Ansible runs them on all servers.

-----------------------------------------------------------------------
3. ARCHITECTURE
-----------------------------------------------------------------------

Two components:

1. Control Node
   - Machine where Ansible is installed.
   - Sends instructions.

2. Managed Nodes (Target Servers)
   - Servers being managed.
   - Linux → SSH
   - Windows → WinRM

Important:
Ansible is AGENTLESS.
No software required on target servers.

Execution flow:
Control Node → SSH → Run module → Return result → Disconnect

-----------------------------------------------------------------------
4. PUSH MODEL
-----------------------------------------------------------------------

Ansible uses PUSH model.

Meaning:
Control node pushes configuration to servers.

Contrast:
Puppet uses Pull model (agent pulls config).

-----------------------------------------------------------------------
5. CORE COMPONENTS
-----------------------------------------------------------------------

You must know these:

Inventory
Module
Task
Playbook
Role
Handler
Variables

-----------------------------------------------------------------------
6. INVENTORY
-----------------------------------------------------------------------

Inventory = list of servers.

Example:

[web]
10.0.0.10
10.0.0.11

[db]
10.0.0.20

Types:
- Static inventory (manual file)
- Dynamic inventory (AWS, Azure auto-detect)

Purpose:
Tells Ansible where to run tasks.

-----------------------------------------------------------------------
7. MODULE
-----------------------------------------------------------------------

Module = small program that performs one task.

Examples:
apt
yum
service
copy
file
user
shell
command

Modules are idempotent.

Idempotency:
Running task multiple times gives same result.

-----------------------------------------------------------------------
8. TASK
-----------------------------------------------------------------------

Task = execution of a module.

Example:

- name: Install nginx
  apt:
    name: nginx
    state: present

-----------------------------------------------------------------------
9. PLAYBOOK
-----------------------------------------------------------------------

Playbook = YAML file containing tasks.

Used when:
- Multiple steps
- Reusable automation
- Production deployment

Example:

- hosts: web
  become: yes
  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present

-----------------------------------------------------------------------
10. ROLE
-----------------------------------------------------------------------

Role = structured way to organize automation.

Structure:

roles/
  web/
    tasks/
    handlers/
    templates/
    vars/

Used for:
- Large projects
- Reusability
- Clean architecture

-----------------------------------------------------------------------
11. HANDLER
-----------------------------------------------------------------------

Handler runs only if change occurs.

Example:
If config changes → restart nginx.

-----------------------------------------------------------------------
12. ADHOC VS PLAYBOOK
-----------------------------------------------------------------------

Adhoc:
- One-time command
- Quick task
- Not reusable

Playbook:
- Multiple tasks
- Reusable
- Structured
- Production ready

-----------------------------------------------------------------------
13. IMPORTANT COMMANDS
-----------------------------------------------------------------------

Ping:
ansible all -m ping

Adhoc:
ansible web -m shell -a "uptime"

Run playbook:
ansible-playbook site.yml

Dry run:
--check

Verbose:
-vvv

-----------------------------------------------------------------------
14. ANSIBLE VS PUPPET
-----------------------------------------------------------------------

Ansible:
- Agentless
- Push model
- YAML
- Simple setup

Puppet:
- Agent required
- Pull model
- Master server required

-----------------------------------------------------------------------
15. INTERVIEW MUST-KNOW
-----------------------------------------------------------------------

What is Ansible?
Automation + configuration management tool.

What is inventory?
List of target servers.

What is module?
Unit of work.

What is playbook?
YAML automation file.

What is idempotency?
Same result on repeated runs.

Push vs Pull?
Ansible push, Puppet pull.

-----------------------------------------------------------------------
# =====================================================================
# PART 2 — HANDS-ON LAB (EC2 MAIN + TARGET)
# =====================================================================
# =====================================================================

Goal:
Control one EC2 (target) from another EC2 (main).

-----------------------------------------------------------------------
STEP 1 — Launch EC2 Instances
-----------------------------------------------------------------------

Create 2 instances in same VPC:

1. Main Server (Control Node)
2. Target Server (Managed Node)

Security Group:
Allow SSH (port 22)
Main → Target

-----------------------------------------------------------------------
STEP 2 — SSH into MAIN from Laptop
-----------------------------------------------------------------------

ssh -i your-key.pem ec2-user@<MAIN_PUBLIC_IP>

-----------------------------------------------------------------------
STEP 3 — Generate SSH Key on MAIN
-----------------------------------------------------------------------

ssh-keygen -t rsa -b 2048
Press Enter for defaults.

Files created:
~/.ssh/id_rsa      (private key)
~/.ssh/id_rsa.pub  (public key)

-----------------------------------------------------------------------
STEP 4 — Copy Public Key to TARGET
-----------------------------------------------------------------------

Use private IP if same VPC.

ssh-copy-id -i ~/.ssh/id_rsa.pub ec2-user@<TARGET_PRIVATE_IP>

If not available:

ssh ec2-user@<TARGET_PRIVATE_IP>
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
Paste id_rsa.pub content
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
exit

-----------------------------------------------------------------------
STEP 5 — Test Passwordless SSH
-----------------------------------------------------------------------

ssh ec2-user@<TARGET_PRIVATE_IP>

Should not ask password.

-----------------------------------------------------------------------
STEP 6 — Install Ansible on MAIN
-----------------------------------------------------------------------

Amazon Linux:
sudo amazon-linux-extras install ansible2 -y

Ubuntu:
sudo apt update
sudo apt install ansible -y

Verify:
ansible --version

-----------------------------------------------------------------------
STEP 7 — Create Project Folder
-----------------------------------------------------------------------

mkdir ansible-lab
cd ansible-lab

-----------------------------------------------------------------------
STEP 8 — Create Inventory File
-----------------------------------------------------------------------

nano hosts.ini

[target]
<TARGET_PRIVATE_IP> ansible_user=ec2-user ansible_ssh_private_key_file=~/.ssh/id_rsa

-----------------------------------------------------------------------
STEP 9 — Test Ansible Connectivity
-----------------------------------------------------------------------

ansible -i hosts.ini target -m ansible.builtin.ping

Expected:
SUCCESS => pong

-----------------------------------------------------------------------
STEP 10 — Run Adhoc Command
-----------------------------------------------------------------------

ansible -i hosts.ini target -m shell -a "touch adhoc_test.txt"

Verify:
ssh ec2-user@<TARGET_PRIVATE_IP> "ls"

-----------------------------------------------------------------------
STEP 11 — Create Playbook
-----------------------------------------------------------------------

nano create_file.yml

---
- name: Create file using Ansible
  hosts: target
  become: yes

  tasks:
    - name: Create hello file
      ansible.builtin.copy:
        content: "Hello from Ansible"
        dest: /home/ec2-user/hello_ansible.txt

-----------------------------------------------------------------------
STEP 12 — Run Playbook
-----------------------------------------------------------------------

ansible-playbook -i hosts.ini create_file.yml

-----------------------------------------------------------------------
STEP 13 — Verify
-----------------------------------------------------------------------

ssh ec2-user@<TARGET_PRIVATE_IP> "cat /home/ec2-user/hello_ansible.txt"

-----------------------------------------------------------------------
WHAT THIS LAB PROVES
-----------------------------------------------------------------------

You demonstrated:

- Control node vs managed node
- SSH key-based authentication
- Inventory creation
- Adhoc command usage
- Playbook creation
- Module execution
- Idempotent automation

-----------------------------------------------------------------------
END OF COMPLETE NOTES
-----------------------------------------------------------------------
```
