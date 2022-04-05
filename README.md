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
  - [Filebeat Config](https://github.com/Phokaia/Week-13-ELK-Stack-Project/blob/471b9e3a7798083779f240b9c00bd0d34a6420ba/Ansible/files/filebeat-config.yml)
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


| Name     | Function          |        IP Address      | Operating System |
|----------|-------------------|------------------------|------------------|
| Jump Box | Gateway           | 10.0.0.4/20.213.158.80 | Linux            |
| WEB-1    | Web Application   | 10.0.0.5/24.146.47.227 | Linux            |
| WEB-2    | Web Application   | 10.0.0.6/24.146.47.227 | Linux            |
|ELK-Server| Monitoring Server | 10.1.0.5/20.219.107.69 | Linux            |

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

- Specify a different group of machines:
      ```yaml
        - name: Config elk VM with Docker
          hosts: elk
          become: true
          tasks:
      ```
  - Install Docker.io
      ```yaml
        - name: Install docker.io
          apt:
            update_cache: yes
            force_apt_get: yes
            name: docker.io
            state: present
      ``` 
  - Install Python-pip
      ```yaml
        - name: Install python3-pip
          apt:
            force_apt_get: yes
            name: python3-pip
            state: present

          # Use pip module (It will default to pip3)
        - name: Install Docker module
          pip:
            name: docker
            state: present
            `docker`, which is the Docker Python pip module.
      ``` 
  - Increase Virtual Memory
      ```yaml
       - name: Use more memory
         sysctl:
           name: vm.max_map_count
           value: '262144'
           state: present
           reload: yes
      ```
  - Download and Launch ELK Docker Container (image sebp/elk)
      ```yaml
       - name: Download and launch a docker elk container
         docker_container:
           name: elk
           image: sebp/elk:761
           state: started
           restart_policy: always
      ```
  - Published ports 5044, 5601 and 9200 were made available
      ```yaml
           published_ports:
             -  5601:5601
             -  9200:9200
             -  5044:5044   
      ```
       
-__Enable service docker on boot__
-name: Enable service docker on boot
    systemd:
      name: docker
      enabled: yes

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

ELK-Server
----------
![image](https://github.com/Phokaia/Week-13-ELK-Stack-Project/blob/23a3d46a0a9896c8871ec06135e1c02b8622ec87/Diagrams/ELK-Server.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring

- Elk-Server: 10.1.0.5
- WEB-1     : 10.0.0.5
- WEB-2     : 10.0.0.6

We have installed the following Beats on these machines:
- Specify which Beats you successfully installed


- __Filebeat__

![image](https://github.com/Phokaia/Week-13-ELK-Stack-Project/blob/1df1df0f487bd806ea483f1701aa71b15a7a58c6/Diagrams/elk.png)


- __Metricbeat__

![image](https://github.com/Phokaia/Week-13-ELK-Stack-Project/blob/1df1df0f487bd806ea483f1701aa71b15a7a58c6/Diagrams/metricbeat%20system.png)

![image](https://github.com/Phokaia/Week-13-ELK-Stack-Project/blob/1df1df0f487bd806ea483f1701aa71b15a7a58c6/Diagrams/metricbeat.png)

These Beats allow us to collect the following information from each machine:

- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc.

__These can be log files (Filebeat), network data (Packetbeat), server metrics (Metricbeat), or any other type of data that can be collected by the growing number of Beats being developed by both Elastic and the community. For example Filebeat can be installed on almost any operating system, including as a Docker container, and also comes with internal modules for specific platforms such as Apache, MySQL, Docker, MariaDB, Percona, Kafka and more.__

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the __YML__ file to __ANSIBLE FOLDER__.
- Update the __CONFIG__ file to include __REMOTE USERS AND PORTS__
- Run the playbook, and navigate to __HTTP://(ELK-Server Public Address)/app/kibana__ to check that the installation worked as expected.

  _Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_

__Install-elk.yml is the playbook. I copy it in /Week-13-ELK-Stack-Project/Ansible/ELK-Stack/__

- _Which file do you update to make Ansible run the playbook on a specific machine?

__/etc/ansible/hosts file__

- _How do I specify which machine to install the ELK server on versus which to install Filebeat on?_

__I have specified two separate groups in the etc/ansible/hosts file. One of the group will be webservers which has the IPs of the 2 Virtual machines that I will gonna install to Filebeat. The other group is named to ELK-Server which will have the Dynamic IP.I will also install ELK to Filebeat.__

- _Which URL do you navigate to in order to check that the ELK server is running?

 - http://Elk-Servers Public IP:5601/app/kibana  (http://52.172.29.17:5601/app/kibana)__

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
