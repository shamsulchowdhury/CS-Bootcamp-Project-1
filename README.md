## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Project 1 Red-Team  Network Diagram](images/elk-stack-diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - [DVWA Webservers Playbook](playbook-and-config-files/redteam.yml)
  - [Ansible Config File](playbook-and-config-files/ansible.cfg)
  - [Ansible Hosts File](playbook-and-config-files/ansible.hosts)
  - [ELK Stack Playbook](playbook-and-config-files/install-elk.yml)
  - [Filebeat Playbook](playbook-and-config-files/redteam.yml)
  - [Filebeat Config](playbook-and-config-files/redteam.yml)
  - [Metric Beat Playbook File](playbook-and-config-files/metric-playbook.yml)
  - [DVWA Webservers Playbook](playbook-and-config-files/metricbeat-config.yml)



This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly functional, in addition to restricting high traffic to the network.
- What aspect of security do load balancers protect? 
  - It helps prevent overloading servers as well as optimizes productivity and maximizes uptime.
  - It also adds resiliency by rerouting live traffic from one server to another causing it to eliminate single points of failure from attacks such as DDoS attack.
- What is the advantage of a jump box?
  - Jump-box are highly secured computers that are never used for non-admin tasks. -Throughout the years, jump-box has improved into an even more comprehensive/lock-down secure admin workstation to decrease the chances of hackers/malware infection.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs.

- What does Filebeat watch for?
  - It monitors the log files/locations that you specify and forwards them to Elasticsearch/Logstash for indexing.
- What does Metricbeat record?
  - It records metrics/statistics data and transports them to the output that you specifics thru Elasticsearch/Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| JBOX | Gateway  | 10.1.0.7   | Ubuntu Linux            |
| Web-01     | Server         | 10.1.0.10           |    Ubuntu Linux              |
| Web-02     |    Server      |   10.1.0.9         |        Ubuntu Linux          |
| Web-03    |   Server       |      10.1.0.11      |          Ubuntu Linux        |
| ELK-VM    |     Server     |   10.2.0.4         |           Ubuntu Linux       |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JBOX  and ELK-Stack machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- My home IP address

Machines within the network can only be accessed by JBOX Provisioner.


A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| JBOX | Yes             | My Home IP Address   |
|    ELK-VM      |   Yes                  |   My Home IP Address                   |
|     Web-01     |          No          |      10.1.0.7                |
|       Web-02   |             No        |        10.1.0.7              |
|      Web-03    |             No        |         10.1.0.7             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- What is the main advantage of automating configuration with Ansible?
  - One main advantage would be YAML Playbooks. It is the best alternative for configuration management/automation.
  - It is also able to automate complex multi-tier IT application environemtns.

The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
  - First I, SSH into the JBOX (ssh azureuser@20.224.244.61)
  - Start/Attached to the ansible docker (sudo docker start tender_morse)/(sudo docker attach tender_morse)
  - Went to /etc/ansible/ directory and created the ELK playbook (install-elk.yml)
  - Ran the install-elk.yml in that same directory 
  - Lastly, I SSH into the ELK-VM to verify the server is up and running.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[ELK-VM Docker PS](images/jbox-docker-ansible.PNG)

[ELK Kibana Homepage](images/kibana-home-page.PNG)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-01  10.1.0.10
- Web-02  10.1.0.9
- Web-03 10.1.0.11

We have installed the following Beats on these machines:
- [File Beat](images/filebeat-img.PNG)
- [Metric Beat](images/metric-beat-img.PNG)

These Beats allow us to collect the following information from each machine:
- Filebeat is used to collect log files from specific files on remote machines.

- Examples of Filebeats can be files that are generated by Apache, Microsoft Azure tools, the Nginx web server, and MySQl databases.

- Metricbeat collects machine metrics.

- It is simply a measurement to tell analysts how healthy it is.

- Examples of Metricbeat can be CPU usage/Uptime

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

---Filebeat---

- Copy the filebeat-config.yml file to /etc/ansible/.
- Update the filebeat-configuration.yml file to include the ELK private IP in lines 1106 and 1806.
- Run the playbook, and navigate to http://20.239.78.241:5601/ (ELK-VM public IP) to check that the installation worked as expected..

---Metricbeat---

- Copy the metricbeat-config.yml file to /etc/ansible.
- Update the metricbeat-configuration.yml file to include the ELK private IP in lines 62 and 96.
- Run the playbook, and navigate to http://20.239.78.241:5601/ (ELK-VM public IP) to check that the installation worked as expected.
- [Screen Shot of /etc/ansible folder](images/ansible-files.PNG)

Answer the following questions to fill in the blanks:_
- Which file is the playbook? Where do you copy it?
  - [filebeat-playbook.yml](playbook-and-config-files/filebeat-playbook.yml) in `etc/ansible` folder
- Which file do you update to make Ansible run the playbook on a specific machine? 
  - `/etc/ansible/hosts` file (IP of the Virtual Machines). 
- How do I specify which machine to install the ELK server on versus which to install Filebeat on?
  - I have to specify two separate groups in the etc/ansible/hosts file. One of the groups will be webservers which has the IPs of the VMs that I will install Filebeat to. The other group is named elkservers which will have the IP of the VM I will install ELK to.
- Which URL do you navigate to in order to check that the ELK server is running?
  - http://20.239.78.241:5601/

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

-------Filebeat---------

	- To create the filebeat-configuration.yml file: nano filebeat-configuration.yml. For this, I used the filebeat configuration file template.
	
	- To create the playbook: nano filebeat-playbook.yml
	
      ---
 	 - name: installing and launching filebeat
    	   hosts: webservers
           become: true
           tasks:

    	   - name: download filebeat deb
      	     command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.7.1-amd64.deb

    	   - name: install filebeat deb
      	     command: dpkg -i filebeat-7.7.1-amd64.deb

    	   - name: drop in filebeat.yml
      	     copy:
       	       src: ./files/filebeat-configuration.yml
       	       dest: /etc/filebeat/filebeat.yml

    	   - name: enable and configure system module
      	     command: filebeat modules enable system

    	   - name: setup filebeat
      	     command: filebeat setup

    	   - name: start filebeat service
      	    command: service filebeat start
	---
	-To run the playbook: ansible-playbook filebeat-playbook.yml
	
	* In order to run the playbook, you have to be in the directory the playbook is at, or give the path to it (ansible-playbook /etc/ansible/filebeat-playbook.yml
	
	
	-------Metricbeat-------
	
	- To create the metricbeat-configuration.yml file: nano metricbeat-configuration.yml. For this, I used the metricbeat configuration file template.
	
	- To create the playbool: nano metricbeat-playbook.yml
	
	---
	  - name: installing and lunching metricbeat
	    hosts: webservers
	    become: true
	    tasks:
	    
	  - name: download metricbeat deb
	    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.7.1-amd64.deb
	    
	  - name: install metricbeat deb
	    command: sudo dpkg -i metricbeat-7.7.1-amd64.deb
	    
	  - name: drop in metricbeat.yml
	    copy:
	      src: /etc/ansible/roles/files/metricbeat-configuration.yml
	      dest: /etc/metricbeat/metricbeat.yml
	      
	   - name: enable and configure system module
	     command: metricbeat modules enable system
	     
	   - name: setup metricbeat
	     command: metricbeat setup
	     
	   - name: start metricbeat service
	     command: service metricbeat start
	     
	   ---
	   
	   - To run the playbook: ansible-playbook metricbeat-playbook.yml
	   
	   * To order to run the playbook, you have to be in the directory the playbook is at, or give the path to it (ansible-playbook /etc/ansible/metricbeat-playbook.yml
	    