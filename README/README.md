## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![NetworkDiagram](Images/Project1Network.PNG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.
- [install_elk.yml](Playbooks/install_elk.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly **avaliable**, in addition to restricting **inbound access** to the network.
- **Load balancer** allows for high avalibility, protecting agaubst attack such as denial-of-service (DDoS) attack
- **Jumpbox** allows for security to be managed outside of managed devices, allowing to harden the system. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the **files on the machine** and monitor **system metric**.
- **Filebeat**  - collects data about the file system
- **Metricbeat** - collects machine metrics, such as uptime

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name                 | Function   | IP Address | Operating System |
|----------------------|------------|------------|------------------|
| Jump-Box-Provisioner | Gateway    | 10.0.0.4   | Linux            |
| Web-1                | Web Server | 10.0.0.7   | Linux            |
| Web-2                | Web Server | 10.0.0.8   | Linux            |
| ELK-Server           | Monitoring | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the **jumpbox** machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address: **70.123.40.65 (my local machine)**

Machines within the network can only be accessed **each other**.
- Jumpbox to connect to ELK VM from IP address: **10.0.0.4**

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses |
|----------------------|---------------------|----------------------|
| Jump-Box-Provisioner | Yes                 | 70.123.40.65         |
| Web-1                | No                  | Virtual Network      |
| Web-2                | No                  | Virtual Network      |
| ELK-Server           | No                  | Virtual Network      |
"""
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because **allows for accelarated automation**
- Ansible is also free

The playbook implements the following tasks:
- Install docker.io
- Install pip3
- Install Docker python module
- Increase virtual memory
- Download and launch a docker

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![dockerps](Images/dockerps.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1: 10.0.0.7  
- Web 2: 10.0.0.8 

We have installed the following Beats on these machines:
- Filebeats
- Metricbeats

These Beats allow us to collect the following information from each machine:
- **Filebeat**  - collects data about the file system
- **Metricbeat** - collects machine metrics, such as uptime

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the **playbook** file to **Ansible Control Node**.
-  Playbooks for Filebeat and Mtricbeat are also here: [filebeat-playbook.yml](Playbooks/filebeat-playbook.yml) & [metricbeat-playbook.yml](Playbooks/metricbeat-playbook.yml)
```
$ cd /etc/ansible
$ mkdir files
# Clone Repository + IaC Files
$ git clone https://github.com/TenkiYamada/Project-1-ELK-Stack-Project.git
# Move Playbooks and hosts file Into `/etc/ansible`
$ cp /Project-1-ELK-Stack-Project/ReadMe/Playbooks/*
```
- Update the **hosts** file to include **webserver** and **elk**
- Edit **hosts** file do you update to make Ansible run the playbook on a specific machine, and specify which machine to install the ELK server on versus which to install Filebeat.
```
$ cd /etc/ansible
$ cat > hosts <<EOF
[webservers]
10.0.0.7
10.0.0.8

[elk]
10.1.0.4
EOF
```
- Run the playbook, and navigate to **Kibana (http://[Host IP]/app/kibana#/home)** to check that the installation worked as expected.
```
 $ cd /etc/ansible
 $ ansible-playbook install_elk.yml elk
 $ ansible-playbook install_filebeat.yml webservers
 $ ansible-playbook install_metricbeat.yml webservers
 ```
 - Check that the ELK server is running: **http://[Host IP]/app/kibana#/home**
