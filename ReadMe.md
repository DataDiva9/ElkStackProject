## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[NetworkDiagram.png](Diagrams/NetworkDiagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the NetworkDiagram file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient and available, in addition to restricting potential Distributed Denial-of-Service (DDoS) attacks to the network.

###Question: What aspect of security do load balancers protect? 

Answer: Load balancers help protect the CIA triad (Availability, Integrity, Confidentially) in several ways. In the most general terms, they ensure that the authorized users of a system have timely and uninterrupted access to information (Availability). 

Load balancers provide:

	1.	Distributed Allocation
	⁃	Manages system thru-put so no single device is overly burdened or taxed.
	⁃	predictive algorithms can determine bottlenecks before they happen and proactively shift traffic to a different server
	⁃	SSL (Secure Sockets Layer) is the standard technology that establishes a link between web server and browser, passing the request on (SSL Termination). This saves the web server from having to “spend” extra CPU cycles required for description, which increases application performance. 
	⁃	This introduces a risk since the traffic between the load balancer and web server is no longer encrypted. 
	⁃	This risk is reduced when the load balancer is housed in the same data center as the web server.
	2.	High Availability
	⁃	Redundancy
	⁃	Clustering multiple systems connected cooperatively (like what we’ve done here) eliminates the single point of failure. reduces the “attack surface”, and makes it harder for a hacker to “own” the system.
	⁃	Fail Over
	⁃	in case of failure, other servers/machines/systems pick up the slack and allow the system to avoid down time. 
	3.	Disaster Recovery
	⁃	Should the redundancy in the system allows recovery to happen as quickly as possible. Infected or vulnerable servers can be brought down, investigated, tinged, and fixed. A redundant copy of information allows the network as whole to be recovered much faster while minimizing loss. 



###Question: What is the advantage of a jump box?

Answer: A jump box is a secure computer that provides a portal for admins to use before launching an administrative task. To remain secure, they should never bu used for ANY non-administrative task - this is the only way to mitigate the high risk associated with common activities like using email, browsing the internet, or various office administrative/productivity applications. This clean bridge helps ensure that malware is not “virally transmitted” to other secure networks, servers, and secure technology zones.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network infrastructure and system logs.



###Question: What does Filebeat watch for?

Answer: Monitors log files or locations that the sysadmin specifies. It records/collects these events/log files and forwards them to an indexing tool like Elasticsearch or Logstash.



###Question: What does Metricbeat record?

Answer: Elastic Beat based on the libbeat framework. Metricbeat monitors servers (and hosted services) and collects metrics like uptime and CPU usage. Metricbeat can insert the collected metrics directly into Elasticsearch or send them to Logstash, Redis, or Kafka. You can build visualizations and dashboards in Kibana. 


###The configuration details of each machine may be found below.

|| Friendly Name | Name               | Function               | Public IP     | Private IP | Operating System |
|---------------|--------------------|------------------------|---------------|------------|------------------|
| Jump Box      | JumpBoxProvisioner | Gateway                | 52.188.122.25 | 10.0.0.5   | Linux            |
| VM1           | DVWA1              | Web Server             | 52.188.40.228 | 10.0.0.6   | Linux            |
| VM2           | DVWA2              | Web Server (Reduntant) | 52.188.40.228 | 10.0.0.7   | Linux            |
| ELK VM        | ELK                | ElkServer              | 52.247.56.101 | 10.1.0.4   | Linux            |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box_Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

108.64.85.132


Machines within the network can only be accessed by the Jump-Box-Provisioner.

###Question: Which machine did you allow to access your ELK VM? What was its IP address?

Answer: Only the Jump-Box-Provisioner has access to the the ELK VM. IP: 52.188.122.25 (10.0.0.5)

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible                 | Allowed IP Address |
|----------------------|-------------------------------------|--------------------|
| Jump-Box-Provisioner | No (Only from VM)                   | 108.64.85.132      |
| DVWA1                | No (Only from Jump-Box-Provisioner) | 10.0.0.5           |
| DVWA2                | No (Only from Jump-Box-Provisioner) | 10.0.0.5           |
| ELK                  | No (only from Jump-Box-Provisioner) | 10.0.0.5           |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

###Question: What is the main advantage of automating configuration with Ansible?

Answer: There are several advantages of Ansible (open source, powerful, efficient). However, the main advantage of configuring with Ansible is its flexibility - you can configure the entire, multi-layer application without having to custom code everything. Once the needed tases are written into the playbook, sensible figures out the rest for you. You don’t have install extra software, which reduces server load. 


The playbook implements the following tasks:
###In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
	▪	SSH into the Jump-Box-Provisoner from local host
	▪	Start and attach the Ansible Docker Container (as root)
	▪	Create Elk Playbook (elk.yml) in /etc/ansible/
	▪	Run Elk Playbook (ansible-playbook elk.yml)
	▪	SSH into ELK VM to verify configuration and that it is up and running

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
	•	DVWA1 - 10.0.0.6
	•	DVWA2 - 10.0.0.7

We have installed the following Beats on these machines:
	•	Filebeat
	•	Metricbeat

These Beats allow us to collect the following information from each machine:

###Question: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. 

Filebeat: monitors log files/locations that you specify. Forwards to Elasticdearch for indexing. Can handle audit logs, depreciation, server logs. 

Metricbeat: Collects metrics from your server (os & services) and sends them where you specify.  Can be visualized in Kibana

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-configuration.yml file to /etc/ansible/roles/files
- Update the filebeat-configuraion.yml file to include the ELK VM private IP in lines 1106 and 1806.
- Run the playbook, and navigate to 52.247.56.101:5601 to check that the installation worked as expected.

###Questions: 
- Which file is the playbook? 
	filebeat-playbook.yml
- Where do you copy it?
	/etc/ansible/roles/
- Which file do you update to make Ansible run the playbook on a specific machine? 
	the hosts file /etc/ansible/hosts
- How do I specify which machine to install the ELK server on versus which to install Filebeat on?
	I created a new group [elkservers] and added the ELK VM private IP. 
	Filebeat: I put the DVWA1 and DVWA2 under [webservers]

- Which URL do you navigate to in order to check that the ELK server is running?
	http://52.247.56.101:5601

