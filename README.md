Airavata-Ansible-Deployment
===========================
1. Playbooks:
  - site.yml: Complete installation from scratch of all Airavata components, including dependencies. 
  - airavata.yml: Airavata only updates, with cleanup of the old installation, and restart of ZooKeeper server. 
  - reset_firewall.yml: An example of deleting firewall rules. 
  - single_airavata.yml: Playbook for single all_in_one airavata server update. 
  - restart_rabbitmq.yml: Restart RabbitMQ server.
  - restart_airavata.yml: Restart ZooKeeper and Airavata servers.
  - restart_single_airavata.yml: Restart ZooKeeper and all-in-one single Airavata server. 

2. hosts:
  - Server groups are defined. Each group (e.g. dbserver, apiserver, etc.) can contain multiple servers. Variables defined under [airavata:vars] are used for the airavatar-server.properties file. For example, if multiple servers are deployed of a single component, say dbserver, for load-balancing purpose, the variable db_server defined under [airavata:vars] should point to the load-balancer address of the group of servers. 

3. group_vars:
  - all: global variables used by all server groups, including Java and database information, as well as server ports. Set the variable "dbpassword" to the real database password before deployment. 
  - airavata: airavata server group variables. "old_airavata_version" is used for cleanup of previous installations by airavata.yml. "myproxy_password" and "ssh_keypass" need to be set correctly before deployment. "local_ssh_key_path" is the local directory of airavata ssh key pairs to be copied to the airavata user home directory on remote machines. 	
  - rabbitmq: rabbimq server group variables.
  - zookeeper: zookeeper server group variables. 
Update any download_url and installation paths as desired before deployment. 

4. roles:
  - roles/*/tasks/main.yml performs real work.  
  - roles/*/handlers/main.yml restarts iptable when firewall rules are updated.
  - roles/*/meta/main.yml defines the dependency of individual airavata server component to the airavata_common role, with parameterized download url.  

5. Internal Notes:
  - Default settings included in group_vars/* are conform with gw127, only airavata version variables and passwords need to be set correctly before deployment:
	* "airavata_version" and "old_airavata_version" in group_vars/airavata;
	* "myproxy_password", "ssh_keypass" and "pga_passphrase" in group_vars/airavata;
	* "dbpassword" in group_vars/all
  - For deployment on gw111, copy the set of dev configurations as follows:
	cp group_vars/dev_all group_vars/all 
	cp group_vars/dev_airavata group_vars/airavata
	cp group_vars/dev_zookeeper group_vars/zookeeper
 	cp dev_hosts hosts
    Then update version variables and passwords as outline above. 
   - Execute one of the playbooks as desired. For example:
	* To restart all-in-one airavata server:
		ansible-playbook -i hosts restart_single_airavata.yml
	* To redeploy all-in-one airavata server:
		ansible-playbook -i hosts single_airavata.yml
