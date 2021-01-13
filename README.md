# Elk-Server-Project
Project 1 for Cybersecurity 
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Under the folder called diagrams,you will find the configuration for the network. 


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Ansible file may be used to install only certain pieces of it, such as Filebeat.

 ---
 - name: Config Web VM with docker
   hosts: webservers
   become: true
   tasks:
   
   - name: docker.io
     apt: 
       force_apt_get: yes
       name: docker.io
       state: present
       update_cache: yes
       
    -name: install python3-pip
     apt:
       force_apt_get: yes
       name: python3-pip
       state: present
       
    - name: install Docker python module
      pip: 
        name: docker
        state: present 
        
     - name: download and launch docker web container
       docker_container:
         name: dvwa
         image: cyberxsecurity/dvwa
         state: started
         published_ports: 80:80
         
      - name: Enable docker service
        systemd:
          name: docker 
          enabled: yes 

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly accessible, in addition to restricting access to the network.
- load balancing can truly protect you from Ddos attacks. It does this by shifting attack traffic from the coroporate server to a public cloud provider. It is also important to have a jump server because it is a hardened and monitored device. It can act like a security zone. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the DVWA and system servers.
- Filebeat monitors the log files and locations that you specify. 
- metricbeat takes the metrics and statistics that it collects and ships them to the output. 

The configuration details of each machine may be found below.


| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.8   | Linux            |
| WEB 1    | DVWA     | 10.0.0.9   | Linux            |
| WEB 2    | DVWA     | 10.0.0.10  | Linux
| WEB 3    | DVWA     | 10.0.0.11  | Linux

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 73.153.4.233

Machines within the network can only be accessed by sda
- The machine that i allow to enter the ElK container is the JumpBox ansible container. 

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 10.0.0.8             |
|          |                     |                      |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- the main advantage of ansible is that it is very easy to setup. There is no special coding that is needed for it all. 

The playbook implements the following tasks:
- install docker.io
- install python3-pip
- install Docker Module
- Increase virtual memory
- use more memory
- download and launch a docker elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

-the folder titled Ansible will include the script needed to apply. 

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.1.0.4

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- filebeat monitors the log files that I ask it to watch
- Metricbeat takes the metrics and statistics that it collects and ships them to the output. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to webservcrs.
- Update the filebeat-config.yml file to include...
- Run the playbook, and navigate to /etc/ansible/files/filebeat-config.yml to check that the installation worked as expected.

- the filebeat file is under filebeat-config.yml and you will nano in to place it
- Make sure to specify that the webservers are the hosts for the configuration. 
- http://[elk-vm-ip]:5601/app/kibana is the URL you will use to make sure that you are connected properly. 


