# CBSA - For Stock Dev/Test Image ONLY !

General Information:

CBSA, for this playbook, is obtained from here: https://github.com/cicsdev/cics-banking-sample-application-cbsa

This Playbook will install CBSA onto the z/OS Stock Dev/Test Image only. Without major modifications it will not work on any other z/OS Instance/Image.
During execution, this playbook will SHUTDOWN the CICS subsystem. CICS will be COLD STARTED during one of the final steps. The CICS Procedure is updated in order to define CBSA artifacts and to also enable a connection to DB2. zOS/Connect configuration is updated to provide the API/Services for CBSA. z/OS Connect will also be shutdown/restarted.

The CBSA Git Repo will be cloned to the local host (the host running the playbook). 

Due to limitations of some processes, it is suggested this playbook be executed when there are NO TSO Users logged on to the z/OS host.


Variable changes needed:
In => roles/cbsa/vars/main/yml
zosver: 31 or 25 for z/OS 3.1 or z/OS 2.5 respecitively. z/OS 3.1 is default so if you want to install for z/OS 2.5 pass -e 'zosver=25'
zos_user: provide a TSO user id to be used for testing CBSA API's (IBMUSER for example)
zos_user_password: Password for TSO user id 
cbsa_base: Directory on local host where the CBSA git Repo will be cloned (for example, /home/user/cbsa/) 

Most of the other variables would only need changing if there are major z/OS and subsystem changes (for example: DB2 Version change)

To execute:
Enable ssh-key authentication from the host you are running the playbook to the z/OS host!
It is expected that /etc/ansible/hosts is populated with the host to configure, for example it might contain the following:
hostxxx ansible_host=ipaddress ansible_port=22 ansible_ssh_user=ibmuser ansible_ssh_pass=password ansible_python_interpreter=/usr/lpp/IBM/cyp/v3r11/pyz/bin/python 
(ansible_ssh_pass is not used since ssh key is used for login, update python directory as required) 

After cloning this Repo change directory to the base and run: 
for z/OS 3.1:
ansible-playbook -e 'target_host=xxx' cbsa.yaml (change xxx to valid host value in /etc/ansible/hosts)

for z/OS 2.5:
ansible-playbook -e 'target_host=xxx' -e 'zosver=25' cbsa.yaml (change xxx to valid host value in /etc/ansible/hosts)

cbsa.yaml includes CBSA configuration as well as testing of the APIs (role: cbsa_test)

If you want to just run the tests then execute: ansible-playbook -e 'target_host=xxx' test.yaml (change xxx to valid host in /etc/ansible/hosts)
