# Ansible NetApp Automation
 
Create a new netapp encrypted volume, share, qos, set permissions on share, and NTFS based on AD group. 

## Requirements:
   -  ansible <2.8
   -  DATAONTAP <9.6
    
## Required settings:
   - On NetApp Cluster:
     - Create a dedicated admin account ssh,ontapi,console
     - ***set -priv advanced; system services web modify -http-enabled true*** command set on storage

## File Descriptions:
   - AnsibleVolShare.yml    - Main ansible playbook no modifications required
   - uservars.yml           - Variables that will need updating with each execution
   - defaultvars.yml        - Default Variables for volume creation and setup
   - secret.yml               - contains host user credentials for setup of config (optional can use extra vars or setup prompt in ansible play)

## Basic setup
   - Install prerequisites
   - Configure storage for ansible
   - Clone repo
   - Review the AnsibleVolShare.yml
   - Update secret.yml, uservars.yml, defaultvars.yml


## Usage examples:
Basic play with all vars in var files

***ansible-playbook AnsibleVolShare.yml***

Remove volume and other things from the play above

***ansible-playbook AnsibleVolShare.yml --extra-vars="state=absent"***

To skip using the NTFS and share hardening use the following

***ansible-playbook AnsibleVolShare.yml --skip-tags=harden***


##### Multiple varibles can be combined to execute a command vs updating each time the play is used.
***ansible-playbook AnsibleVolShare.yml --extra-vars="svm='svm01',volume_share='vol02',aggr='agggr1',vol_gb_size=50,netapp_hostname='Cluster02',netapp_username='Ansible-admin',netapp_password='Ansibe-password',useradmin_acct='AD-UserAdminGroup',usergroup_acct='ADUSERSGRP01',vol_qospolicy='extreme'"***


Note: the NVE encryption flag has been set to true by default. set encrypt="false" if this is not the desired behavior.

If you run into issues with ansible use the -vvv flag to show full output 
The top issues in ansible are:
   - syntax errors spaces not tabs!
   - module installation or configuration of storage
   - version compatibility issues older versions of ontap work with some of the features not all. 
