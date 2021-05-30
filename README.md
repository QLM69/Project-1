## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![](https://github.com/QLM69/Project-1/blob/9132f35a4f1ec51e1658911ba5322577876076c7/Untitled%20picture.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

This document contains the following details:
- Description of the Topologu
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
| Project1ELK-VM | Log Server | 10.2.0.4   | Linux            |
| Web-1          | Server     | 10.1.0.5   | Linux            |
| Web-2          | Server     | 10.1.0.6   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal IP Address

Machines within the network can only be accessed by _____.
- The ELK-Server is only accessible by SSH from the JumpBox and via web access from Personal IP Address.

A summary of the access policies in place can be found in the table below.

| Name           | Publicly Accessible | Allowed IP Addresses |
|----------------|---------------------|----------------------|
| Jump Box       | Yes/No              | 10.0.0.1 10.0.0.2    |
| Load Balancer  | Yes                 | Open                 |
| Web 1          | No                  | 10.1.0.5             |
| Web 2          | No                  | 10.1.0.6             |
| ELK Server     | Yes                 | Personal             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- The main advantage of automating the installation process is that we could deploy multiple servers easily and quickly without having to physically touch each server.

The playbook implements the following tasks:
- Install Docker.io and pip3
- Increases VM memory
- Download and Configure elk docker container
- Sets Published Ports

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Untitled picture.png](Images/docker_ps_output.png)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 : 10.1.0.5
- Web-2 : 10.1.0.6

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

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
