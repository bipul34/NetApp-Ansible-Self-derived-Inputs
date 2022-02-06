## Build NetApp Cluster from scratch

This is a list of playbooks which will help build a NetApp cluster from scratch. All we need is to rack and stack the hardware and put an mgmt ip on each node.
## Different scenarios 

I have separated it to two parts depends on how NetApp PS team is involved to install. You can run the playbooks depending on your condition. If NetApp PS team is not involved then you can rack and stack the hardware by your own and add a mgmt ip on the nodes (This needs a physical presence). Then run both Day0_Cluster_Create.yml  and Day0_Cluster_setup.yml. If NetApp PS team involves and they create the cluster for you then you can just run Day0_Cluster_setup.yml. Both this playbook build a Cluster from scratch in matter on minutes with bare minimum inputs if staged properly.

## Note
  * I just used some variable in the playbook , you can define it in variable files, Prompt or Tower/AWX survey
  * The main idea here is to minimum user intervention once this is setup correctly. As a standard environment we have different parameters like ntp,dns etc are fixed for a typical site / datacenters /region. So i have arrange these parameters on variable file. So this variable file will have all the information related different sites ( its better to setup a separate file ) . Once this file is setup , you can just refer it as site name 

  *  
##Playbooks

**Day0_Cluster_Create.yml** : This playbook create the cluster adding all the mentioned nodes once they rack and stack following by configuring node-mgmt ip's

**Day0_Cluster_setup.yml** : This playbook consider cluster is already created and it completed all the Day0 configuration completed by aggr creations. It use the site specific variable file to derive required values 

**Rename_cluster.yml**:  This is an interesting playbook which just take a new cluster name as input  and perform all the activities that need to be changed with cluster name like  node names , node-mgmt lif's,cluster-mgmt lif , root aggr names etc. It derived all required information from the cluster. All we have to provide is the new cluster name. 
