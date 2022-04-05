# Week-13-Homework-GitHub Fundamentals and Project 13 Submission
Project ELK-Stack Submission
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![image](https://user-images.githubusercontent.com/95952098/161176378-a13df2ab-ae1a-43e3-8dd6-2e3edfe30f4f.png)

__Security Rules for our network__

__1.__	Deny All Inbound. (Source=Any , Destination=Any)

__2.__	Allow Azure Load Balancer Inbound. (Source=Azure Load Balancer, Destination=Any)

__3.__	Allow Vnet Inbound. (Source=Virtual Network, Destination=Virtual Network)

__4.__	Allow SSH from my IP (Source=24.146.47.227, Destination=10.0.0.4)

__5.__	Allow SSH InBound To JumpBoxVN (Source=10.0.0.4, Destination=Virtual Network)

__6.__	Allow Port 80 Inbound Public VirtualNet (Source=24.146.47.227,Destination= Virtual Network)

__7.__	ELK IP Restrict (Source=24.146.47.227, Destination=10.1.0.5)

__8.__	Allow SSH over Port 5601 to access KIBANA  from ELK Server


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of __YML AND CONFIG__ file may be used to install only certain pieces of it, such as Filebeat.

  - [My Penetration Tester](https://github.com/Phokaia/Week-13-ELK-Stack-Project/blob/7c75b0b88fefbdd5763553222341a914d976d5e9/Ansible/pentest.yml)
  - [Hosts](https://github.com/Phokaia/Week-13-ELK-Stack-Project/blob/0b2d3415644d9c7e4a4918882aa519612459a59b/Ansible/ELK-Stack/hosts)
  - [Ansible Config](https://github.com/Phokaia/Week-13-ELK-Stack-Project/blob/50e3b99ba20ef4897230a94de6993b49968d6eb3/Ansible/ELK-Stack/ansible.cfg)
  - [ELK Installation](https://github.com/Phokaia/Week-13-ELK-Stack-Project/blob/50e3b99ba20ef4897230a94de6993b49968d6eb3/Ansible/ELK-Stack/install-elk.yml)
  - [Filebeat Config]
  - [Filebeat Playbook](https://github.com/Phokaia/Week-13-ELK-Stack-Project/blob/50e3b99ba20ef4897230a94de6993b49968d6eb3/Ansible/roles/filebeat-playbook.yml)
  - [Metricbeat Config](https://github.com/Phokaia/Week-13-ELK-Stack-Project/blob/50e3b99ba20ef4897230a94de6993b49968d6eb3/Ansible/files/metricbeat-config.yml)
  - [Metricbeat Playbook](https://github.com/Phokaia/Week-13-ELK-Stack-Project/blob/50e3b99ba20ef4897230a94de6993b49968d6eb3/Ansible/roles/metricbeat-playbook.yml)




 This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly __functional and available__, in addition to restricting __traffic__ to the network.
- What aspect of security do load balancers protect? What is the advantage of a jump box?
__Load balancers add resiliency by rerouting live traffic from one server to another server.
  If the server falls prey to a DDoS attack or otherwise becomes unavailable.__

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the __network__ and system __logs__.

- What does Filebeat watch for?
__Filebeat monitors the log files or locations that you specify, collects log events,
  and forwards them either to Elasticsearch or Logstash for indexing.__

- What does Metricbeat record?
__Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify,
  such as Elasticsearch or Logstash.__

The configuration details of each machine may be found below.


| Name     | Function     |        IP Address      | Operating System |
|----------|--------------|------------------------|------------------|
| Jump Box | Gateway      | 10.0.0.4/20.213.158.80 | Linux            |
| WEB-1    | Linux Server | 10.0.0.5/24.146.47.227 | Linux            |
| WEB-2    | Linux Server | 10.0.0.6/24.146.47.227 | Linux            |
|ELK-Server| Linux server | 10.1.0.5/20.219.107.69 | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the __Jump-Box__ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Add whitelisted IP addresses
__Workstation my public IP through TCP 5601.

Machines within the network can only be accessed by __Workstation and Jump-Box through SSH Jump-Box.
- Which machine did you allow to access your ELK VM? What was its IP address?
__I did allow to Jump-Box access my ELK VM via SSH port 22 and IP address was 20.213.158.80. It was static IP.__ 

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses                      |
|----------|---------------------|-------------------------------------------|
| Jump-Box | Yes                 | 20.213.158.80 on SSH 22                   |
| WEB-1    | No                  | 10.1.0.5 on SSH 22                        |
| WEB-2    | No                  | 10.1.0.5 on SSH 22                        |
|ELK-Server| No                  | Workstation my public IP through TCP 5601 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?

__There are multiple advantages, Ansible lets you quickly and easily deploy multi layer applications throug a .YAML playbook.__
__You don't need to write custom code to automate your systems.__
__Ansible will also figure out how to get your systems to the state you want them to be in.__

The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- __Specify a different group of machines__
-name: Config Web VM with Docker
  hosts: elk
  become: true
  tasks:
- __Install Docker.io__
-name: docker.io
    apt:
      force_apt_get: yes
      update_cache: yes
      name: docker.io
      state: present
-__Install Python-pip__
-name: Install pip3
    apt:
      force_apt_get: yes
      name: python3-pip
      state: present

    #Use pip module
  - name: Install Docker python module
    pip:
      name: docker
      state: present
-__Increase Virtual Memory__
-name: Increase virtual memory
    command: sysctl -w vm.max_map_count=262144

    #Use sysctl module
  - name: Use more memory
    ansible.posix.sysctl:
      name: vm.max_map_count
      value: 262144
      state: present
      reload: yes
-__Download and Launch ELK Docker Container (image sebp/elk)__
-name: download and launch a docker elk container
    docker_container:
      name: elk
      image: sebp/elk:761
      state: started
      restart_policy: always
-__Published ports 5044, 5601 and 9200 were made available__
-#Please list the ports that ELK runs on
      published_ports:
       -  5601:5601
       -  9200:9200
       -  5044:5044
-__Enable service docker on boot__
-name: Enable service docker on boot
    systemd:
      name: docker
      enabled: yes

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
