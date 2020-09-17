# ELK-Container-Setup

This repository contains the necessary documents to install an ELK Container/Server

# Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/williamrcolwell/ELK-Container-Setup/blob/master/Diagrams/Week%2013%20Network%20Diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yaml file may be used to install only certain pieces of it, such as Filebeat.  These are all found in the /ansible subfolder of this directory.

install-elk.yml (https://github.com/williamrcolwell/ELK-Container-Setup/blob/master/Ansible/install-elk.yml)

DVWA.yml (https://github.com/williamrcolwell/ELK-Container-Setup/blob/master/Ansible/DVWA.yml)

filebeat-playbook.yml (https://github.com/williamrcolwell/ELK-Container-Setup/blob/master/Ansible/filebeat-playbook.yml)

metricbeat-playbook.yml (https://github.com/williamrcolwell/ELK-Container-Setup/blob/master/Ansible/metricbeat-playbook.yml)


# This document contains the following details:

1. Description of the Topology
2. Access Policies
3. ELK Configuration 
4. Beats in Use
5. Machines Being Monitored
6. How to Use the Ansible Build

# Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.  Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.  Load balancers are necessary for today’s applications.  They ensure that no one server will be overloaded, and the inbound traffic will be routed to other identical servers.  In this diagram the traffic is shared by the webservers.  In addition, load balancers run consistent health checks on the servers to determine which are available for the inbound traffic requests.  Load balancers can aid in protecting against denial of service attacks as it can distribute the traffic evenly.  The advantage of the jump box is that it provides access controls.  In general, a jump box is a secure computer that administrators can login to before connecting to any of the other servers within the network or perform other administrative tasks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the virtual machines on the network and system metrics.  For example, Filebeat monitors log files or specified locations and collects log data to be forwarded for indexing.  While Filebeat is a log management tool, Metricbeat is a monitoring tool.  It records metrics from the system and various services such as CPU Usage and priviledge escalations.

The configuration details of each machine may be found below. 

|      Name      |      Function |      IP Address |      Operating System |
|----------------|---------------|-----------------|-----------------------|
| Jump Box       | Gateway       | 10.0.0.1        | Linux                 |
| Web-1 (DVWA 1) | Webserver     | 10.0.0.5        | Linux                 |
| Web-2 (DVWA 2) | Webserver     | 10.0.0.6        | Linux                 |
| Web-3 (DBWA 3) | Webserver     | 10.0.0.7        | Linux                 |
| ELK            | Monitoring    | 10.1.0.4        | Linux                 |

# Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address: 23.124.110.82

Machines within the network can only be accessed by each other.  Webservers 1-3 (DVWA VM 1, DVWA VM 2, DVWA VM 3 send traffic to the ELK server.  The Jump Box has access to the ELK VM with the IP address 13.66.198.8

A summary of the access policies in place can be found in the table below.

| Name           | Publicly Available | Allowed IP    |
|----------------|--------------------|---------------|
| Jump Box       | Yes                | 23.124.110.82 |
| Web-1 (DVWA 1) | No                 | 10.0.0.1-254  |
| Web-2 (DVWA 2) | No                 | 10.0.0.1-254  |
| Web-3 (DBWA 3) | No                 | 10.0.0.1-254  |
| ELK            | No                 | 10.0.0.1-254  |

# Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows you to perform mass updates, configurations, etc. at once instead of on each separate machine.  You can deploy multiple identical virtual machines in an instant.

The playbook implements the following tasks:

1. First the playbook will install Docker.io
2. Then follows that by installing the python3-pip and then installing the docker module so it defaults to pip3.
3. It increases the memory so that there is enough to run the ELK container.

Once the above steps are completed it then downloads and installs the ELK container.

YAML file found here: https://github.com/williamrcolwell/ELK-Container-Setup/blob/master/Ansible/install-elk.yml

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.

https://github.com/williamrcolwell/ELK-Container-Setup/blob/master/Diagrams/Screen%20Shot%202020-09-16%20at%202.33.22%20PM.png

# Target Machines & Beats

This ELK server is configured to monitor the following machines:

Web-1 (DVWA) VM: 10.0.0.5
Web-2 (DVWA) VM: 10.0.0.6
Web-3 (DVWA) VM: 10.0.0.7

We have installed the following Beats on these machines:

Filebeat
Metricbeat

These Beats allow us to collect the following information from each machine:

Filebeat: This monitors logs files and/or other locations you specify.  This will detect changes to the file system and in this case specifically Apache logs.  

Metricbeat: This will monitor and collect information regarding changes in the system.  We will use this monitor for sudo priviledges, CPU usage, etc.

# Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

1. SSH into the control node and follow the steps below:
2. Copy the /etc/ansible/install-elk.yml file to the docker session.
3. Update the Ansible host file to include the IP addresses of the servers to have ELK installed under the ELK header (compared to Webservers)
4. Run the playbook and navigate to the public IP address of the ELK server (http://[your ELK Server Public IP]:5601/app/kibana) to check that the installation worked as expected.
