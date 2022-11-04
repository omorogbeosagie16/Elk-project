## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![CLOUD NETWORK](https://user-images.githubusercontent.com/86134880/122697274-fcf39c00-d212-11eb-89e4-a3d1cf17bd09.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above or alternatively, select portions of the .YML file may be used to install only certain pieces of it, such as Filebeat.

 [elkinstall-playbook.yml](https://github.com/omorogbeosagie16/Elk-project/blob/a5025d6f86e77140485179d5e5f263001ce57c76/Ansible/elkinstall-playbook.yml)
                              [filebeat-playbook.yml](https://github.com/omorogbeosagie16/Elk-project/blob/f09889fedf19dde5349e5313c3766cf4be519df8/Ansible/filebeat-playbook.yml)

This document contains the following details:

- Description of the Topology
- Access Policies
- ELK Configuration
- Beats in Use
- Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly avaiable, in addition to restricting access to the network.

Load balancers help to protect against DDos and Dos attacks by ensuring that web assets are deployed across multiple machines so if one machine goes down, traffic is redirected to the others. 

A JumpBox is the orgination point for launching Administrative Tasks. It serves as a gateway from outside to your internal network. A Jump Box machine also allows for ansible configuration deployments across multiple VM's at once using one Playbook thereby making sure all machine within the network are configured the same way with the same operating systems.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.

  Filebeat monitors system log files from all VM's on the network or location that are specified. it collects log events from these locations and sends to either elastic search or logstash for indexing. 


Metricbeats helps monitor servers on a network by recording metrics and statistical data from the systems and services running on the server. 

The configuration details of each machine may be found below.

| NAME     	| FUNCTIONS            	| IP ADDRESSES 	| OPERATING SYSTEM         	|
|----------	|----------------------	|--------------	|--------------------------	|
| JUMP BOX 	| GATEWAY ANSIBLE HOST 	| 10.1.0.5     	| LINUX (UBUNTU 18.04 LTS) 	|
| WEB 1-VM   	| DVWA-HOST            	| 10.1.0.6     	| LINUX (UBUNTU 18.04 LTS) 	|
| WEB 2-VM    	| DVWA-HOST            	| 10.1.0.7     	| LINUX (UBUNTU 18.04 LTS) 	|
| ELK-VM 	| ELK SERVER           	| 10.2.0.4     	| LINUX (UBUNTU 18.04 LTS) 	|


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JUMPBOX machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

99.254.1.105

Machines within the network can only be accessed by SSH. The only machine that is able to connect to the ELK server is the JUMPBOX (10.1.0.5).

A summary of the access policies in place can be found in the table below.

| NAME       	| PUBLICLY ACCESSIBLE 	| ALLOWED IP ADDRESSES  	|
|------------	|---------------------	|-----------------------	|
| JUMP BOX   	|        YES           	| 99.254.1.105          	|
| WEB 1-VM   	|        NO           	| 10.1.0.5              	|
| WEB 2-VM   	|        NO           	| 10.1.0.5              	|
| ELK SERVER 	|        YES          	| 99.254.1.105 10.1.0.5 	|



### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because when the playbook is made and deployed, it makes sure all system on the network have the same configurations across the board. The main advantage is automating with ANSIBLE is that it saves time and resources, because it is a container, it uses less resources to operate and with a single playbook configuration, you can configure multiple machine across the network at the same time.

The playbook implements the following tasks:

1. Set the maximum map count
2. installs docker.io which is the engine for runnig containers
3. installs Python3-pip, the package used to install python software
4. Install Python docker module which ansible uses to to control container
5. Installs and starts the ELK server container, assigns specific ports to the containers.


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![elk-installed](https://user-images.githubusercontent.com/86134880/122697760-016c8480-d214-11eb-816a-a379a2d3a9a3.png)



### Target Machines & Beats

This ELK server is configured to monitor the following machines: WEB 1-VM (10.1.0.6) and WEB 2-VM (10.1.0.7)

We have installed the following Beats on these machines: FILEBEAT AND METRICBEAT

These Beats allow us to collect the following information from each machine: Filebeat enables us to monitor log files, it collects log events and forwards them to elastic search or logstash for indexing. it can collect logs from audit logs, gc logs, server logs which are all used to monitor specfic logs from different department within the server. While Metricbeat collects metric data from systems running on the server, like CPU usuage, RAM memory and Network activities.



### Using the Playbook


In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

Copy the -curl funtion to download the git files to the destination machine.

Copy the filebeat.yml and metricbeat.yml file to the /etc/ansible/roles/files/ directory.

Update the configuration files to include the Private IP of the Elk-Server to the ElasticSearch and Kibana sections of the configuration file.

Create a new ansible-playbook filebeat-playbook.yml in the /etc/ansible/roles/ directory that will install, drop the filebeat.yml and metricbeat.yml files from the /etc/ansible/roles/files/ directory to the ELK server machine, update the configuration files, and start the service for both Filebeat and Metricbeat.

Run the playbook, and navigate to ELk-Server to check that the installation worked as expected. [docker ps]


Answer the following questions to fill in the blanks:

Which file is the playbook? Where do you copy it? 

The playbook are called filebeat-playbook.yml and metricbeat-playbook.yml. You copy the files to the "/etc/filebeat/filebeat.yml" and /etc/metricbeat/metricbeat.yml" directory


Which file do you update to make Ansible run the playbook on a specific machine? 

You update the hosts file to designate which machines the playbook will install configurations on.


How do I specify which machine to install the ELK server on versus which to install Filebeat on? 

In the hosts file you have to designate a hosts server section and a seperate elk server section with the appropriate IP's. Then in your playbook you would designate which host group you are pushing the data to.


Which URL do you navigate to in order to check that the ELK server is running? http://[your.ELK-VM.External.IP]:5601/app/kibana


_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

from localhost : ssh RedAdmin@JumpBoxpublicIP

sudo docker start (name of the container)

sudo docker attach container (name of the container)

cd /etc/ansible/files

nano elk.yml and configure IP's

cd ..

cd roles

nano elkinstall-playbook.yml and configure task then save

ansible-playbook elkinstall.playbook.yml to run playbook.


cd /etc/ansible/files

nano filebeat.yml and configure IP's

cd ..

cd roles

nano filebeat-playbook.yml and configure task then save

ansible-playbook filebeat.playbook.yml to run playbook.

ansible-playbook filebeat-playbook.yml (installs Filebeat and Metricbeat)


open a new web browser (Elk-Server PublicIP:5601) This will bring up the Kibana Web Portal
