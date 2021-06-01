## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![](https://github.com/QLM69/Project-1/blob/37beaf17ffca6b85148c184483ab4a8a95facc53/Diagrams/N.BRAZIL%20PROJECT1.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook YAML file may be used to install only certain pieces of it, such as Filebeat.

PLAYBOOK NO.1 PENTEST.YML
```
---
- name: Config Web VM with Docker
hosts: webservers
become: true
tasks:

  - name: Install docker.io
    apt:
      update_cache: yes
      name: docker.io
      state: present

  - name: Install pip3
    apt:
      name: python3-pip
      state: present

  - name: Install python Docker Module
    pip:
      name: docker
      state: present

  - name: Download and launch a docker web container
    docker_container:
      name: dvwa
      image: cyberxsecurity/dvwa
      state: started
      restart_policy: always
      published_ports: 80:80

  - name: Enable Docker Service
    systemd:
      name: docker
      enabled: yes
```      
      

PLAYBOOK NO.2 INSTALL-ELK.YML
```
---
- name: Configure Elk VM with Docker
hosts: elk
remote_user: azadmin
become: true
tasks:

  - name: Install docker.io
    apt:
      update_cache: yes
      force_apt_get: yes
      name: docker.io
      state: present

  - name: Install pip3
    apt:
      force_apt_get: yes
      name: python3-pip
      state: present

  - name: Install Docker module
    pip:
      name: docker
      state: present

  - name: Increase virtual memory
    command: sysctl -w vm.max_map_count=262144

  - name: Use more memory
    sysctl:
      name: vm.max_map_count
      value: '262144'
      state: present
      reload: yes

  - name: download and launch a docker elk container
    docker_container:
      name: elk
      image: sebp/elk:761
      state: started
      restart_policy: always
      published_ports:
        -  5601:5601
        -  9200:9200
        -  5044:5044
```

PLAYBOOK NO.3 FILEBEAT-PLAYBOOK.YML
```
---
- name: installing and launching filebeat
hosts: webservers
become: yes
tasks:

- name: download filebeat deb
  command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

- name: install filebeat deb
  command: dpkg -i filebeat-7.6.1-amd64.deb

- name: drop in filebeat.yml
  copy:
    src: /etc/ansible/filebeat-config.yml
    dest: /etc/filebeat/filebeat.yml

- name: enable and configure system module
  command: filebeat modules enable system

- name: setup filebeat
  command: filebeat setup

- name: start filebeat service
  command: service filebeat start

- name: enable service filebeat on boot
  systemd:
    name: filebeat
    enabled: yes
```

PLAYBOOK NO.4 METRICBEAT-PLAYBOOK.YML
```
- name: Install metric beat
hosts: webservers
become: true
tasks:

- name: Download metricbeat
  command: curl -L -O curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb

- name: install metricbeat
  command: dpkg -i metricbeat-7.6.1-amd64.deb

- name: drop in metricbeat config
  copy:
    src: /etc/ansible/metricbeat-config.yml
    dest: /etc/metricbeat/metricbeat.yml

- name: enable and configure docker module for metric beat
  command: metricbeat modules enable docker

- name: setup metric beat
  command: metricbeat setup

- name: start metric beat
  command: sudo service metricbeat start

- name: enable service metricbeat on boot
  systemd:
    name: metricbeat
    enabled: yes
```
This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network. Load Balancing ensures availability to the web-servers which is the availability aspect of security in regards to the CIA Triad.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- Filebeat watches for log files/locations and collect log events. (Filebeat: Lightweight Log Analysis & Elasticsearch)
- Metricbeat records metrics and statistical data from the operating system and from services running on the server (Metricbeat: Lightweight Shipper for Metrics)

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name           | Function   | IP Address | Operating System |
|----------------|------------|------------|------------------|
| Jump Box       | Gateway    | 10.1.0.7   | Linux            |
| Project1ELK-VM | ELK        | 10.2.0.4   | Linux            |
| Web-1          | DVWA       | 10.1.0.5   | Linux            |
| Web-2          | DVWA       | 10.1.0.6   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal IP Address

Machines within the network can only be accessed by the Jump Box within the Ansible Container.
- The ELK-Server is only accessible by SSH from the JumpBox and via web access from Personal IP Address.

A summary of the access policies in place can be found in the table below.

| Name           | Publicly Accessible  | Allowed IP Addresses  |
|----------------|----------------------|-----------------------|
| Jump Box       | Yes                  | Personal Home Network IP|
| Load Balancer  | Yes                  | Open                  |
| Web 1          | No                   | 10.1.0.5              |
| Web 2          | No                   | 10.1.0.6              |
| ELK Server     | No (but has PublicIP)| Personal              |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- The main advantage of automating the installation process is that we could deploy multiple servers easily and quickly without having to physically touch each server.

The playbook implements the following tasks:
- Install Docker.io and pip3
- Increases VM memory
- Download and Configure elk docker container
- Sets Published Ports

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![](https://github.com/QLM69/Project-1/blob/37beaf17ffca6b85148c184483ab4a8a95facc53/Diagrams/Docker%20ps.png)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 : 10.1.0.5
- Web-2 : 10.1.0.6

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
Filebeat watches for log files/locations and collect log events. (Filebeat: Lightweight Log Analysis & Elasticsearch)
Metricbeat records metrics and statistical data from the operating system and from services running on the server (Metricbeat: Lightweight Shipper for Metrics)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the YAML file to Ansible container.
- Update the Ansible hosts /etc/ansible/hosts file to include the following:
```
[webservers]
10.1.0.5 ansible_python_interpreter=/usr/bin/python3
10.1.0.6 ansible_python_interpreter=/usr/bin/python3

[elk]
10.2.0.4 ansible_python_interpreter=/usr/bin/python3
```
```
-Update the Ansible file /etc/ansible.cfg and set the current to the admin user of the web servers.
-SSH into the Jump Box by running this command ssh (admin_user)@(Jump Box Public IP)
-Start Ansible container by running sudo docker start (ansible-container-name)
-Run the playbook by running ansible-playbook (name-of-playbook).yml and navigate to ELK-Server-PublicIP:5601/app/kibana to check that the installation worked as expected by checking the Filebeat and Metricbeat
```
_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

Commands needed to run the Anisble Configuration for the Elk-Server are:
```
1.ssh (admin_user)@(Jump Box Public IP)
2.sudo docker container list -a - Locate the ansible container
3.sudo docker start gallant_ptolemy(name)
4.sudo docker attach gallant_ptolemy(name)
5.cd /etc/ansible
6.ansible-playbook elk-playbook.yml (Installs and Configures ELK-Server)
7.cd /etc/ansible/
8.ansible-playbook (file/metric)beat-playbook.yml (Installs and Configures Filebeat/Metricbeat)
9.Open a new browser on Personal Workstation, navigate to (http://{elk-server-ip}:5601/app/kibana)
```
References
Filebeat: Lightweight Log Analysis & Elasticsearch. (n.d.). Retrieved August 22, 2020, from https://www.elastic.co/beats/filebeat Metricbeat: Lightweight Shipper for Metrics. (n.d.). Retrieved August 22, 2020, from https://www.elastic.co/beats/metricbeat
