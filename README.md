Airavata-Ansible-Deployment
===========================
1. Playbooks:
  - site.yml: Complete installation from scratch of all Airavata components, including dependencies. 
  - airavata.yml: Airavata only updates, with cleanup of the old installation, and restart of ZooKeeper server. 
  - reset_firewall.yml: An example of deleting firewall rules. 

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
