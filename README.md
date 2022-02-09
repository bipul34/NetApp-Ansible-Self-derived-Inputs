# NetApp-Ansible-Self-derived-Inputs
Repository of Ansible Playbooks for NetApp with bare minimum Inputs and maximum self derived values
# NetApp-Ansible 

## Ideology: 
 The basic purpose of this project is to create Ansible Playbooks with maximum automation and minimum inputs . As per my view, most of the playbooks available is using direct inputs from users which can be auto derived wherever possible. Hence i just like to implement this idea to create playbooks which use modules to collect data from existing array and then derived the value as per requirement or best. It help us to put best practices and maintaining a standard . Also i tried to use just ansible  not taking help of PowerShell/python sub scripts or direct ONTAP commands  to get the desired results. 

## Requirement
  Requirement is same with any other NetApp ansible implementation. I am using NetApp Collections . For user input variables , i am not using separate files as they can be either prompted or using survey in Ansible Tower/AWX. I have put comments wherever possible. Please update your inputs in << >> area.

## Request 
   You are free to use this playbooks if you think they are useful (Will be happy if you keep the author line) . Suggestions / Contributions are welcome. Really appreciate sponsorship :)

## Upcoming Tasks 

Day0 build of cluster (build cluster as per requirement post rack and stack by NetApp ) with very few inputs  :  Completed , Check Build directory


Create a Kubernetes pod with persistent storage from netapp via trident CSI 

## Completed Playbooks 

**Vserver.yml** : SVM or vserver build ,This playbook will create a SVM using a best fit aggr to create its root vol . It also enable and start the service as per allowed protocols 
  
   *  As an input we just input name of the vserver and type of aggr that it should use to create its root volume. Vserver root volume can be created in any aggr but here rather than specifying the hard coded aggr name we just mention type (hdd/ssd) and it will select the best aggr as per conditions (More on this on volume.yml)

* Services can be automatically configured and started as per allowed protocols with new service section of the module (ONTAP 9.6+ and REST Api) but i have used handlers to start the required service. CIFS section can be added 

**Lif.yml** : Add a LIF to an existing vserver:  It will just take the vserver name ,vlan  and other mandatory parameters . All other parameters are default (you can override them by state them specifically) . Home node and Home port are derived considering the owner of the root vol containing aggr as node .

 *   It will find the owner of the root vol of the mentioned vserver and set it as home node of the new Lif

*  It will find the suitable port on the home node based the vlan supplied. If multiple port found it will take any one of them. This can be modified to use topologies like primary secondary or vlan less ifgrp etc

* Add default gateway

**Volume.yml** : Add a volume to a vserver : It will create a volume while taking only mandatory user inputs and deriving aggr to be chosen as per below conditions . It will take all the aggr that are allowed for the given vserver and filter as per below conditions

* Non root aggr's
* usages less than 80%
* hdd or ssd as given in input
