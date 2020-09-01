## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.
	Images/diagram_filename.png 

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the install-elk.yml file may be used to install only certain pieces of it, such as Filebeat.


This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant, in addition to restricting inbound traffic to the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system servers.

The configuration details of each machine may be found below.


| Name                 | Function              | IP Address | Operating System | 
|----------------------|-----------------------|------------|------------------|
| Jump-Box-Provisioner | Gateway               | 10.0.0.4   | Linux            | 
| Web-1                | Hosts DVWA containers | 10.0.0.5   | Linux            | 
| Web-2                | Hosts DVWA containers | 10.0.0.6   | Linux            |  
| Web-3                | Hosts DVWA containers | 10.0.0.7   | Linux            |  
| vNetmachine          | Hosts ELK  containers | 10.1.0.4   | Linux            | 


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the vnetmachine machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 174.52.66.3

Machines within the network can only be accessed by an ubuntu virtual machine with the public IP address 174.52.66.3 .


A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Address     |
|----------------------|---------------------|------------------------|
| Jump-Box-Provisioner | No                  | 174.52.66.3            |
| Web-1                | No                  | 10.0.0.4               |
| Web-2                | No                  | 10.0.0.4               |
| Web-3                | No                  | 10.0.0.4               |
| vNetmachine          | No                  | 174.52.66.3            |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because automate configuration 
is faster than manual configuration
		

The playbook implements the following tasks:
- The playbook first installs docker onto the virtual machine, vnetmachine.
- Next, python3 is installed and then python3-pip is installed.
- Then the playbook increases memory and after the machine is instructed to use the extra memory created. 
- Lastly, the playbook downloads and launches the docker elk container on the virtual machine, vnetmachine.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
	
	Images/docker_ps_output.png 

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
	Web-1: 10.0.0.5
	Web-2: 10.0.0.6
	Web-3: 10.0.0.7

We have installed the following Beats on these machines:
	Filebeat and Metric beat 

These Beats allow us to collect the following information from each machine:
	
  Filebeat collects data about the file system, like files that have changed and when. Metricbeat collects metrics from the operating system and services, such as CPU and memory usage.    

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-configuration.yml file to /etc/ansible/files/.
- Update the filebeat-configuration.yml file to include
	
		output.elasticsearch:
	         hosts: ["10.1.0.4:9200"]
 	  	 username: "elastic"
	  	 password: "changeme"

		setup.kibana:
	         host: "10.1.0.4:5601"


- Run the playbook, and navigate to 

http://13.82.150.250:5601/app/kibana#/dashboard/Filebeat-syslog-dashboard-ecs?_g=(refreshInterval:(pause:!t,value:0),time:(from:now-15m,to:now))&_a=(description:'Syslog%20dashboard%20from%20the%20Filebeat%20System%20module',filters:!(),fullScreenMode:!f,options:(darkTheme:!f),panels:!((embeddableConfig:(),gridData:(h:16,i:'1',w:32,x:0,y:4),id:Syslog-events-by-hostname-ecs,panelIndex:'1',type:visualization,version:'7.6.1'),(embeddableConfig:(),gridData:(h:16,i:'2',w:16,x:32,y:4),id:Syslog-hostnames-and-processes-ecs,panelIndex:'2',type:visualization,version:'7.6.1'),(embeddableConfig:(columns:!(host.hostname,process.name,message),sort:!('@timestamp',desc)),gridData:(h:28,i:'3',w:48,x:0,y:20),id:Syslog-system-logs-ecs,panelIndex:'3',type:search,version:'7.6.1'),(embeddableConfig:(),gridData:(h:4,i:'4',w:48,x:0,y:0),id:'327417e0-8462-11e7-bab8-bd2f0fb42c54-ecs',panelIndex:'4',type:visualization,version:'7.6.1')),query:(language:kuery,query:''),timeRestore:!f,title:'%5BFilebeat%20System%5D%20Syslog%20dashboard%20ECS',viewMode:view) 

to check that the installation worked as expected.
