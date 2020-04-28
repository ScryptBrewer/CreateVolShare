# Ansible NetApp Automation
 
Create a new netapp encrypted volume, share, qos, set permissions on share, and NTFS based on AD group. 

## Requirments:
   -  ansible <2.8
   -  dataontap <9.6
   -  python netapp-lib
    
## Required settings:
   - On NetApp Cluster:
     - Create a dediacated admin account ssh,ontapi,console
     - ***set -priv advanced; system services web modify -http-enabled true*** command set on storage

## Required Files:
   - CreateVolumeShare
     - AnsibleVolShare.yml    - Main ansible playbook no modifications required
     - uservars.yml           - Varibles that will need updating with each execution
     - defaultvars.yml        - Default Varibles for volume creation and setup
   - secret.yml               - contians host user credentials for setup of config (optional can use extra vars or setup propmpt in ansible play)

## Basic setup
   - Install prerequisites
   - Configure storage for ansible
   - Clone repo
   - Review the AnsibleVolShare.yml
   - Update secret.yml, uservars.yml, defaultvars.yml


## Usage exaples:
Basic play with all vars in var files

***ansible-playbook AnsibleVolShare.yml***

Remove volume and other things from the play above

***ansible-playbook AnsibleVolShare.yml --extra-vars="state=absent"***

To skip using the NTFS and share hardening use the following

***ansible-playbook AnsibleVolShare.yml --skip-tags=harden***


##### Multiple varibles can be combined to execute a command vs updating each time the play is used.
***ansible-playbook AnsibleVolShare.yml --extra-vars="svm='svm01',volume_share='vol02',aggr='agggr1',vol_gb_size=50,netapp_hostname='Cluster02',netapp_username='Ansible-admin',netapp_password='Ansibe-password',useradmin_acct='AD-UserAdminGroup',usergroup_acct='ADUSERSGRP01',vol_qospolicy='extreme'"***


Note: the NVE encryption flag has been set to true by default. set encryopt="false" if this is not the desired behavor.

If you run into issues with ansilbe use the -vvv flag to show full output 
The top issues in ansible are:
   - syntax errors spaces not tabs!
   - module installation or configuration of storage
   - version compatiblity issues older version of ontap work with some of the features not all. 
