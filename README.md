# ELK-Container-Setup

This repository contains the necessary documents to install an ELK Container/Server

# Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/williamrcolwell/ELK-Container-Setup/blob/master/Diagrams/Week%2013%20Network%20Diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yaml file may be used to install only certain pieces of it, such as Filebeat.  These are all found in the /ansible subfolder of this directory.

install-elk.yml
DVWA.yml
filebeat-playbook.yml
metricbeat-playbook.yml

# This document contains the following details:
1. Description of the Topology
2. Access Policies
3. ELK Configuration 
4. Beats in Use
5. Machines Being Monitored
6. How to Use the Ansible Build

# Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.  Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.  Load balancers are necessary for today’s applications.  They ensure that no one server will be overloaded, and the inbound traffic will be routed to other identical servers.  In this diagram the traffic is shared by the webservers.  In addition, load balancers run consistent health checks on the servers to determine which are available for the inbound traffic requests.  Load balancers can aid in protecting against denial of service attacks as it can distribute the traffic evenly.  The advantage of the jump box is that it provides access controls.  In general, a jump box is a secure computer that administrators can login to before connecting to any of the other servers within the network or perform other administrative tasks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the virtual machines on the network and system metrics.  For example, Filebeat monitors log files or specified locations and collects log data to be forwarded for indexing.  While Filebeat is a log management tool, Metricbeat is a monitoring tool.  It records metrics from the system and services including CPU, memory, Redis, and NGINX.

The configuration details of each machine may be found below. Note: Use the Markdown Table Generator to add/remove values from the table.

|      Name      |      Function |      IP Address |      Operating System |
|----------------|---------------|-----------------|-----------------------|
| Jump Box       | Gateway       | 10.0.0.1        | Linux                 |
| Web-1 (DVWA 1) | Webserver     | 10.0.0.5        | Linux                 |
| Web-2 (DVWA 2) | Webserver     | 10.0.0.6        | Linux                 |
| Web-3 (DBWA 3) | Webserver     | 10.0.0.7        | Linux                 |
| ELK            | Monitoring    | 10.1.0.4        | Linux                 |

Access Policies

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

•	First the playbook will install Docker.io
•	Then follows that by installing the python3-pip and then installing the docker module so it defaults to pip3.
•	It increases the memory so that there is enough to run the ELK container.

  Once the above steps are completed it then downloads and installs the ELK container.

Full .yaml file below for convenience:

---
- name: Configure Elk VM with Docker
  hosts: elk
  remote_user: sysadmin
  become: true
  tasks:
    # Use apt module
    - name: Install docker.io
      apt:
        update_cache: yes
        force_apt_get: yes
        name: docker.io
        state: present

      # Use apt module
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

      # Use command module
    - name: Increase virtual memory
      command: sysctl -w vm.max_map_count=262144

      # Use sysctl module
    - name: Use more memory
      sysctl:
        name: vm.max_map_count
        value: '262144'
        state: present
        reload: yes

      # Use docker_container module
    - name: download and launch a docker elk container
      docker_container:
        name: elk
        image: sebp/elk:761
        state: started
        restart_policy: always
        # Please list the ports that ELK runs on
        published_ports:
          -  5601:5601
          -  9200:9200
          -  5044:5044

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.
 
Note: The following image link needs to be updated. Replace docker_ps_output.png with the name of your screenshot image file.

# Target Machines & Beats

This ELK server is configured to monitor the following machines:
•	Web-1 (DVWA) VM: 10.0.0.5
•	Web-2 (DVWA) VM: 10.0.0.6
•	Web-3 (DVWA) VM: 10.0.0.7

We have installed the following Beats on these machines:
•	Filebeat
•	Metricbeat

These Beats allow us to collect the following information from each machine:
•	Filebeat: This monitors logs files and/or other locations you specify.  This will detect changes to the file system and in this case specifically Apache logs.  
•	Metricbeat: This will monitor and collect information regarding changes in the system.  We will use this monitor for sudo priviledges, CPU usage, and more.

# Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

1. SSH into the control node and follow the steps below:
2. Copy the /etc/ansible/install-elk.yml file to the docker session.
3. Update the Ansible host file to include the IP addresses of the servers to have ELK installed under the ELK header.
4. Run the playbook and navigate to the public IP address of the ELK server (http://20.36.198.2:5601/app/kibana) to check that the installation worked as expected. (Replace the IP in the URL with your own ELKVM's Public IP Address).
